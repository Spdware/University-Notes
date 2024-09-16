Cloud is a business model. Cloud permits to store services to full applications in the cloud with a pay per use principle. Concept is if I need something I connect to the web and require the service.

![](https://i.imgur.com/L4MaOpt.png)

At the start of a project you expect a linear capacity BUT as resource gathering is problematic you have to wait more to have a resource so you try to have a capacity greater than your needs. The red line represents the real capacity of the project: in some cases you have more capacity than needed and in some cases you have less resources than needed. 
In the cloud you can start/stop a VM/container in minutes and also predict the loads in time so I can allocate slighter higher capacity than what I need so I am reducing the overprovisioning saving money. This helps if you have high variability. If I need to have the 100% of the power of the server it can be smarter to buy the server.
### Virtualization

![](https://i.imgur.com/e3zNb6H.png)

HW resources are partitioned and shared among multiple virtual machines. The virtual machine monitor (VMM) governs the access to the physical resources among running VMs. Performance isolation and security: if you want to devote a number of resources to a machine you can and each machine is separated from the other so if one crashes the others are unaffected. Different machine can runs different OS.
Without virtualization every server runs an OS and a single application to avoid to crash other applications in case of failures, but this would lead to a waste of resources. With virtualization you can run multiple applications in a single machine using different VM. You can also port the VM to others machines copying the VM reducing the time required.
Impact of Virtualization on the evolution of IT systems:
- Sever consolidation
- Cloud computing
### Server Consolidation

![](https://i.imgur.com/1ApWdWz.png)

### Virtualization

![](https://i.imgur.com/Rc8QdA4.png)

Virtualization gives advantages also on consuming costs as running one server at full capacities can cost less than running different servers at 10% capabilities. It is possible to move Virtual Machines, without interrupting the applications running inside. It is possible to automatically balance the Workloads according to set limits and guarantees.
Servers and Applications are protected against component and system failure. Best to have a 60% utilization for every machine.
Advantages of Consolidation:
- Different OS can run on the same hardware
- Higher hardware utilization
	- Less hardware is needed
		- Acquisition costs
		- Management costs (human resources, power, cooling)
- Green IT-oriented
- Continue to use legacy software (e.g., software for WIN on Linux machines thanks to VMs) as you can run every OS independently from the age 
- Application independent from the hardware
### Cloud Computing
Cloud computing is a model for enabling
- convenient
- on-demand
network access to a shared pool of configurable computing resources, like for example:
- Networks
- Servers
- Storage
- Applications
- Services
that can be rapidly provisioned and released with minimal management effort or service provider interaction. 
In few minutes you can allocate resources as you wish. 

![](https://i.imgur.com/2j6iKlZ.png)

![](https://i.imgur.com/w2puUpE.png)

For the Applications layer we have full applications as Gmail, Google Docs, SalesForce.com, etc... The benefit to develop applications like this as it is faster as you develop for the VM, you access software well tested as you can consider it as a library computed by various people but you are accessing a black-box where you don't have possibilities to alter performance
For Cloud Software Environment the examples are Amazon SageMaker, Microsoft Azure Machine Learning, Google AI: TensorFlow, etc.. You are speedlining development and deployment and you can support automatic scalability as the platform is managed by the provider but at the cost of locking yourself in a single software environment as the APIs are propietary(To migrate to another one you have to spend time adapting the code to the new environment). 
For Cloud Software Infrastructure you are having computational(IaaS), storage(DaaS) and communications(CaaS) services. Provides resources to the higher-level layers (i.e., Application and Software Environment). Usually it is the layer where all the configurations and applications build are done. The computational layer permits to handle VM and gives flexibility and super user access to VMs for customization BUT this also introduce performance interference, inability to provide strong guarantees about SLAs, you have to monitor the behaviour and utilization of your server/machine/VM.
In the DaaS layer you are allowed to store data/objects and you can access it at anytime from any place but you have the problem of dependability, replication and data consistency. CEPH is an open source solution where you can build a cloud solution on your device and on every block on the device increasing dependability and performance in some cases. 
For CaaS the communication is a vital component. The communication capability and Network security are looked on.

![](https://i.imgur.com/RlHLPTY.png)

### Types of Clouds

![](https://i.imgur.com/SI4tftY.png)

Based on who owns the infrastructures.
- Public is when you go to a provider and pay per use, you have fully customer self-service and accountability is e-commerce based. Service Level Agreements (SLAs) are advertized. Requests are accepted and resources granted via web services. Customers access resources remotely via the Internet. 
- Private is when an organization owns a datacenter and runs only its applications. It's relevant for security. You also control the virtualization software but it is not a cloud service so it doesn't have infinity scalability(if you don't pay for new machines)
- Community is a single cloud managed by multiple organizations. Combining together several organizations allows economy of scale. Resources can be shared and used by one organization, while the others are not using them. 
- Hybrids are a mix of all the others: the most frequent is the scheme that mix a public cloud with a private one called Cloud Bursting(I buy a quantity of servers and use them as a mini private server and when I hit the limit of my resources I use a Public server). Another one is using a private cloud for storing the data and using a public cloud for running my application. 
### From Cloud to Edge and Fog Computing
Nowadays Cloud is used for various things and the advantages are costs, software updates, performances, unlimited capacity....
The key problem of these systems is that they are remote services and you need a good connection to the web otherwise your experience would be suboptimal or completely absent. There are also problems related to Lock-in and security on stored data.
The solution is Fog/Edge computing: as today we can gather data from sensor and that sensors can be closer to the data source we can use them to generate a solution.

![](https://i.imgur.com/M4XOUif.png)

The edge do some calculus but the cloud do the most expensive ones.