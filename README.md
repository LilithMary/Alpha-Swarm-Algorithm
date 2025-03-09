# Alpha Swarm Algorithm: Specification and Verification in NuXMV and GROOVE
This repository provides implementations of a simplified Alpha swarm aggregation algorithm in NuXMV and GROOVE for unbounded and bounded model checking.

## Alpha Algorithm
The Alpha Algorithm (a.k.a connection degree algorithm) [1] aims to guarantee that a given network of robots, capable only of short-range wireless communications, any robot which gets disconnected from the swarm would eventually return to the swarm.

Each robot in the swarm moves forward by default, periodically sending out "Are you there?" messages to detect nearby robots. If a robot finds that the number of its neighbours falls below a certain threshold ```alpha```, it assumes it is moving out of the swarm. Therefore executes a 180-degree turn and continues to move forward. Once it regains enough neighbours, it performs a random turn to prevent the swarm from collapsing in on itself and resumes exploring. This algorithm relies on local wireless connectivity information to achieve swarm aggregation and maintain cohesion​​.

We have applied the following simplifications to the Alpha algorithm to improve scalability of verification:
1. Wraparound grid instead of an infinite grid
2. Synchronous movement
3. Alpha parameter set to 1
4. Detection range set to 1
5. Avoidance range set to 1
6. Cadence set to 1

The NuXMV version of the code along with the wraparound grid and concurrency modelled as synchrony were based on [2].

[1] Julien Nembrini. 2005. Minimalist Coherent Swarming of Wireless Networked Autonomous Mobile Robots. Ph.D. Dissertation. 

[2] Clare Dixon, Alan FT Winfield, Michael Fisher, and Chengxiu Zeng. 2012. Towards temporal verification of swarm robotic systems. Robotics and Autonomous Systems 60, 11 (2012), 1429–1441.

## Software
**NuXMV**: Install [NuXMV] (https://nuxmv.fbk.eu/download.html)<br />
**GROOVE**: Install [GROOVE] (https://groove.cs.utwente.nl/installing.html)<br />

To use the GROOVE ```ModelChecker``` (as instructed further below):
1. Download the GROOVE source code from here: https://github.com/nl-utwente-groove/code/releases <br />
2. Unzip it and copy the ```nl``` folder in the GROOVE path (in the same folder where ```bin``` and ```lib``` subfolders are).

## Experiments
For each of GROOVE and NuXMV, we have provided a sample code for grid length 4 with 3, 4, and 5 robots. Below are guidelines for running the code on Linux. 

### Run Sample Codes

#### NuXMV
```
cd <nuxmv-path>/bin/
(time ./nuxmv <path-to-smv-code> > <path-to-save-output>;) 2> <path-to-save-duration>
```

#### GROOVE
1. Inside the gps folder, modify the ```system.properties``` file to set ```location=``` to the path of the gps folder in your system.
2. Run the following commands for 3 robots:
```
cd <groove-path>/nl/utwente/groove
(time java -cp ../../../bin/Simulator.jar nl.utwente.groove.ModelChecker -ctl "AG(AF(connectedSwarm))" <path-to-groove-gps-folder> > <path-to-save-output>;) 2> <path-to-save-duration>
```
For 4 and 5 robots, replace ```"AG(AF(connectedSwarm))"``` with ```"AG(AF(path | star))"``` and ```"AG(AF(path | star | vbranch))"``` respectively.

### Modify Sample Codes
Below are guidelines for conducting bounded model checking and changing the initial locations of robots. 

#### Bounded Model Checking

##### NuXMV
1. Inside the SMV file, change the CTLSPEC to LTLSTEP and modify the formula accordingly as shown below:
   Replace  ```CTLSPEC AG (AF connectedSwarm);``` with ```LTLSPEC G (F connectedSwarm);```.
2. Run the following commands which sets the bound to 20 and disables generation of a counterexample via ```-dcx```:
   ```
   cd <nuxmv-path>/bin/
   (time ./nuxmv -bmc -bmc_length 20 -dcx <path-to-smv-code> > <path-to-save-output>;) 2> <path-to-save-duration>
   ```

##### GROOVE
1. Inside the gps folder, modify the ```system.properties``` file to set ```location=``` to the path of the gps folder in your system.
2. Navigate to the ```bin``` folder where the GROOVE jar files are:
```
cd <groove-path>/bin/
```
3. Run the following command:
```
(time java -jar Generator.jar -a cycle -r 1000 -s ltl:"G(F(connectedSwarm))" <path-to-gps-folder> > <path-to-save-output>;) 2> <path-to-save-duration>
```
This will explore 1000 cycles of the simulation and provide a counterexample in the form of a sequence of state IDs. <br />

The following variation of the command (with extra flags ```-f``` and ```-o```) provides detailed information about the states and transitions labeled with corresponding rules:

```
(time java -jar Generator.jar -a cycle -r 1000 -s ltl:"G(F(connectedSwarm))" -f <path-to-save-states>/'state-#.gxl' -o <path-to-save-transitions>/'labeled_transitions.gxl' <path-to-gps-folder> > <path-to-save-output>;) 2> <path-to-save-duration>
```
With this information, the counterexample can be decoded into a sequence of states with rule names marking transitioning from one state to the next. 

#### Initial Robot Locations


##### NuXMV
1. From the initial coordinations file with the number of robots of your choosing, select an initial configuration for the robots. Copy the corresponding code starting with ```VAR``` and ending before a dashed line. Open the SMV sample code with the same number of robots and replace the section at the beginning of ```MODULE main``` starting with ```VAR``` and ending before ```DEFINE``` with the code you copied previously. Save the file.
2. Run commands provided in previous sections. 


##### GROOVE
1. Select a start configuration from the provided initial configurations and copy it into the gps folder containing the sample codes. The number of robots must match, e.g. if the grammar in the gps folder has 3 robots, the start graph must be selected from those with 3 robots.
2. Inside the gps folder, modify the ```system.properties``` file to:
    1. Set ```location=``` to the path of the gps folder in your system.
    2. Set ```startGraph=``` to the start graph file of your choosing (without the gst extension).
3. Run commands provided in previous sections.





