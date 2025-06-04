According to ISO Cloud Computing is a paradigm for enabling network access to a scalable and elastic pool of shareable physical or virtual resources with self-service provisioning and administration on-demand
We identifiy Cloud Computing through computing resources gained by a cloud provider, but there are also the SW resources for these HW resources, you can have different levels of procurements from a cloud providers. 
The contract with cloud computing companies cannot be in self service mode, describe the maximum resources you can gain and you may have easily more capacity than the one in the data center. 
There are six global provider to which basically everyone refer to, but in the case the company has special needs(infrastructure in remote places, end users are in a geographical area that is different from the one of the provider) there can be smaller providers/need more security. Can there hybrid solutions where you handle the geographical distribution of the servers through different contracts with different providers.
## Horizontal scalability
Instead of enhancing the capability of the single server you buy more servers with the same HW and put them in parallel, add more node to your cluster. You can also do resource culling, the server you are using can be shared with other users, resources are dynamically assigned wrt the demand of the system. 
## Service Models
There is the division between the number of services/infrastructure that you acquire through the cloud that can go from everything in house to everything on cloud.
## Cloud Computing Types
- Public Cloud: you are sharing the computing capacities with other users
- Private Cloud: the cloud resources are dedicated only to a single user
Simplification and ease of use are beautiful concept but they are expensive as they needs more initial resources to be established and inserted in a company. Need to pay attention to the advantages and disadvantages of the cloud transformation, better to have an hybrid approach and use the scalability of cloud when the local datacenter is not fast enough for the company need. Also having only horizontal scalability is convenient to have small/cheap servers that we can add to the network, but you have to design application to be horizontally scalable and sometimes the algorithm for design can be not suitable for horizontal scalability.
Not important to make SW efficient as the machines are so powerful that it is a waste of time(only for application SW, for maintenance/processing SW is still better to write efficient code).
You want predictability of performance as you want to know what you are gonna need. Private cloud computing has the disadvantage that is cost intensive as it needs more physical resources(servers, employees,...).
## Fully managed
The cloud manager handles all aspects of the operational management of a service or asset relieving the customer of the responsibility for most/all operational management tasks. 
## Autoscaling
Capability of scaling horizontally automatically without the need of human intervention, set up policies to maintain the service up(this is done by few companies as they want to avoid the risk of continual expansion and infinite costs).
Cost are difficult to control, if you need do not know how much resources you have you tend to not thinking about efficiency so you overspend on resources as you use more resources than needed.
## Value Proposition
- AWS:On demand resources, pay on the go, no long term commitments. highly automated. 
- GCP: brings together innovations across Goggle, helps customers digitally transform with AI. 
Amazon offers some capabilities for free to create a lock in along the way as the company meanwhile will use Amazon SW. 
## Organizational Change thanks to The Cloud Migration
There is a transformation focused on the business and trying to sell the idea of simplifying coding and selling the idea of no competence in house. But is this transformation is really in act? Probably no as it is real that lower some barriers for some small companies but there is still the needs of having capabilities in house, there is still flexibility in changing the provider. You never want a lock in, there are committee that search for lock ins as there is the problem of resilience wrt the closure of a provider. 
## Cloud products and services
The products and services of cloud providers have a difficulty in distribution, difficulty in coping with marrying a cloud service, if you standardize you may need only a few service, no need of huge, complex, layered architecture. Cost control is difficult thanks to this pletora of services, also if you use autoscaling as you may not need these services and do not need so much resources. Complicated to buy different services by different providers as the integration is bothersome. 
## Software Efficiency
Cloud providers focuses on the efficiency of their datacenters but never talks about the software they run, as their business model is based on selling HW and less usage they gain less revenue. 
Also misconception on efficiency and capacity: managers think SW works as vehicles as they think a SW can only be fast or efficient, this is not true and is what the cloud providers use.

#### Exercise

|     | GCP   | Hetzner |
| --- | ----- | ------- |
| UC1 | 2400$ | 188$    |
| UC2 | 1600$ | 324$    |
Google is more expensive but much easier to put services on as they have already a bunch of APIs, useful for speed and simplicity.
Advantages also on predictions but not on saving money. 