# Testing Self-Driving Car Software Contest

## Scenario/Road Generation

### Introduction
This task is about generation of virtual roads to test lane keeping assist function of the test subject(s). A scenario consists of an ego-car equipped with a test lane keeping assist system, a road, a driving task and the environment definition.  The driving task is to drive from road start to road end without going astray

### Goal
Force the test subject to drive of the lane, without creating invalid roads (See below)


### Road structure
Roads are defined by a (ordered) sequence of coordinates in a two-dimensional space, i.e., (x, y). Each point corresponds the central line of the road, a.k.a., *road spine*. 

The first point in the sequence defines the starting location the last one the target location.

> **NOTE**: the initial placement and rotation of the ego-vehicle is automatically defined.

Test generators must generate sequences of coordinates, i.e., *road points*, that define the overall geometry of the roads. The road points are automatically interpolated using cubic splines to obtain the final road geometry.

The following image illustrates how a road is rendered from the following *road points*: 
`[(10,20), (30, 20), (40, 30), (50, 40), (150, 100), (30, 180)]`

![Sample Road caption="test"](./figures/road_sample.pdf "Sample Road")

In the figure, the inner square identifies the boundary of the map (200x200), the white dots are the *road points*, the yellow solid line is the *road spine* that interpolates them, and, finally, the gray area is the road.

As the figure illustrates, the road layout consist of one left lane and one right lane (where the car drives). Each lane is 4 meter wide and the lane markings are defined according to the US standard (solid yellow line in the middle and solid white lines on the side).

> **Note**: road material, slope, layout, and environmental factors such as weather and time-of-day are fixed and predefined  (daylight, no rain, clear-sky, green grass, no curbs, cement/asphalt/concrete). 


### Validity checks
Roads must be valid, and we perform a series of validity check before executing a test. An invalid input results in an invalid (not failed) test.

To keep things simple we do no allow intersections (geometry check already implemented) or overlapping. Turns must have a geometry that allows car to drive on them, so "too" sharp edges are disallowed (limit on curvature). To limit the length of road, roads must be completely within a predefined squared map, so we enforce a map limit (box of given size, the road should not cross the limits), and have a maximum length. Also to avoid trivial tests, roads have a minimum length. (TODO Are we sure about those? What are those limits? Min must ensure the car fits there, but max?)

To avoid overly complex roads we also limit the points that can be used to define them (500/1000 points). Notably, the points defining the roads are interpolate using cad-mul splines. For the same reason, we might enforce a minimum distance between consecutive points.


### Competition
The contest's organizers provide a library to check the validity of the tests, to execute them, and to keep track of the time budget. Execution can be mocked or simulated. Simulation requires  registering and installing the simulation software, BeamNG.tech which runs only under windows (see simulation).

To ease the integration of various components, the organizers provide a python 3.7 library and submission should integrate with them. Hence, we expect python submissions. (TODO how can we integrate other technologies? Do we want that?)

Competitors must create roads to test lane keeping assist function of the test subject(s) and submit them in the agreed format. The generation will continue until a given time budget is reached. The time budget includes time for generating and simulating tests.

During development contestant can either use BeamNG.tech to simulate executions or mock them. If the mock is used, an approximation of the simulation time will be computed. There's no limit on the number of test that can be generated and executed. The time budget will be up to one hour.


### Results
To evaluate submissions we will follow those guidelines:
- count how many valid/invalid tests have been executed
- how many tests passed/failed. A test fail if the test subject does not reach the end of the road within a timeout (computed over the length of the road) or drives off the lane/road
- input diversity and feature coverage (measures only on the roads)
- output diversity and feature coverage (measures on the test subject)

TODO: define coverage and metrics in broad terms, do not release the code to compute them, or not? Why yes/no?

The test subjects used for the evaluation will be not released before the submission deadline to avoid biasing the solution towards it. Mock test subjects are provided with the package (simulation and mock execution).

The evaluation will be conducted using the same simulation, BeamNG.tech, and setup provided to the contestants.

### Random Generation
The submission package comes with an implementation of a random test generator. This serves the dual purpose of providing an example on how to use the library, and a baseline for the evolution.

# Installation
Check the [Installation Guide](documentation/INSTALL.md)

# Technical considerations
contest - 
    command line interface based on CLI to get time budget, map size
    configure the executor (validity checks)
    load dynamically the test generator (predefined name, check plugin style)
    instantiate the generator with the executor and start it
    
>> How to stop it?

test generator 
    - constructor, executor + additional parameters
    - start

executor
    - execute tests [test] -> [time, simulation data, status (pass/fail)

    



