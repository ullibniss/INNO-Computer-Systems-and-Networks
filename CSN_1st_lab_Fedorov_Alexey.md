# Lab 1. Identification of a System

## Done by Fedorov Alexey  ([Telegram](https://t.me/ullibniss), [Github](https://github.com/ullibniss/))

---

### 1. What is a System? Can you give an example?

```
The concept of system implies an arrangement of parts which, through interaction, form a whole.
```
**Ludwig von Bertalanffy**

System is a set of components working together for one purpose. It is the crusual concept for humanity. Every thing in our world is a system, from nature to big companies.

### 2. What are the essential elements of a functional System? can you draw a diagram of it?

1. **Input** - The resources, information, or materials that are put into the system. Inputs are necessary for the system to function and produce outputs.

2. **Process** - The transformation or activities that take place within the system to convert inputs into outputs. This could involve various mechanisms, actions, or functions depending on the type of system.

3. **Output** - The result or product that comes out of the system after processing the inputs. Outputs can be physical products, services, information, or other outcomes.

4. **Feedback** - Information about the output or the process that is fed back into the system to regulate or adjust the system’s functioning. Feedback helps the system to maintain stability, improve performance, or adapt to changes.

5. **Environment (partially)** - The external conditions, constraints, and influences that affect the system. The environment can provide inputs and receive outputs, and it may also impact the system’s performance.

System flow

![System Flow](https://github.com/user-attachments/assets/ee4122d6-d333-406b-bb19-a8c3f9b04fbf)

### 3. What is feedback in a system? what does it help us with?

**Feedback** in a system refers to the process where a portion of a system's output is returned as input, thereby influencing future behavior of the system.

Types of Feedback:

1. **Positive Feedback** - amplifies changes in the system. An example is when a microphone picks up sound from a speaker, causing an escalating squeal due to continuous amplification.

2. **Negative Feedback** - make system more stable. For example, thermostat. When the temperature exceeds a certain level, the thermostat will turn off the heater to reduce the temperature.

#### What Feedback Helps Us With:

1. **Stabilization and Control** - feedback helps maintain system stability. Negative feedback is particularly useful in regulating systems, ensuring they operate within desired parameters.

2. **Improvement of Performance** - by monitoring the output and making adjustments based on feedback, the system can improve efficiency, accuracy, and effectiveness.
    
3. **Adaptation** - in complex or dynamic environments, feedback enables systems to adapt to changes, ensuring continued functionality or performance.

### 4. What is Information System? Can you give an example?

An organization's decision-making, coordination, control, analysis, and visualization processes are supported by a **Information System (IS)**, a structured arrangement that gathers, processes, stores, and distributes information. In order to efficiently handle and interpret data, it blends people, technology, and procedures.

A **Customer Relationship Management (CRM) System** is an illustration of an information system. An information system called a CRM is made to help businesses handle their contacts with both present and future clients.

The essential elements of a Information system are:

1. **Hardware**: Physical devices and equipment used for input, processing, and output.
   
2. **Software**: Applications and operating systems that process data and provide interfaces for users.
   
3. **Data**: Raw facts and figures that are processed into meaningful information.
   
4. **Procedures**: Instructions and rules that govern the operation of the system .
   
5. **People**: Users and IT professionals who interact with the system.

![image](https://github.com/user-attachments/assets/3d634fae-426b-4710-a7f7-7b313af18ad7)

### 6. From your point of view what are the components of a distributed file systems? can you draw a diagram connecting these components?

#### Components of a Distributed File System:

1. **Clients** - users or applications that access and interact with the distributed file system. Clients initiate requests for file operations such as read, write, or delete.

2. **Servers** - machines that store and manage files. They handle client requests, provide access to data, and ensure data consistency and availability. Servers often collaborate to distribute the workload.

3. **Storage** - these are the physical or virtual machines where the actual file data is stored. In some systems, storage nodes can be integrated with servers, while in others, they might be separate entities.

4. **Network** - the communication backbone that connects clients, servers, metadata servers, and storage nodes. This network ensures data transfer, synchronization, and communication across the distributed system.

6. **Replication Mechanism** - ensures redundancy by replicating data across multiple servers or storage nodes to enhance fault tolerance and availability.

7. **Caching** - temporary storage mechanisms used to improve performance by storing frequently accessed data closer to the clients.

9. **Security Mechanisms** - authentication, encryption, and access control measures to ensure that data within the DFS is secure and only accessible to authorized users or applications.

![image](https://github.com/user-attachments/assets/dc1ba2ba-fe20-4ca6-91f1-29b5b9e45118)

### 7. What is the difference between Centralized, Decentralized, and Distributed system? Can you give an example of each one?

#### **Centralized System**

In a centralized system, all decision-making, data processing, and management are handled by a single central authority or server. All nodes or clients in the system are dependent on this central point for information and services.

- **Single point of control** - a central server or authority governs the entire system.
- **Single point of failure** - if the central node fails, the entire system can become inoperative.
- **Easier to manage** - centralized systems are generally easier to maintain and secure because everything is managed from one location.

Example:
**Traditional Banking System** - in a traditional bank, all transactions and account details are processed and managed by a central server. Customers interact with the bank's centralized system for all their financial needs.

#### **Decentralized System**

Even though a decentralized system splits up decision-making and data management among several nodes or servers, there is still some degree of interdependence among these nodes. There are multiple important nodes that collaborate with one another rather than a single central authority.

- **Multiple points of control** - dontrol is shared among several nodes or entities.
- **Reduced single point of failure**- the system can still function if one node fails, as other nodes can continue to operate independently.

Example:
**Blockchain Networks (e.g., Bitcoin)**: In Bitcoin, multiple miners (nodes) validate and record transactions on the blockchain. There’s no single central server; instead, the network relies on a decentralized consensus among participants to validate transactions.

#### **Distributed System**:

A distributed system spreads data, processing, and control across multiple independent nodes or machines. These nodes work together as a cohesive unit to achieve a common goal, often appearing as a single system to the user.

- **No single point of control** - all nodes work independently but collaborate to perform tasks, reducing reliance on any single node.
- **Highly fault-tolerant** - even if several nodes fail, the system can continue to function because the workload is distributed.

Example:
**Google Search Engine**: Google’s search engine is powered by a vast distributed system with thousands of servers across the globe. These servers work together to index web pages, process search queries, and deliver results, providing users with a seamless experience.

### 8. What does transparency means in distributed systems? Can you give an example for each form of transparency?

A transparency is some aspect of a distributed system that is hidden or abstracted from the user (programmer, system developer, user or
application program), so it can be ignored. A transparency is implemented by some set of mechanisms in the distributed system at a layer below the interface where the transparency is required

- **Access** - hide differences in data representation and how a resource is accessed  
- **Location** - hide where a resource is located  
- **Migration** - hide that a resource may move to another location  
- **Relocation** - hide that a resource may be moved to another location while in use  
- **Replication** - hide that a resource may be shared by several competitive users  
- **Concurrency** - hide that a resource may be shared by several competitive users  
- **Failure** - hide the failure and recovery of a resource  
- **Persistence** - hide whether a (software) resource is in memory or on disk  

### 9. In system documentation, what is the difference between Structure and Behavior documentation?

In system documentation, **Structure** and **Behavior** documentation refer to two different aspects of the system's architecture and functionality:

#### **Structure Documentation**

Structure documentation focuses on the static aspects of the system, detailing the components and how they are organized and related to one another.

This typically includes diagrams and descriptions of:
 - **Architecture** - the overall architecture of the system, including layers, modules, components, and their interactions.
 - **Data Models** - descriptions of data entities, their relationships, and how data flows through the system.
 - **Interfaces** - specifications of the interfaces between different components or subsystems, including APIs.
 - **Deployment** - information on how the system is deployed, such as server configurations, network topology, and physical distribution of components.
  
#### **Behavior Documentation**

Behavior documentation focuses on the dynamic aspects of the system, describing how the system behaves and functions in response to various inputs or events.

This typically includes:
 - **Use Cases** - descriptions of how the system is expected to behave in various scenarios, often from the user's perspective.
 - **Sequence Diagrams** - diagrams showing the sequence of interactions between components or objects over time to achieve specific functionality.
 - **State Diagrams** - diagrams that depict the states the system or a component can be in and the transitions between those states based on events or conditions.
 - **Workflows** - detailed descriptions of business processes and how the system supports these processes.

# References

1. https://en.wikipedia.org/wiki/Feedback
2. https://en.wikipedia.org/wiki/Decentralised_system
3. https://en.wikipedia.org/wiki/Centralized_computing
4. Lecture 1, Lecture 2 of Computer Systems and Networks course
5. https://vowi.fsinf.at/images/b/bc/TU_Wien-Verteilte_Systeme_VO_%28G%C3%B6schka%29_-_Tannenbaum-distributed_systems_principles_and_paradigms_2nd_edition.pdf
