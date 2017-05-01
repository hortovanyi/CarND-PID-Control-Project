# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program
---
![75 MPH speedster](https://github.com/hortovanyi/CarND-PID-Control-Project/blob/master/speedster75mph-small.png?raw=true)

##PID Controller

For this project I used two PID controllers. One to control the steering angle and the other the throttle. The first lap or two,   the Cross Track Error (CTE) for the steering angle is higher then later laps.

I created a simple set of if tests, to find a desired speed, based on an average of the last 20 absolute steering angles used. The idea being that the straighter a car is driving, the higher the speed. The simulator is limited to 100 MPH. I was able to reach high 70 MPH speeds. On the rare occasion this configuration as is just hits 80 MPH. 

If the absolute steering CTE is closer to zero i.e. the vehicle is on path, then the desired speed is increased. As the absolute CTE increases the desired speed is decreased. I found if I tried to break too quickly, then it induced oscillation that was often not correctable.

To create a CTE for the throttle I used the existing speed - desired speed as input to the throttle PID controller update error function. The PID as initialised induced both breaking and acceleration. My first attempts, had no breaking, just acceleration and the car would land in the lake. The current settings provide a nice balance whilst keeping the vehicle on track.

The steering PID class was initialised with the following values
```
double Kp = 0.1;  // proportional coefficient
double Ki = 0.003;  // integral coefficient
double Kd = 3.0;  // differential coefficient
```

The values themselves were found by manual tuning based initially on the values from the lessons.

A larger value for Kp caused high proportional gain and oscillation. With Kp = 0.9 [this video shows](https://www.youtube.com/watch?v=44jhBRV-m3k) how eventually the oscillation caused the vehicle to crash on the hard left turn.

A smaller value for Kd < 1.0, caused low differential gain i.e. the PID controller was underdamped. The oscillation was severe and caused the vehicle to leave the track shortly after starting [per this video](https://www.youtube.com/watch?v=AEbfu3abMLA).   

Larger integral coefficient > ~0.003, did not result in much forward movement. I did not create a video, if you set it to 0.1 shortly after starting, the car will circle and not drive straight down the road.

[Full Video of a few laps Kp = 0.1, Ki = 0.004, Kd = 3.0](https://www.youtube.com/watch?v=-DYt5eV8ZqQ)


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
* [uWebSockets](https://github.com/uWebSockets/uWebSockets) == 0.13, but the master branch will probably work just fine
  * Follow the instructions in the [uWebSockets README](https://github.com/uWebSockets/uWebSockets/blob/master/README.md) to get setup for your platform. You can download the zip of the appropriate version from the [releases page](https://github.com/uWebSockets/uWebSockets/releases). Here's a link to the [v0.13 zip](https://github.com/uWebSockets/uWebSockets/archive/v0.13.0.zip).
  * If you run OSX and have homebrew installed you can just run the ./install-mac.sh script to install this
* Simulator. You can download these from the [project intro page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/aca605f8-8219-465d-9c5d-ca72c699561d/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562) in the classroom.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./
