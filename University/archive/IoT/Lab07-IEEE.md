Usually you have different moles, in a mesh topology they can communicate between them.
There can be Full Functions Devices that contains all the logic needed to work, they can rely messages and need to have an energy plug. Reduced Function Devices have only some capabilities of the FFD and they are powered by a battery.
Personal Access Network Coordinator: it coordinates all the messages of the personal network. An hub is enough to communicate even between different "languages" as the hub will translate the messages when needed.
$Beacon$: time where you define all the other timers for the period, decide who needs to talk and in which direction
$T_{CAP}$ : time from the beacon when everyone can send a message(there will be a lot of collisions)
$N_{CFP}$: time when the communication is done without collisions
$T_{INACTIVE}$: time when there will not be any  form of communication(antennas can be shut down)
Beacon interval: time between the two start of two beacons
Data Rate: how fast I can communicate in the channel, $R=\frac{L_S}{T_S}$, $L_S$ is the bytes in one slot
Equivalent Data Rate: considering that I can communicate only in one slot how fast can I communicate? In the EDR we consider the full frame and see what happens if we are assigned only one slot, $r =\frac{L_S}{BI}$ , whit BI the Beacon Interval
