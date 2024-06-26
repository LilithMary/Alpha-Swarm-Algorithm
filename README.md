# Alpha-Swarm-Algorithm
This repository provides implementations of a simplified Alpha swarm aggregation algorithm in NuXMV and GROOVE.

## Software
**NuXMV**: Install [NuXMV] (https://nuxmv.fbk.eu/download.html)
**GROOVE**: Install [GROOVE] (https://groove.cs.utwente.nl/installing.html)

## Experiments
For each of GROOVE and NuXMV, we have provided a sample code for grid length 4 with 3, 4, and 5 robots. Below are guidelines for running the code on Linux. 

### Run Sample Codes
#### NuXMV
```
cd <nuxmv-path>/bin/
(time ./nuxmv <path-to-smv-code> > <path-to-save-output>;) 2> <path-to-save-duration>
```

#### GROOVE
```
cd <groove-path>/nl/utwente/groove
(time -cp ../../../bin/Simulator.jar nl.utwente.groove.ModelChecker -ctl "AG(AF(connectedSwarm))" <path-to-groove-gps-folder> > <path-to-save-output>;) 2> <path-to-save-duration>
```

### Modify Sample Codes
Below are guidelines for conducting bounded model checking and changing the initial locations of robots. 

#### Bounded Model Checking

#### NuXMV
1. Inside the SMV file, change the CTLSPEC to LTLSTEP and modify the formula accordingly as shown below:
   Replace  ```CTLSPEC AG (AF connectedSwarm);``` with ```LTLSPEC G (F connectedSwarm);```.
2.

#### GROOVE

#### Initial Robot Locations
