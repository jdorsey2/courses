## §1 Introduction

cloud is the commercial availability of individual computing services delivered remotely.

* desktop
* disk/storage
* processing
* networking
* database

on demand model

cloud economics

service is delivered by way of internet

service accessed over the network are remote services.

### Scalable computing and storage services

Scalable computing and storage services is an offering of Cloud computing. Virtualization makes it possible to scale up computing resources on-demand. This allows forms to add new computing resources just when they need it, and manage costs. The same is true for storage. Cloud makes it possible for firms to obtain more disk for storage, when it is needed.

* type 1 hypervisor runs directly on the hardware. the hypervisor also provides the os
* type 2 hypervisor is managed by the os, like any other application.

Distributed computing is a field of computer science that studies distributed systems. A distributed system is a system whose components are located on different networked computers, which communicate and coordinate their actions by passing messages to one another. The components interact with one another in order to achieve a common goal. Three significant characteristics of distributed systems are: concurrency of components, lack of a global clock, and independent failure of components. Examples of distributed systems vary from SOA-based systems to massively multiplayer online games to peer-to-peer applications.

A computer program that runs within a distributed system is called a distributed program (and distributed programming is the process of writing such programs). There are many different types of implementations for the message passing mechanism, including pure HTTP, RPC-like connectors and message queues.

### managed infra - saas

saas software products are hosted by a third party and made available to users over an internet connection.

sass is a method for delivering sw product services based on a subscription model.

## §2 level of managed service

dev and testing in an environment that emulates prod is the best scenario for quality.

with virutalization it is possible to manage hardware like software. therefore an env can be templated, and copied as many times as needed.

email is a saas service. office 365.

function as a service. serverless computing.
aws lambda, google functions, azure functions.

firm's data is under control of cloud provider.

Infrastructure as a Service (IaaS) is a category of Cloud Computing where computing infrastructure is used by customers on a "pay-as-you-go" basis. Often IaaS is considered outsourcing. IaaS is scalable to meet the needs of customers. With IaaS, customers can have all the same computing services as very large firms.

Platform as a Service (PaaS) is a category of Cloud Computing that delivers development environments and tools for developing applications. PaaS delivers a pay-as-you-go suite of tools to firms of any size. The development tools can be the top line products that are very expensive.

Software as a Service (SaaS) is a category to of Cloud Computing where a 3rd party hosts software products, and makes them available to users over the internet. SaaS offers a pay-by-user, and pay-by-month options. These options are much more efficient than the perpetual license model offered by many software vendors.

Function as a Service (FaaS) is a very specialized category of Cloud Computing. FaaS delivers real time functionality to firms, when the functionality is needed. With FaaS, the customer does not pay for idle time. During idle time the processing stops. The processing starts again when the user requests the functionality. Only paying for "up time" is a major difference between FaaS, and IaaS. With IaaS, customer pay for the computing infrastructure with it is active, or inactive.

## §3 Deployment Models

a private cloud does not use the public internet. cloud services are delivered on the firm's network.

private clouds can be physical and virtual.

virtual private cloud (vpc) uses virtual private network (vpn) approach to delivere cloud services over the internet.

public clouds are typically data centers.


### HPC cloud

there are 3 components of HPC: computing, data storage and networking.

### Big data cloud

processing of data is scheduled to run off hours when users are not connected to the system.

the processed dat is stored in the data warehouse for reporting.

### Reading

The Private Cloud is Cloud computing services offered by an enterprise on its private internal network and only to select users instead of the general public. Also referred to as an internal or corporate cloud, private cloud computing gives businesses many of the benefits of Cloud computing such as: self-service, scalability, and elasticity. However, a private Cloud offered the enterprise control and customization of the data and hardware resources. The enterprise also has control over the computing infrastructure hosted on-premises. In addition, private clouds deliver a higher level of security and privacy through both company firewalls and internal hosting to ensure operations and sensitive data are not accessible to third-party providers. One disadvantage is that the enterprise's IT department is held responsible for the cost and accountability of managing the private cloud. So private clouds require the same staffing, management, and maintenance expenses as traditional data center ownership.

A Virtual Private Cloud (VPC) is an on-demand configurable pool of shared computing resources allocated by a public Cloud provider, within a public cloud, delivering Cloud computing services over a virtual private network (VPN). The VPN permits or creates a sub-network that is part of the enterprise's network. However, the VPN uses the internet as a medium for the enterprise connection. The VPN permits the enterprise to virtually work virtually in private.

A Public Cloud is a 3rd party provider that makes computing services available to any individual or business that seeks to purchase the services. The services are delivered over the public internet. Public Clouds make services available in a "pay as you go" model, where customers can scale up, or down as needed.

A Hybrid Cloud is a mix between on-premises private Cloud and services from a public Cloud provider. The mix allows the enterprise to reduce some of the disadvantages, and risks of the public Cloud. The enterprise can use the private portion of the solution to manage private or sensitive data. This can also keep control of the critical data. The private component can also reduce the reliance on a 3rd party provider.

The public component of the Hybrid Cloud typically adds infrastructure as a service. The public component delivers processing, networking, test environments, and other items. A Hybrid Cloud delivers all the advantages of Cloud computing, while still managing the downsides and risks.

High Performance Computing (HPC) is a computing strategy involving parallel processing. The parallel processing is enabled by a cluster. A cluster is a group of computers acting as one computer. The processing is shared by all the computers in the cluster. This strategy allows more calculations per second.

Big Data is the concept of analyzing complex data sets. The data sets are so complex, that only powerful computing resources can identify trends and further analyze the data. Big data also require large amounts of storage for the complex data sets. Cloud computing can deliver the computing resources to storage and process Big Data's complex data sets.


## §4 Hosting Scenarios

bare metal computing. a laptops native hardware and software is a good exmaple. most mobile devices.

hypervisor manages access to the physical hardware for each vm.

multiple copies of vm images can be made, each with their own ip address and host id.

docker is a paas product that provides virtualized os env called containers.

docker containers are 100% independent from each other, and they are bundled with their own software, libraries and others.

all docker containers run on a single os kernel called docker engine.

a pod is independent and has its own ip address.

a k8s service is a set of pod that uses the cluster approach.
a sercie is a group of pods working as one.

on-premise computing must be installed with excess capacity. inefficient.

### edge computing

edge computing is where computing services including processing and storage are brought physically closer to customer. orgins from content delivery of large data files audio and video. popular in gaming and iot as responsiveness is critical.

healthcare is important edge computing use case.

### Best practices

* software hosting
* on demand expertise and support
* email hosting
* backup services and disk
* dev/test env
* virtual desktops
* devops tools and processes

## §5 comparing cloud platform alternatives

## §6 future of cloud

* Serverless computing
* an extension of function as a service
* distributed ledger technology dlt
* dapp: decentralized applications
* no central point of failure
* ai backend heavy. ideal for cloud
* iot is vulnerable
* 
