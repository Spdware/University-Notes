Dependability: a measure of how much we trust a system. The ability of a system to perform its functionality while exposing:
- Reliability: continuity of correct service
- Availability: readiness for correct service
- Maintainability: ability for easy maintainance
- Safety:absence of catastrophic consequences
- Security: confidentiality and integrity of data
### Why dependability

A lot of effort is devoted to make sure the implementation matches specifications, fulfill requirements, meets constraints, optimizes selected parameters (performance, energy, …)
Nevertheless, even if all above aspects are satisfied … things may go wrong

![](https://i.imgur.com/655UMMX.png)

Failure effects:
- A single system failure may affect a large number of people
- A failure may have high costs if it impacts economic losses or physical damage
- Systems that are not dependable are likely not be used or adopted
- Undependable systems may cause information loss with a high consequent recovery cost
### When to think about  dependability?
__*You have to think about dependability both at design time and at runtime*__
- Design time: analyse the system under design, measure dependability properties, modify the design if required
- Runtime: detect malfunctions, understand causes, react
Failures occur in development & operation
-  Failures in development should be avoided 
- Failures in operation cannot be avoided (things break), they must be  dealt with
Design should take failures into account and guarantee that control and safety are achieved when failures occur. Effects of such failures should be predictable and deterministic … not  catastrophic
### Where to apply dependability?
Initially dependability has been a relevant aspect only for safety-critical and 
mission-critical application environments as it has huge costs and it was acceptable only when mandatory. However it shouldn't be neglected even in everyday/basic appliance.
Non-critical critical systems: a failure during operation can have  economic and reputation effects(Consumer products)
Mission-critical systems: a failure during operation can have serious or irreversible effects on the mission the system is carrying out(Satellites,  Automatic Wheather Stations, Surveillance drones, Unmanned vehicles)
Safety-critical systems: a failure during operation can present a direct threat to human life(aircraft control systems, medical instrumentation, railway signaling, nuclear reactor, control systems)
### How to provide dependability?
You can use failure avoidance paradigm as:
- Conservative design
- Design validation
- Detailed test on HW/SW
- Infant mortality screen
- Error avoidance
You can also use failure tolerance paradigm:
- Error detection / error masking during system operation
- On-line monitoring
- Diagnostics
- Self-recovery & self-repair
#### Key elements
Dependable systems can be achieved by means of a Robust design (error-free), Processes and design practices, Robust operation (fault tolerant), Monitoring, detection and mitigation
#### Safety-critical systems




### Reliability
The ability of a system or component to perform its required functions under stated conditions for a specified period of time.
Definition: R(t): probability that the system will operate correctly in a specified operating environment until time _t_
R(t) = P(not failed during [0, t]) assuming it was operating at time t = 0
1 – R(t): __*unreliability*__, also denoted Q(t) is the probability that a system is not working after a period of time equal to _t_
R(t) is a non-increasing function varying from 1 to 0 over $[0,+∞)$
$$\lim_{x\rightarrow +\infty}R(t)=0,\lim_{x\rightarrow +\infty}Q(t)=1 $$
Often used to characterize systems in which even small periods of incorrect behavior are unacceptable:
- Performance requirements
- Timing requirements
- Extreme safety requirements
- Impossibility or difficulty to repair
### Availability
The degree to which a system or component is operational and accessible when required for use: Availability = Uptime / (Uptime + Downtime)
Definition: A(t): probability that the system will be operational at time _t_
A(t) = P(not failed at time t), it is a constant probability in time
1 – A(t): unavailability
Reliability and Availability are 0 if a system is broken.
In general (repairable systems): A(t) ≥ R(t) [A system can be available also when it is not reliable]

![](https://i.imgur.com/5ks7l1C.png)

![](https://i.imgur.com/PyNxhLU.png)

### Definition
MTTF (Mean Time To Failure): mean time before any failure will occur(in particular the first failure after a repair)
MTBF (Mean Time Between Failures): mean time between two failures, $MTBF = \frac{total operating time}{number of failures}$
MTTF (Mean Time To Failure): mean time before any failure will occur $MTTF= \int_0^{\infty}R(t)dt$ 

![](https://i.imgur.com/W8qqR8U.png)

![](https://i.imgur.com/25lnC80.png)

Of course reliability and availability are related:
- if a system is unavailable it is not delivering the specified system services
It is possible to have systems with low reliability that must be 
available:
- system failures can be repaired quickly and do not damage data,  low reliability may not be a problem (for example a database management system)
The opposite is generally more difficult
### Reliability terminology
Fault: a defect within the system, an incorrect system failure, it can be quiet, not detected by the system
Error: a deviation from the required operation of the system or subsytem
Failure: an error that produce a different behaviour from the expected one
Not always the chain fault-error-failure close as there could be some error/fault not resulting in failure or sometimes the fault is innoque.
### Reliability Block Diagrams
We have to compute as fast as possible the reliability and a good model to do so is the Reliability Blocks Diagrams. It doesn't represent your system but how it has to work.
An inductive model where a system is divided into blocks that represent distinct elements such as components or subsystems.
Every element in the RBD has its own reliability (previously calculated or modelled)
Blocks are then combined together to model all the possible success paths

![](https://i.imgur.com/1jr7vrX.png)

To simplify things we will consider $\lambda$ fixed, constant. 
We can represent the components in series(all the components have to be up to have a functioning system) and in parallel(at least one components has to be up to have a functioning system). The RBD is a graph, a success path. 
For components in series the failure rate is $R_s(t)=e^{-\lambda_st}$ where $\lambda_s=\sum_{i=1}^n\lambda_i$ and 
$$MTTF_s=\frac{1}{\lambda_s}=\frac{1}{\sum_{i=1}^n\lambda_i}=\frac{1}{\sum_{i=1}^n\frac{1}{MTTF_i}}$$

![](https://i.imgur.com/i2lYZfH.png)

A system may be composed of two parallel replicas: 
- The primary replica working all time, and
- The redundant replica (generally disable) that is activated when the primary  replica fails 
Obviously we need:
- A mechanism to determine whether the primary replica is working properly  or not (on-line self check)
- A dynamic switching mechanism to disable the primary replica and activate the redundant one
