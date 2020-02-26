---
title: Workflow
category: Pyelastica
order: 4
---

## Pyelastica workflow
When using Pyelastica, most users will setup a simulation in which they define a system of rods, define initial and boundary conditions on the rods, run the simulation, and then post-process the results. For this case, the typical outline for using Pyelastica would be:

####  1. import necessary modules 
There are several different modules from Elastica that need to be imported. They can be broadly classified as:  
&ensp; a. wrappers  
&ensp; b. system conditions (i.e. rods and boundary conditions)  
&ensp; c. call backs (for saving state information during simulation)  
&ensp; d. timestepper  
>See the examples folder for a list to typical import statements. Future implementations will work to simplify this step.

####  2. create simulator
We need to define an object that will contain the system we are about to create. To do this we define a simulator class that inherits the necessary attributes from the wrappers we previously imported. We will add rods and different boundary conditions to this class to create our system and pass it to the timestepper to be solved. The most generic simulator class is:  
```
class SystemSimulator(BaseSystemCollection, Constraints, Connections, Forcing, CallBacks): 
    pass 
```
This simply combines all the wrappers previously imported together. If a wrapper is not needed for the simulation, it does not need to be added here (i.e. if only one rod, you do not need to include the `Connections` class).

#####  3. define parameters for each rod 
Each rod has a number of physical parameters that need to be defined. These values then need to be assigned to the rod to create the object and the rod need to be added to the simulator. 
```
# Create rod
rod1 = CosseratRod.straight_rod(
    n_elem,        # number of elements
    start_rod,     # Starting position of first node in rod
    direction,     # Direction the rod extends
    normal,        # normal vector of rod
    base_length,   # original length of rod (m)
    base_radius,   # original radius of rod (m)
    density,       # density of rod (kg/m^3)
    nu,            # Energy dissipation of rod
    E,             # Elastic Modulus (Pa)
    poisson_ratio, # Poisson Ratio
)

# Add rod to SystemSimulator
SystemSimulator.append(rod1)
```
This can be repeated to add multiple rods to the system. Be sure to remember to change the name of the rods (rod1 -> rod2) and the starting location for each rod along with the rods physical properties as needed. 

####  4. define boundary conditions, forcings and connections
Now that we have added all our rods to `SystemSimulator`, we need to apply the relevant boundary conditions. See the documentation and tutorials for in depth explanations of the different types of forcings available. 

As a simple example, to fix one end of a rod, we use the `OneEndFixedRod` boundary condition (which we imported in step 1) and apply it to the rod. Here we will be fixing the 0th node as well as the 0th element. 
```
SystemSimulator.constrain(rod1).using(
    OneEndFixedRod,                 # Displacement BC being applied
    constrained_position_idx=(0,),  # Node number to apply BC
    constrained_director_idx=(0,)   # Element number to apply BC
)
```
We have now fixed one end of the rod while leaving the other end free. We can also apply forces to free end using the `EndpointForces`
```
#Define 1x3 array of the applied forces
origin_force = np.array([0.0, 0.0, 0.0])
end_force = np.array([-15.0, 0.0, 0.0]) 
SystemSimulator.add_forcing_to(rod1).using(
    EndpointForces,                 # Traction BC being applied
    origin_force,                    # Force vector applied at first node
    end_force,                      # Force vector applied at last node
    ramp_up_time=final_time / 2.0   # Ramp up time 
)
```
We can also add more complex forcings, such as friction, gravity, or torque throughout the rod (see tutorials and documentation for details). One last condition we can define is the connections between rods. 
```
# Connect rod 1 and rod 2. '_connect_idx' specifies the node number that 
# the connection should be applied to. You are specifying the index of a 
# list so you can use -1 to access the last node. 
SystemSimulator.connect(
    first_rod  = rod1, 
    second_rod = rod2, 
    first_connect_idx  = -1, # Connect to the last node of the first rod. 
    second_connect_idx =  0  # Connect to first node of the second rod. 
    ).using(
        FixedJoint,  # Type of connection between rods
        k  = 1e5,    # Spring constant of force holding rods together (F = k*x)
        nu = 0,      # Energy dissipation of joint
        kt = 5e3     # Rotational stiffness of rod to avoid rods twisting
        )
```

#####  5. create callback functions (optional)
If you want to know what happens to the rod during the course of the simulation, you must create a callback function to output the data you need as the simulation runs. There is a base class `CallBackBaseClass` that can help with this. If you do not define a callback function, then at the end of the simulation, you will only have the final state of the system available.  
```
# MyCallBack class is derived from the base call back class.   
class MyCallBack(CallBackBaseClass):
    def __init__(self, step_skip: int, callback_params):
        CallBackBaseClass.__init__(self)
        self.every = step_skip
        self.callback_params = callback_params
    
    # This function is called every time step
    def make_callback(self, system, time, current_step: int):         
        if current_step % self.every == 0:
            # Save time, step number, position, orientation and velocity
            self.callback_params["time"].append(time)
            self.callback_params["step"].append(current_step)
            self.callback_params["position" ].append(system.position_collection.copy())
            self.callback_params["directors"].append(system.director_collection.copy())
            self.callback_params["velocity" ].append(system.velocity_collection.copy())
            return

# Create dictionary to hold data from callback function
callback_data_rod1, callback_data_rod2 = defaultdict(list), defaultdict(list)

# Add MyCallBack to SystemSimulator for each rod telling it how often to save data (step_skip)
SystemSimulator.collect_diagnostics(rod1).using(
    MyCallBack, step_skip=1000, callback_params=callback_data_rod1)
SystemSimulator.collect_diagnostics(rod2).using(
    MyCallBack, step_skip=1000, callback_params=callback_data_rod2)
```
You can define different callback functions for different rods and also have different data outputted at different step intervals depending on your needs.

#####  6. finalize system, define time stepper and run simulation
Now that we have finished defining our rods, the different boundary conditions and connections between them, and how often we want to save data, we have finished setting up the simulation. We now need to finalize the simulator by calling `SystemSimulator.finalize()`. This goes through and collects all the rods and applied conditions, preparing for the simulation. 

With our system now ready to be run, we need to define which timestepping algorithm to use. Currently, we use the position Verlet algorithm. We also need to define how much time we want to simulate as well as either the time step (dt) or the number of total time steps we want to take. Once we have defined these things, we can run the simulation by calling `integrate()`, which will start the simulation. 
```
timestepper = PositionVerlet()
final_time = 10   # seconds
dt = 1e-5         # seconds
total_steps = int(final_time / dt) 
integrate(timestepper, SystemSimulator, final_time, total_steps)
```

#####  7. post-process
Once the simulation ends, it is time to analyze the data. If you defined a callback function, the data you outputted in available there (i.e. `callback_data_rod1`, otherwise you can access the final configuration of you system through your rod objects. For example, if you want the final position of one of your rods, you can get it from `rod1.position_collection[:]`. 

# Visualization
If you wish to visualize your system, make sure you define your callback function to output all necessary data. You can either plot your data using a python package such as matplotlib, or any rendering software that you choose. 
>For high-quality visualization, we suggest [POVray](http://povray.com). See [this tutorial](link-to-POVray-tutorial) for examples of different ways of visualizing the system. 

\\
[back to top](./)