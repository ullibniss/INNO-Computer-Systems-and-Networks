# Lab. 2. Architecture Patterns

## Task 1

### 1.1. Provide 3 unique definitions for software architecture.

1. **Master-Slave** Pattern - is made up of a master component that assigns jobs to several slave components and gathers their output.
2. **Client-Server** Pattern - involves a server component that uses a network to give services to several client components. The server component is in charge of managing the system's business logic and data storage. The client components are in charge of managing the system's display and user interface.
3.  **Pipe-Filter** Pattern - organizes a software system into a sequence of filters that process data streams. Each filter performs a specific transformation on the data and passes it to the next filter through a pipe. The pipes act as buffers and connectors between the filters.

### 1.2 List and explain with sample diagrams, 3 types of software architecture.

#### Master-slave

To be honest, master-slave not only about assigning jobs and getting it's output. The main idea, that master contains more complex logic, that let it controls slave. It can be not only about executing jobs. This patter is also used for replication management and other cases.

![image](https://github.com/user-attachments/assets/3d92a40a-e92f-40de-b8f1-298e363a1f9c)

Examples:

- **Postgres** - supports master-slave replication. Master is in read-write mode, slave is readonly.
- **Salt** - use master-minion naming, but actually it is the same pattern. Master assigns jobs to minions.

#### Client-server

- **Server** - software that contains business logic. It means that server implements behaviour of web application. It accepts requests from clients and asyncroumsly processes them.
- **Client** - software that runs on user's side. It provides interface for user to interact with server. Client not only browser frontend, it can also be a cli interface or desktop application. 

![image](https://github.com/user-attachments/assets/e4bacf95-0c8d-4a96-85fb-b33606974e41)

Examples:

- **AWS S3** - service that implements file storage. Logic of file storage and how it implemented is serverside. Control plane provided by cloud or cli tool are clients.
- **Mysql** - simple SQL database server. Server contains innodb that manages data storage and provides socket to connect. Mysql cli is client which allows to connect to server and execute SQL requests.

#### Pipe-Filter

Pattern that provides data flow management. There is row infomation at the start of pipe. Then 2 filters applied. There is filtered useful information in output. 

![image](https://github.com/user-attachments/assets/fec219a3-684b-445b-a7fd-aa257822f60d)

Example:

- **Bash**  
![image](https://github.com/user-attachments/assets/a1679a51-3f04-4006-b495-0db7cd692d7a)


### 1.3. What are the differences between System architecture and Software architecture

**System Architecture** - describes the entire structure of a system, including both hardware and software components. It looks at how these components interact, including communication protocols, physical devices, and infrastructure.  
**Software Architecture** - focuses on the structure of the software within a system. It deals with how software components are organized, how they interact, and the principles guiding their design.

### 1.4. Differentiate between the following in the context of software architecture and provide examples;

### Software correctness and Software robustness.

**Software Correctness**: 
- **Definition** - correctness refers to the degree to which a software system meets its specified requirements and behaves as expected under normal conditions.
- **Focus** - ensuring that the software produces the correct outputs for a given set of inputs and adheres strictly to its functional requirements.
- **Example** - a calculator application that correctly computes 5 + 5 as 10 every time, as specified in the requirements, demonstrates correctness.
  
**Software Robustness**:
- **Definition** - robustness is the ability of software to handle unexpected or erroneous inputs and conditions gracefully without crashing or producing incorrect results.
- **Focus** - how well the software performs under abnormal, erroneous, or unexpected conditions. It emphasizes stability and fault tolerance.
- **Example** - a robust calculator application will handle invalid input like dividing by zero by showing an error message rather than crashing.

### Topic & queue

**Topic**:
- **Definition** - a topic is a messaging pattern used in publish-subscribe (pub-sub) architectures where messages are broadcasted to multiple consumers that have subscribed to a topic.
- **Key Characteristics**:
 - Many-to-many communication.
 - Subscribers receive messages in real-time.
- **Example** - In a stock market system, a topic like "NASDAQ" can broadcast stock price updates to multiple traders subscribing to that topic.

**Queue**:
- **Definition** - a queue follows the point-to-point messaging model where messages are sent to a single consumer that pulls messages from the queue in a First-In-First-Out (FIFO) order.
- **Key Characteristics**:
  - One-to-one communication.
  - Messages remain in the queue until processed.
- **Example** - a support ticket system where incoming tickets are placed in a queue, and each ticket is assigned to a single customer service representative.

### Architecture & design

**Architecture**:
- **Definition** - architecture refers to the high-level structure of a system, outlining its components, how they interact, and the overall framework.
- **Focus** - system-wide decisions, non-functional aspects, and high-level organization.
- **Example** - choosing a microservices architecture for an e-commerce application, where each service handles specific functionalities like orders, payments, and inventory, is an architectural decision.

**Design**:
- **Definition** - design is concerned with the detailed specification and construction of individual modules, classes, or components within the architecture. It deals with how the system's components are implemented.
- **Focus** - detailed decisions like algorithms, data structures, interfaces, and specific design patterns within the system.
- **Example** - using the Singleton design pattern to manage the database connection in a microservice is a design decision.
  
### User, primary stakeholder and secondary stakeholder

**User**:
- **Definition** - a user is the individual who directly interacts with the software to accomplish tasks or achieve specific goals.
- **Example** - the customer using a mobile banking app to check their account balance.

**Primary Stakeholder**:
- **Definition** - a primary stakeholder is someone who has a direct interest in the outcome of the software and its development. They can influence the project, and the system directly impacts them.
- **Example** - the product owner of a banking app is a primary stakeholder, as they represent business requirements and priorities in the development process.

**Secondary Stakeholder**:
- **Definition** - a secondary stakeholder has an indirect interest in the project but is affected by the system or its outcome. They may not directly interact with the software or have a say in its development but are impacted by its performance.
- **Example** - a compliance officer in a bank may be a secondary stakeholder because they are interested in the system’s adherence to financial regulations but do not use the system directly.

### Cohesion & coupling.

**Cohesion**:
- **Definition** - cohesion refers to how closely related and focused the responsibilities of a single module or component are. High cohesion means that the module's functions are closely related to each other and contribute to a single, well-defined task.
- **Focus** - ensuring that modules are focused, organized, and self-contained.
- **Example** - a "User Authentication" module that handles all tasks related to user login, signup, and password management exhibits high cohesion because all its functions revolve around user authentication.

**Coupling**:
- **Definition** - coupling refers to the degree of dependency between different modules. Low coupling means that modules can function independently and interact minimally with other modules, while high coupling means they are highly dependent on each other.
- **Focus** - reducing dependencies between modules to improve modularity and flexibility.
- **Example** - if the "User Authentication" module depends directly on the "Payment Processing" module for some functionality, there is high coupling between these two modules. If instead, they communicate via a well-defined interface or API, this results in low coupling.


## Task 2

### 2.1. List and explain the key concepts of Object-Oriented programming.

1. **Encapsulation** - means to enclose data by containing it within an object. In OOP, encapsulation forms a barrier around data to protect it from the rest of the code.
2. **Abstraction** - refers to using simplified classes, rather than complex implementation code, to access objects. Often, it's easier to design a program when you can separate the interface of a class from its implementation.
3. **Inheritance** - most object-oriented languages support inheritance, which means a new class automatically inhabits the same properties and functionalities as its parent class.
4. **Polymorphism** - polymorphism refers to creating objects with shared behaviors. In OOP, polymorphism allows for the uniform treatment of classes in a hierarchy.

![image](https://github.com/user-attachments/assets/fc19349c-eeab-409a-bf16-2364eca09eb9)

### 2.2. What is an architectural view? Explain characteristics of architecture patterns in pattern based.

An **architectural view** is a representation or perspective of a system's architecture, highlighting specific aspects or concerns to help stakeholders understand the system. Since systems are complex, it’s useful to break them down into different views, each addressing different concerns, such as performance, security, or scalability.

#### Examples:

**Logical** View - describes the functional elements of the system and how they interact (e.g., classes, objects).  
**Physical** View - represents the system's physical layout, including hardware and network components.  
**Development** View - shows the system's structure in terms of software modules, libraries, and their relationships.  
**Process** View - illustrates how the system behaves during execution, focusing on dynamic aspects like processes and threads.  

#### Characteristics of Architecture Patterns

- **Reusability** - offer predefined, reusable design solutions that can be applied across different systems and contexts.
- **Modularity** - promote organizing a system into discrete components or modules, enabling separation of concerns and simplifying system maintenance.
- **Scalability** - architecture patterns often address how a system can scale efficiently as demand grows, providing mechanisms to handle larger loads or more users.- 
- **Maintainability** - they focus on creating clear, organized structures that make it easier to maintain and modify the system over time.
- **Performance** - architecture patterns often include performance considerations, optimizing resource usage, latency, and response times in the system.
- **Separation of Concerns** - patterns help isolate different concerns (like data management, business logic, and user interface), reducing complexity and improving code organization.

### 2.3. Which are the main approaches to describing a software architecture? List and explain with sample diagrams.

There are seeveral approaches to describe software architecture:

- **Views and Viewpoints** - Describes the system from different perspectives like logical, process, deployment, etc.
- **C4 Model** - Breaks down the system into four levels of abstraction, from high-level context to detailed code.
- **UML Diagrams** - Uses visual diagrams like class, sequence, and deployment diagrams to describe system behavior.
- **ADRs** - Focuses on capturing architectural decisions in written form, emphasizing the rationale behind design choices.


#### Views and Viewpoints Approach (View Model by Philippe Kruchten)

- **Logical View** - Describes the functional requirements of the system, focusing on how the system’s major functionalities are provided.
- **Development View** - Focuses on the software development perspective, showing how components are organized in the codebase.
- **Process View** - Describes the dynamic aspects, such as concurrency and interaction between components.
- **Physical View** - Focuses on how the system will be physically deployed across hardware resources.
- **Scenarios (Use Case) View** - Captures how the system meets specific functional requirements by combining the above views.

![image](https://github.com/user-attachments/assets/9a017ffa-2ffa-4680-b304-da24f7110eff)

#### C4 Model (Context, Containers, Components, Code)

The **C4** model breaks down the system into 4 hierarchical levels of abstraction

- **Context** - Describes the system’s interactions with external entities like users or other systems.
- **Containers** - Describes the high-level containers in the system and their interactions.
- **Components** - Focuses on the internal components within each container.
- **Code** - Focuses on how the components are implemented in the code level.

![image](https://github.com/user-attachments/assets/9175391b-82d4-4c3e-8a49-662bd83fa799)

#### Unified Modeling Language (UML) Diagrams

**UML** is a standardized modeling language used to visualize the design of a system. UML diagrams can be used to describe different aspects of a system architecture:

- **Class Diagrams** - Show the static structure, like classes and relationships.
- **Sequence Diagrams** - Represent interactions between components over time.
- **Component Diagrams** - Illustrate the high-level structure of a system, with focus on modular components.
- **Deployment Diagrams** - Describe the hardware and software environment, showing where each component is deployed.

Class diagram example:

![image](https://github.com/user-attachments/assets/21adb8d6-e42f-4067-bfab-6c2ac8a1bc2d)

Sequence diagram example:

![image](https://github.com/user-attachments/assets/82d66efc-3485-47ce-ba55-3a08b35cefeb)

####  Architecture Decision Record (ADR)

An **Architecture Decision Record (ADR)** is a way of documenting the significant architectural decisions made during the development process. ADRs do not focus on diagrams but provide the rationale behind architectural choices. Each decision document records the context, problem, decision, consequences, and any alternatives considered.

![image](https://github.com/user-attachments/assets/8e8ebae1-e36b-4b6d-b87a-98435a1e2a39)


### 2.4. Provide the advantages and disadvantages of the main approaches explained in question 2.3

| **Approach**                | **Advantages**                                                                                                                                   | **Disadvantages**                                                                                                                                                  |
|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Views and Viewpoints**     | - Comprehensive<br>- Clear separation of concerns<br>- Standardized communication<br>- Scenario-driven                                           | - Complex to manage<br>- Requires expertise<br>- Risk of fragmentation between views                                                                               |
| **C4 Model**                 | - Layered detail<br>- Simplicity<br>- Clear focus on scope<br>- Good for communication                                                          | - Lacks detailed dynamic views<br>- May over-simplify<br>- Focuses mostly on static aspects                                                                       |
| **UML Diagrams**             | - Standardized<br>- Flexible (variety of diagrams)<br>- Visual communication<br>- Tool support                                                   | - High overhead in maintaining diagrams<br>- Steep learning curve<br>- Risk of over-specification<br>- Too low-level for high-level communication                  |
| **Architecture Decision Records (ADRs)** | - Captures rationale<br>- Simple to maintain<br>- Promotes transparency<br>- Helps avoid “decision drift”                                      | - No visual representation<br>- Can be lengthy over time<br>- Doesn’t capture runtime or performance aspects<br>- Quality varies with author skills                |
