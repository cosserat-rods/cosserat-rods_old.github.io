---
title: Elastica
category: Software
order: 0
---

## About Elastica  
Elastica is a software tool designed to numerically solve systems made up of collections of Cosserat rods, which can be used to analyze a wide range of both natural and artificial systems ranging from single muscle fibers to birds nests to the next generation of soft robots. Details on the numerical method used can be found [here](../../2_numerics/numerics/).

## Features
Elastica has been designed to be modular, extensible and easy to use. It allows the user to define a collection of Cosserat rods subject to both external (i.e. gravity, friction, etc...) and internal (i.e. muscle torque) forces. Rods account for self contact and can be combined to create collections of rods which can then be used to model increasingly complex system.

## Implementations
>There are currently two versions of Elastica. 

[**Pyelastica**](../users) is a python implementation of Elastica. It is the most user friendly version and we encourage new users to start here. We are actively developing it and continually adding new features and performance improvements. 

[**Elastica++**](https://github.com/mattialab/elastica) is a C++ implementation of Elastica. This is an older version of Elastica which is less documented and developed. It is currently faster than Pyelastica, and so may be necessary for larger projects, however, we encourage users to start with Pyelastica and only use this implementation if necessary. We hope to in the future update this version to be inter-operable with Pyelastica, allowing improved performance. 

