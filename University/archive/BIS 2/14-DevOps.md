How to better take an idea into production.
More of a set of tools and practices and mindset. 
Agile is not enough: it is fast but when you have to deploy the application in a company where there is a traditional Ops team you will have to wait for them to take your application into production(common in the past). Problem is the development team want to be fast meanwhile the operational team want reliability and stability, the agile team want to be free wrt the choice of components meanwhile the operational team is responsible for security. Best solution is to have a single team where there are people with operational skills.
Operation team worked in the past with a ticket based approach. Organisational wall between agile and operational worlds.
Try to shift the problems from production to the development teams(change in paradigm, no more a problem of the production guys, need to discover problems in the development part, usually you want to push to Github only the code that is Warning and Error free)
Usually when there is something broken at integration level you stop everything and try to resolve it as it can be catastrophic for all the project.
Nowadays you can see if your product can work you can use a VM(still need to talk to the Op guys to understand the capability of the VMs to adapt to the real world scenario).
Ops guys often work in emergency settings as they are the first to be called when something is wrong. Devs guys want to have access to all the system, also to the production machines to have a complete overview of the system. 
To be effective you need a toolchain to integrate Devs and Ops. 
## Goals
Need more speed, reliability and less costs. 
Try to execute your pipeline as much as possible before release to train the system and the team to overcome problems and difficulties. Repeating and automating all the processes can help in intercept the problems before they arrive in production/you can cope faster from errors as you have already seen something similar.
## How to adopt DevOps: the three wants
1. System thinking
2. Amplify feedback loop: ask feedback between devs and ops and apply them
3. Culture of continual experimentation and learning: analyze where you can improve your pipeline.
All the tools need to be configured, if you use scripting instead of manual setting you can treat these scripts as source code(Infrastructure as a Service, need to knows how much time you need to recreate the production environment from scratch). Need to test your script if they can recreate the production environment in a small amount of time. Scripting instead of manual configuration is useful to create an exact copy of your environment.
Really need observability tools as they can help in finding where the problems are.
You need:
- Continuous integration: continue to add functionalities as much as possible
- Continuous deployment: deploy the SW in as much environment as possible
**Unit test**: test the behaviour of the unit
**Integration test**: simulate a component that calls some APIs and see if the behaviour of the system is the correct one
**End to End test**: recreate the global system, need the initial configuration and usually this is an expensive procedure so they are used less than the others
Usually the proportion production/test for a 90% coverage could be of 100/60.
If you want to maintain a good system you should write good test coverage as it helps in maintaining the source code error free.
The shift of problem discovery and resolution to the development portion of the company and not the operational part helps in anticipating and resolve better the problems. 
## ChatOps
Need to promote communications between the members of the same team and between different teams, we do not want to create segregation of knowledge, even with machines. 