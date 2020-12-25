# Objectives

- understand the method of **least squares** for parameter and state estimation
- apply the **linear Kalman filter** and its nonlinear variants, the **extended** and **unscented Kalman filters**, to state estimation problems
- develop models for typical localization sensors like GPS receivers, inertial sensors, and LIDAR range sensors
- learn about **LIDAR scan matching** and the **iterative closest point (ICP)** algorithm
- use these tools to **fuse data from multiple sensor streams** into a single state estimate for a self-driving car

# Importance

Motivation

- where am I?
- how fast am I moving?

Why is it hard: sensors and measurements are imperfect

# Definitions

**Localization**: process of determining position and orientation of a vehicle

- How? E.g. state estimation

**State Estimation**: computing [the most likely value of] a physical quantity (e.g. vehicle position) from a set of [noisy] measurements 

- related concept: **Parameter Estimation**
- unlike states (e.g. position and orientation of a vehicle), parameters (e.g. a resistor's resistance) are constant over time [i.e. are not expected to change from the first to the last measurement]