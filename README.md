# Alpha Swarm Algorithm: Specification and Verification in NuXMV and GROOVE
This repository provides implementations of a simplified Alpha swarm aggregation algorithm in NuXMV and GROOVE for unbounded and bounded model checking.

## Alpha Algorithm
The Alpha Algorithm (a.k.a connection degree algorithm) [1] aims to guarantee that a given network of robots, capable only of short-range wireless communications, any robot which gets disconnected from the swarm would eventually return to the swarm.

[^1] Julien Nembrini. 2005. Minimalist Coherent Swarming of Wireless Networked Autonomous Mobile Robots. Ph.D. Dissertation. 

## Software
**NuXMV**: Install [NuXMV] (https://nuxmv.fbk.eu/download.html)<br />
**GROOVE**: Install [GROOVE] (https://groove.cs.utwente.nl/installing.html)<br />

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
(time -cp ../../../bin/Simulator.jar nl.utwente.groove.ModelChecker -ctl "AG(AF(connectedSwarm))" <path-to-groove-gps-folder> > <path-to-save-output>;) 2> <path-to-save-duration>
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
2. Run the following commands:
```
cd <groove-path>/bin/
(time java jar Generator.jar -a cycle -r 1000 -s ltl: "AG(AF(connectedSwarm))" <path-to-gps-folder> > <path-to-save-output>;) 2> <path-to-save-duration>
```

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





