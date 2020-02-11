---
title: Pylastica
category: Software
order: 2
---

The python implementation of elastica is the easiest version to get started with. This page contains useful information for getting elastica set up, using it to model single and multiple rod systems, and postprocessing the results. 

#### [Getting Started](#getting-started)  
##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; -[Installation](#installation)
##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; -[Pylastica Workflow](#pylastica-workflow)
##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; -[Useful Information](#useful-information)
##### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; -[Rules of Thumb](#rules-of-thumb)
#### [Tutorials](#tutorials)  
#### [Postprocessing/Visualzation](#postprocessing-and-visualzation)  

<br /> 

# Getting Started
## Installation
Pylastica uses Python 3, which needs to be installed prior to using Pylastica. For information on installing Python, see [here](https://realpython.com/installing-python/). If you are interested in using a package manager like Conda, see [here](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html).

>The easiest way to install Pylastica is with PIP: `pip install pylastica`

You can also download the source code for Pylastica repo directly from GitHub. 

## Pylastica workflow
This is a description of how you would generally set up and excecute an elastica simulation. 
#####  import modules 
    a. wrappers
        i.  base system
        ii. connections
        iii.constraints
        iv. forcing
        v.  callbacks
    b.  rod 
    c.  boundary conditions
        i.  free or fixed ends
        ii. joints
        iii. external forcing
    1.  gravity
    2.  torque (internal?)
        iv. friction and/or slender body forces
    d.  Call backs
    e.  Time stepper
    f.  Integrate

#####  Define simulator class
    a.  Inherits imported wrappers

#####  Define parameters for each rod 
    a.  Apply parameters to rod objects
    b.  Do you always start with a straight rod?

#####  Define boundary conditions and connect rods together
    a.  Constrain
    b.  Connect
    c.  add_forcing_to

#####  Create callback function (optional)
    a.  What to output
    b.  How often to output

#####  Finalize
    a.  simulator.finalize()
    b.  What does this actually do?

#####  Define time stepper
    a.  Final time & dt
    b.  Verlet (other options?)

#####  RUN 
    a.  integrate(timestepper, fixed_joint_sim, final_time, total_steps)

#####  Post-process
    a.  Get data from call-backs
    b.  Visualization (POVray?)

## Useful information
Indicate what parameters are defined per node (area), per element (moduli) and per simulation (time-step). 

Link to documentation (TBD).

## Rules of Thumb
List of general rules to help users immediatly build some intutition. For example, what sort of dx and dt should you be using. Run times and HPC questions. 

    1. Number of elements (dx)
    2. Timestepper (dt)
    3. Stability concerns? dx~dt?
    3. Run time estimation
        i.   scaling with increased dx and dt
        ii.  scaling with adding rods
        iii. parallization/HPC
    4. Call back timing
        i.   use for output - time needed for videos
        ii.  storage requirements
        iii. memory or i/o to disk

# Tutorials
Either walkthroughs of the different examples, of link to other websites that can host the code. Possibly [Binder](https://mybinder.readthedocs.io/en/latest/index.html) if we convert the examples we have to jupyter notebooks. 


# Postprocessing and Visualzation
How to access data from the callback functions. 

How to visualize data (POVray?).