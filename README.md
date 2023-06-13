## Project Description

This is a simulation of a drone and robot, pickup and dropoff system. There is a map of the University of Minnesota campus that will be populated by drones that can be scheduled to take robots from one point to another based on several different routing algorithms.

This project was meant to be practice for implementing different software design patterns, working in a large codebase, extending the codebase with well designed scalable features, as well as working iteratively and incrementally. 

## How to Run:

Here is an overview of how to run the project (If you are using ssh, navigate to ssh category below):

    ```bash
    # Go to the project directory
    cd /path/to/repo/project
    
    # Build the project
    make -j
    
    # Run the project (./build/web-app <port> <web folder>)
    ./build/bin/transit_service 8081 apps/transit_service/web/
    ```
    
Navigate to http://127.0.0.1:8081 and you should see a visualization.

Navigate to http://127.0.0.1:8081/schedule.html and you should see a page to schedule the trips.

*Note: 8081 will depends on what port you used. If you use port 8082, then it will be http://127.0.0.1:8082 instead.*

Important: You have to start the ssh clean if you would like to use the commands below. This means, if you are currently using vscode and already login via ssh, then you cannot run the commands below.

SSH into a CSE Lab Machine using port forwarding for the UI

Note: If port 8081 is not available, choose a different port (e.g. 8082, 8083, etc...)

'ssh -L 8081:127.0.0.1:8081 x500@csel-xxxx.cselabs.umn.edu'


## What the Simulation Does:

The simulation shows a map of the University of Minnesota campus populated with different entities (Robots, Drones, Humans, and Helicopters). The main acting entities are robots and drones where the user can schedule trips from one point to another and the closest available drone will pick it up and complete that trip. 

Getting to the Robot is always completed with a beeline routing strategy to be as direct as possible, the route the drone travels with the robot can be specified by the user from the options of Dijkstra, A-star, and DFS when scheduling the trip.

You can always create new robots by scheduling new trips on the schedule.html page and create new humans with a button at the bottom of the same page. To make more drones or helicopters, you must change the umn.json to configure the starting state of the simulation.

On the simulation page, you can adjust the simulation speed as well as select an option to view all possible routes around the campus. These are all the routes the simulation will consider in conducting the search algorithms for the drone's travel path. You can also pan around the map as well as follow various entities as they traverse the map.

As discussed further below, you can also select to opt in or out of drone notifications and view some data analysis upon the simulations completion.

## New Features

### #1 Notifications

**What does it do?**

The notification feature allows the user to opt in and out of messages that the drones are sending through their runtime. It's basic information about what trips they're going on and what steps they're currently on but can be useful for understanding the simulation and keeping track of everything.

**Why is it useful?**

With the notifications on, you can more easily keep track of the simulation when a large number of drones are on the map. You can see which Robots have been picked up and which ones will soon. You also get some information on how long it will take for a drone to get to it's next robot for pickup. Separately, the ability to dynamically opt in and out of notifications allows the user more flexibility in what they want to use the simulation for. If notifications are in the way, simply turn them off.

**How does it add to the work?**

The existing work provides no way of tracking specific drones, besides selecting to follow them individually, which would be tedious. With the notifications, you can now easily see which trips have been taken care of and which have yet to be handled or begun. This

**Which design pattern?**

The notification feature was implemented with the observer design pattern. We chose this because it allows an easy setup for allowing an observer (the user) to observe subjects (the drones). The observers can also easily opt out of these notifications without affecting whether or not the subjects send out notifications to other observers that may still want them. This design pattern also is very scalable and other such notification systems could be developed off of it in this simulation.

**How to use?**

To use the notification feature, you need only run the simulation and navigate to http://localhost:{port} and toggle on or off the notifications selector. From here, the simulation will do the rest.

The notifications feature allows the user to better keep track of trips when many drones are flying around at once. There will be 4 buttons off to the side, more or less if you alter the code, that represent the 4 drones in the system. These represent 4 different subjects that you as the user can opt in and out of getting notifications from. If the box is checked, you will receive notifications from the drone and if not, you won't. The notifications will show up on the left hand notification bar and the newest notifications will show up at the top. Finally, there is a button to clear the notification bar if it gets too cluttered.

### #2 Data Collection and Analysis

**What does it do?**

The data collection and analysis feature tracks various statistics about all of the drones within the simulation. The feature will automatically track all statisics and can be exported to a csv file from the schedule page of the drone simulation at any time. Additionally, there is a python script that will then use the generated csv file to automatically create bar and pie chart visualizations of the data.

**Why is it useful?**

This feature is useful for several reasons. First, it can give you basic information about whether the simulation is working or not. For instance, if there is a drone who doesn't complete any pickups or dropoffs then there may be something wrong. Additionally, this feature could be used to gain insights into various aspects of the simulation. One could look to see whether drone speed has a proportional impact on travel time or number of pickups/dropoffs. The statistics could also be used to gather information on average trip distance/time which could be used to better optimize drone scheduling or route finding. 

**How does it add to the work?**

The existing work provides no way of retroactively determining what happened in the simulation. By adding a way of tracking and analyzing statistics, there is now a way to learn from the simulation. Without being able to analyze the simulation there is little practical use for it. However, the simulation could now be used to better understand how a drone delivery service around the U of M campus might behave. 

**Which design pattern?**

The data collection feature was implemented with the singleton design pattern. We chose this design pattern because it creates a single easily accessible point in the simulation that is responsible for storing the collected statistics. This is important because there will be multiple drones that all need to have statistics stored that should not overwite/conflict with each other. By having one consistant spot for statistics it ensures the data remains correct and it also makes it easier to export the data when the time comes since it is all stored in one spot. Moreover, if we were to add stat collection for more entities in the future, the singleton would make it easy since those entities could use the same singleton that is used for drone data collection.

**How to use?**

The data collection feature is always collecting date on the simulation as it is running. To export the statistics into a csv file simply press the "Export to CSV" from the schedule page found at http://localhost:{port}/schedule.html. The csv file will be generated and stored in the projects root directory with a name follwing the pattern simulationStats{timestamp}.csv. To automatically generate the data visualizations, simply run 'python3 stat_visualization.py {csv filename}. The graphs will be automatically saved into two seperate png files in the root directory of the project.

## Sprint Retrospective

### What went well:

The tasks we laid out at the beginning of the sprint were manageable and attainable. 
We were able to effectively delegate those tasks among us and everyone was able to work on the parts of the project that most excited them. 
We were helpful in unblocking each other on difficult tasks and felt comfortable asking for help.

### What could have gone better:

We could have communicated more consistently about where each other were at in their tasks.
We could have laid out clearer expectations for what each task implied so as to not overlap with eachother's work or come up short on what others thought we would do.
Our deadlines were fairly long term and more intermediate ones would have helped the project progress more consistently rather than sporatically at the end of the Sprint.

## UML Link

Visual representation of how the code of the new features are structured.

![alt text](UML.png "UML")

[UML Diagram](https://github.umn.edu/umn-csci-3081-S23/Team-010-39-homework4/blob/main/HW4%20UML%20(1).pdf)

## Docker Link

A container you can run the program from without needing to install all the required dependencies. All the fun of drone simulations, none of the hassle!

To run the docker image, run

 ```bash
    docker run --rm -it -p 8081:8081 phunner/drone_simulation
```

If you want to run the stat_visualization.py file from the docker container, you can use the following two commands

```bash
   docker exec -t -i {container_id} /bin/bash
   python3 stat_visualization {csv file}
```

You can then use the docker cp command to copy the visualizations to your local machine for inspection if desired:

```bash
   docker cp {container_id}:{visualization file path} {desired local path}
```

[Docker Repo](https://hub.docker.com/repository/docker/phunner/drone_simulation/general)

## Youtube Link

Video overview of the simulation and its features.

[Youtube overview](https://youtu.be/JWbUGhDRIxE)
