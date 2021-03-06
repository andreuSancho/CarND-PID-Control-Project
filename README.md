# PID Controller
Self-Driving Car Engineer Nanodegree Program

---
## Introduction

To safely take control of the autonomous car steering wheel, the Proportional Integral Derivative (PID) controller is presented. This simple, yet effective algorithm computes proportional, integral, and derivative responses to compute the final steering wheel angle [1]. The purpose of this project is to implement a PID controller in C++ and demonstrate its effectiveness in a simulator.

## Rubric points

In what follows the rubric points are discussed. Recall that we use the Cross-Track Error (CTE) as the measure of wellness. It computes the distance from the lane center.

### P, I, and D parts of the PID controller

**P, the proportional gain.** This coefficient causes the vehicle to steer proportional to the vehicle distance to the lane center (the aforesaid CTE). Therefore, the angles obtained can be very large and abrupt if CTE is high, and may the car oscillate wildly, turning from one side to another. Experimentally, low values (that is: less than 0.5) are preferred to avoid the sudden turns.

**D, the differential gain.** Differently from P, this coefficient is proportional to the rate of change of the CTE. This component balances (that is: decreases) the steering wheel angle if it is changing abruptly. Therefore, if P is set to high values, D helps by smoothing the behavior of the turns.

**I, the integral gain.** This coefficient sums the error term (that is: the CTE) over time. This way biases in steering wheel angle can be mitigated.

### Description of how the hyperparameters were chosen
Parameters were chosen experimentally by performing a grid search. First, the Kp was set to an initial 0.3 and Kd to 6 and the remaining parameter to zero. The car couldn’t drive safely, so in the following iterations Kp was decreased by 0.01, while Kd was increased a unit. After a few iterations, the car could drive safely (albeit the oscillations). Next, the Ki parameter was set to 0.001 and later to 0.002. As similar results were reported, the hyperparameter search was stopped at this point.
The final values are the following:
- Kp = 0.22
- Ki = 0.002
- Kd = 14.0

It is important to highlight that the final steering angle is filtered in the following way: 
*steer_value = -pid.TotalError() * 0.7 + angle * 0.3*
where *angle* is the previous steering wheel angle in radians.

Also, a simple controller for the throttle is implemented in the following way:
*throttle = (60.0 - speed) * 0.01*
where *60.0* is the maximum speed that we want to achieve and *0.01* is a normalizing constant. Other speed maximums have been tested but this particular value makes the ride of the car safer.

See the video here: 

[![Result](https://img.youtube.com/vi/QComZ1jBYkI/0.jpg)](https://youtu.be/QComZ1jBYkI)

## Note on using the code in Windows.

Windows users have to do a bridge between the Virtual Machine (VM) with Linux and the Windows Host. To do so, open the console as administrator and type the following:

`netsh interface portproxy add v4tov4 listenport=4567 listenaddress=127.0.0.1 connectport=4567 connectaddress=<LINUX_IP> protocol=tcp`

where *<LINUX_IP>* is the IP address of the Linux VM machine.

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

## External Links
[1] http://www.ni.com/white-paper/3782/en/
