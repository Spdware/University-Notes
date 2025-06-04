# Why Microservices are needed in the new Application paradigm
Microservice architecture is difficult to achieve, even if it brings in a lot of advantages. It requires a good technical level and a good organization structure. If there aren't the conditions is better to stick to the monolith approach.
## Static Web Sites
They were using files and requesting resources through a browser and updating the client with the new resources.
## Web application
Content is no more static, created starting from the data sent in the request, can still have static data in a DB. Now we can also have web application instead of browsers. 
Usually used an MVC design pattern as base for the production of the application. Use different units that work asynchrounsly wrt each other(maybe a mistake in some cases). Nowadays is used as starting base for Web Application, still needs a good architecture to do not do mistakes and reduce the possible complexity. 
Application server(Usually closed source, open source is JBoss). The server part have an orchestration role, contains all the logic for your application. 
## Monolithic Architectural Paradigm 
Single application server that runs  the application logic, for redundancy and reliability purposes you have other servers, there can be a Load Balancer that distribute the load between the nodes(still you have the monolithic component that runs the application).
two orthogonal dimensions:
- Architectural level
- Business level
### Architectural level
It have different layers but it is still monolithic as they are on the same component.
### Business level
Highly couple the different modules to let the application run smoothly, attention to not have circularity in the components. 

### Strength of monolithic architecture
Easiness of production, high initial productivity, fast integration between objects created by different developers, unique technology stack.
This lead to not needed an high starting skill level and could integrate developers better
## Technical Debt
If you have bugs, if you take shortcuts during the design phase, if you compromise quality and maintainability you are creating technical debt. There are more way to create technical debt, but the point is whenever you try to take shortcuts/try to cover problems with a band aid you are creating problems for the future. 
Try to find the right architecture for the moment with the compromise that you should be able to change it later if needed. 
Better to not enter in the technical debt loop. Still try to not over analyze the solution as you will not produce anything, still do not under analyze the solution as it will be bad in the future. Technical debt can be introduced if there is little time but it need to be addressed as early as possible. 
Can also happen if you do not have experience you can accidentally introduce technical debt.
Can also happens that during maintenance you introduce more bugs than the one you resolved, in these cases could be better to rewrite the code instead of trying to save it.
## Weaknesses of monolithic architecture
Cannot scale the team, if the application is to complex it can be difficult to coordinate more than 9 people on the same component. 
Performance impact: the fact that you have a single operating block the load balancing of the requests can be difficult as you need to upgrade the HW of your machine, even if the needs is minimal.
Unique technology stack: you cannot have multiple stacks that can help you as they are better at handling the current task. 
It is easy to accumulate a lot of technical debt, easier to not have a generational scalability(unable to upgrade to new technology) and heterogeneous scalability(couple too much with some technology so it is difficult to move to a newer technology as you need to change everything). 

# Microservice Architecture
Created for business reasons: 
- Have smaller system
- Scale to be fast
- Want to be able to scale
- Change version of technology used
Business motives created the microservice paradigm.
## Logical Architecture
Each component will not do more than a fixed amount of functions, logic is divide et impera: divide the functionalities in different components, use APIs to access them, do one thing and do it well. No more possibility to two part of the application to communicate/read from a DB that is not mine. So these architecture are cloud based, they can natively be distributed. 
Some entities are unique to the microservice, others are shared between the microservices, can distribute data with replication of data, can also have mine representation of the data. There can be shared entities that needs synchronization(only one microservice has the right to change the shared data). This is bounded context, there is also business logic.
The microservices can communicate through APIs, usually these microservices do not communicate with web pages, there is an application between the user and the microservices that uses the APIs to retrieve data, also using an API gateway.
Usually there is a publish/subscribe paradigm to let the communication go through. This guarantee eventual consistency(at some time all the users will have the same data, very different from single DB solution, distributed systems are more complex to manage).
### Strength of Microservice architecture
- **Autonomy**: every is parted together, every service has all its component in a single place. SOA probably failed as they proposed the separation of data and business logic in the single service
- **Scalability**: you can implements different functionality in different ways, you only need to create the right interfaces to let the services communicate between them. Can implement different part of the stack with different technologies
- Promotes strong decoupling and business oriented APIs
- **Cloud Ready**: architectural model is well suited to the use of services offered by the cloud
Scrum masters want to have projects that can be handled by every member of the company, no closure of the information to the team members.
Also helpful in adding new functionalities without the need to change all the structure. 
Reusability is a key factor so you need a really good API design. 
### Problems with Microservice architecture
Log files are difficult to be analyzed.
Use a search engine to analyze the logs as the paradigm create an enormous amount of log entities, also almost impossible to know by hand where the clicks/requests are sent to. 
Consistency is difficult to manage as you need to take in consideration the multiple structure and need to handle also failure of remote objects.
Need to use the SAGA pattern
If the domain is not stable you end up to move entities between microservices a lot(expensive). In this case you can cope with it creating a monolith and then divide the monolith in microservices(if you do not couple too much the services). 
Another concept can be modular monolith(Modulit project) as microservice architecture is expensive to maintain. 