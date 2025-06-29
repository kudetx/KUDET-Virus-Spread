Simulation Tool:
AnyLogic Professional 8.7.11

Simulation/Programming Language:
Java (AnyLogic DSL)

Scenario of the Application
In this project, a simulation was developed to model passenger (pedestrian) flow within an airport. Passengers move through various areas such as terminal entrance, check-in, security control, restrooms and restaurants, waiting areas (either standing or seated), boarding gates, and terminal exits. By adding an epidemiological dimension to the simulation, the risk of infected individuals transmitting the disease to other passengers through contact was monitored. Additionally, this spread can be observed graphically

Defining the Goals of the Project
The aim of the project is to demonstrate and simulate pedestrian behavior and infection spread in an airport environment using an agent-based model. Through real-time graphical analyses, the time-dependent changes in the number of infected, at-risk, and healthy individuals were examined. The objective here is to observe which areas accelerate or slow down the spread of the virus and to monitor whether social distancing has an effect on virus transmission within the simulation environment.

System Definition
In this study, the developed simulation system represents an abstract digital model of an airport terminal.
The modeling does not aim to replicate the exact architecture of a real airport; rather, it focuses on logically representing functional elements such as passenger flow, service points, and epidemic spread. The main objective is to analyze the dynamics of a complex environment by constructing meaningful abstractions to observe human behavior and interactions within the system.
<img width="454" alt="image" src="https://github.com/user-attachments/assets/7cb699b9-9ce0-4243-824a-a8ecd632bc5f" />

<img width="320" alt="image" src="https://github.com/user-attachments/assets/3e580a2e-a420-43e4-b7f1-ecdb635af863" />

<img width="481" alt="image" src="https://github.com/user-attachments/assets/32399175-6601-4ce7-bff6-0f73cd28ee8d" />

<img width="501" alt="image" src="https://github.com/user-attachments/assets/b97f50d9-9cee-4e5c-887d-236c46b32553" />

System Components
The model consists of three main components:
1. Agents:
Individuals representing passenger movement are modeled using an agent class called Pedestrian. Each pedestrian is defined as a separate entity and carries their own state (healthy, infected, or at risk). With the agent-based approach, the independent movement and state transitions of each individual are monitored.
2. Static Objects:
Structures within the environment—such as walls (wall1–wall12), tables (table), security scanners (xrayScanner), detectors (detector), and personnel (officeWorker, policeman)—are defined as static entities. These objects help guide pedestrian flow and contribute to a realistic simulation atmosphere.
3. Logic/Flow Modules:
Directional blocks (e.g., selectOutput), waiting zones (pedWait, pedWait1), and service points (pedService, pedServiceSecurity) define how agents interact with their environment. Events such as infection transmission are controlled by background-running event blocks.

Defined Areas
Four main zones are defined in the simulation:
• Entrance Area:
Represented by pedSource, this area is the entry point for passengers into the system. Group arrivals triggered by the FlightArrival event also occur in this section.
• Service Area:
Passengers complete check-in and security procedures here using the pedServiceCheckin and pedServiceSecurity blocks. Through the selectOutput1 block, they are randomly directed to either the restroom (pedGoToWC) or restaurant (pedGoToRest).
• Waiting Area:
Represented by pedWait and pedWait1 blocks, passengers wait here until their flight time, possibly sitting (with the isSitting state activated). This area poses a high risk of infection transmission due to contact with infected individuals.
• Exit and Boarding Area:
Passengers proceed to the gate via pedGoTo and exit the system through pedSink after completing all procedures, finalizing the simulation loop.
 
Infection Dynamics
The core feature of the simulation is its ability to track infection spread between individuals on a per-agent basis. The following components support this mechanism:
• isInfected: Indicates whether the agent is infected.
• isAtRisk: Represents individuals who have been exposed to infected agents but are not yet infected.
• risk_Exposure: Numerically represents the level of contact intensity and potential risk accumulation.
Connections between agents are established using a connections collection and Links to Agentproperty. An infected individual can affect connected pedestrians at specified intervals. These interactions are governed by the infection algorithm defined in the event block.

Graphical and Numerical Monitoring
The model tracks not only movement but also the time-based changes in the number of individuals:
• TimePlot: Displays line charts of the number of at-risk, infected, and total individuals.
 
Diagram Explanation
The figure below presents a graphical representation of the entire simulation model. The upper part shows the physical layout of the terminal, while the lower part illustrates the logical flow of agents.
Figure 1. Overview of the Airport Terminal Simulation Model:
In the upper section, physical structures (e.g., restaurant, restroom, waiting area) are shown. The lower section includes the flow diagram that manages agent movements. The pedestrian movement chain, starting from pedSource and ending at pedSink, passes through functional steps such as check-in, security control, service points, and waiting zones.
Entities 
Within the simulation system, multiple types of entities are modeled, encompassing both physical components and behavioral agents. These entities are categorized into the following subgroups:
- Pedestrian
Pedestrian agents are the primary simulation entities in this model. They possess independent mobility, carry individual attributes, and interact with their surroundings.
-Each pedestrian agent can be initialized with a random health status (healthy or infected).
-They are directed to specific service points (check-in, security, restaurant, restroom, waiting areas), where they exhibit behavioral patterns by interacting with environmental objects.
-Infected agents can turn other individuals into “at risk” through physical proximity (via link connections).
-Each pedestrian is individually observable and represented by distinct visual forms (e.g., sitting, standing, infected, healthy, at risk).
 
Static Objects (Physical Elements)
Static objects defined in the model represent the physical environmental components that guide, serve, or accommodate the agents.
-Walls (wall1–wall12): Serve as guiding barriers and are used to restrict pedestrian movement paths.
-Seating Areas (oval1, oval2): Located in waiting zones, these allow agents to rest by switching between isSitting and !isSitting states.
-Tables and Scanners (table, xrayScanner, detector): Used in security control and restaurant services.
-Restaurant / Restroom: Areas to which agents are directed based on their needs (pedGoToRest, pedGoToWC).
 Source / Sink (Entry and Exit Points)
-pedSource: The object where new pedestrian agents enter the system at specified time intervals. It is typically triggered by the FlightArrival event.
- pedSink: The exit point where agents leave the system after completing their tasks. It represents the final stage of the model.

Attributes 
These are the core variables that enable pedestrian agents to exhibit different behaviors during the simulation process. Each variable is used for analyzing both individual agent behavior and the overall system dynamics:
• isInfected (boolean)
- A logical variable that indicates whether an agent is infected.
- Agents with true are sources of infection and can affect others upon contact.
- A few individuals are initialized as infected at the start of the simulation using this variable.
- This attribute is directly used in graphical displays and in the infection spread algorithm.
  
• isAtRisk (boolean)
-Indicates whether an agent has been in close contact with an infected individual.
-Agents connected to any infected pedestrian switch to isAtRisk = true.
-The number of at-risk individuals is statistically monitored, and they carry the potential to become infected.
 
• risk_Exposure (int)
-A numerical value representing the total amount of risk the agent has been exposed to.
-This value increases with each new contact.
-In the model, repeated exposure (e.g., being affected multiple times by the same infected agent) is tracked through this variable.
-In future versions, this value can be used to trigger automatic infection once it exceeds a certain threshold.
 
• isSitting (boolean)
- Indicates whether the agent is currently in a waiting or seating area.
- When true, the agent remains stationary and is visually represented in a seated position (e.g., using the person_sitting image).
- In waiting areas, agents sit for a random duration before standing up to be redirected.
 

Activities
In the simulation model, each pedestrian agent follows a specific sequence of behaviors. These behaviors define the agent’s journey within the system and its interactions with the environment. Below are the core activities represented in the model:
 
• Walking (pedGoTo, pedGoToWC, pedGoToRest)
This is the fundamental action that enables agents to move from one location to another.
• pedGoTo: General navigation activity that guides the agent toward a specified target.
• pedGoToWC: Directs the agent to the restroom area to meet sanitary needs.
• pedGoToRest: Guides the agent toward a restaurant or resting area.
Each movement is diversified through behavior algorithms and randomness-based decision blocks.
• Receiving Services (pedService, pedServiceCheck, pedServiceSecurity)
Pedestrian agents wait and receive services at designated service points:
• pedService: A general-purpose service unit used for check-in or other service types.
• pedServiceCheck: A specific service point dedicated to check-in operations.
• pedServiceSecurity: Area where security screening is performed.
The service duration is usually defined as fixed or based on a statistical distribution. These waiting periods play a critical role in infection spread.
• Waiting (pedWait, pedWait1)
This refers to periods where agents wait in line or remain stationary before receiving services.
• pedWait: Used in seated terminal waiting areas.
• pedWait1: Can represent standing waiting zones in the terminal.
While waiting, if agents enter a seating area, their isSitting attribute becomes active.
 
• Restaurant and Restroom Interaction (pedGoToRest) (pedGoToWC)
The model includes a separate restaurant and restroom section. These areas are divided by walls to reduce infection risk.
• Pedestrian agents are randomly directed to these areas for services.
• Once directed, agents remain in these zones for a predefined duration.
• The separation via walls is intended to limit the critical density and reduce disease transmission within these zones.
• Exit (pedSink)
Agents exit the system upon completing their journey within the simulation.
• This occurs after all tasks assigned to the agent are completed.
• It is handled through the pedSink module, after which the agent no longer appears in visual or statistical outputs.
• Connection Interaction (Links to Agent)
Although not an activity in itself, this is a core mechanism that triggers infection spread among agents.
• Agents located within the same area automatically establish connections with one another.
• These connections form the basis for the spread of infection risk.
 
Events
Within the simulation, the dynamic structure of the system is continuously updated through time-based and scenario-specific events. These events serve a wide range of functions—from altering agent states to calculating statistical data.
• event (Periodic Update Event):
This event is configured to execute every 1 minute throughout the simulation. Its primary purpose is to identify new at-risk individuals through interactions between infected agents and nearby individuals. The process is executed based on the following algorithm:
-Each infected individual (from the pedInfected collection) is checked for connections using getConnections().
-If a connected individual is not already at risk (!isAtRisk), they are marked as at-risk, and their risk_Exposure counter is incremented.
-This process is repeated every minute to simulate the spread of infection dynamically.
Additionally, this event:
-Calculates the number of infected individuals (pedInfected.size()),
-Determines the number of at-risk individuals (by counting unique agents with isAtRisk == true),
-Computes the number of healthy individuals (total - infected - at-risk),
and transmits these values to the TimePlot component for graphical visualization. This enables real-time data analysis without the need for textual output.
• FlightArrival (New Pedestrian Entry Event):
This event ensures the system remains dynamic by periodically introducing new pedestrian agents into the simulation.
-It is typically scheduled to run every 90 minutes.
-New agents are introduced into the system from the terminal’s entry point via the pedSource block.
-Additionally, it clears the waiting areas "pedWait" and "pedWait1" using the following code:
pedWait.freeAll();
pedWait1.freeAll();
These commands release the agents currently in waiting areas, allowing them to continue moving through the terminal. This ensures that the next group of passengers can access these areas.
-This structure simulates both fluid pedestrian behavior and realistic crowd management.
- Moreover, in terms of infection dynamics, introducing new agents and releasing previously waiting ones increases the system’s variability and realism.
 
State Variables 
For the purposes of simulation tracking and analysis, each agent's individual health status and social connection dynamics are represented through specific variables. These variables directly influence the functioning of the infection spread algorithm and the visualization of results.
 
• pedInfected:
A Collection<Pedestrian> that stores all currently infected individuals.
-At the beginning of the simulation, a predefined number of agents are manually added to this collection.
-Any individual who becomes infected through contact is also added to this collection.
-This collection allows real-time tracking of the number of infected individuals and is used directly in graphical analysis.
• connections (Links to Agent):
Each Pedestrian agent forms connections with others based on proximity criteria.
-These connections are modeled using Links to Agent objects.
-During event execution, the connections of infected agents are scanned to assess the risk of infection spread.
-This structure represents real-life social interactions and physical contacts.
• risk_Exposure:
An integer variable representing the number of contacts an individual has had with infected agents.
-This value is incremented by 1 for each new contact.
-A higher value indicates more frequent or closer interaction with infected agents.
-After a certain threshold, the individual is marked as "at risk" (isAtRisk = true).
• isAtRisk:
A boolean variable indicating whether an individual is considered at risk due to previous exposure to infected agents.
-If an individual has had contact with infected agents, this variable becomes true.
-At-risk individuals are not yet in pedInfected but are considered potential carriers.
-This value is visualized in the TimePlot as the “number of at-risk individuals”.
 



***System Behavior / How It Works***
The simulation begins at the terminal’s entry point using the pedSource block. This block introduces passengers into the system by generating 100 pedestrian agents per hour along a line defined by EntryLine.
-Each newly created Pedestrian agent is individualized with randomly assigned comfort speed, initial speed, and radius (size).
-Additionally, there is a 10% probability that the agent will be marked as infected and added to the pedInfected collection.
 

In the simulation, passengers follow the steps below as they move through the terminal:

<img width="454" alt="image" src="https://github.com/user-attachments/assets/f975eae0-7ac1-4e8c-b2c3-3cd455453223" />
 
Contact and Spread Mechanism: Infected individuals are modeled with social connections to surrounding agents using the connections list. The event block, which runs every minute, spreads the infection risk through these connections. The code below demonstrates this mechanism:
 

Time-Based Update: The periodic block named event runs every 1 minute and performs the following updates:
• The number of infected individuals (pedInfected.size()),
• The number of at-risk individuals (via the getRiskliSayisi() function),
• The number of healthy individuals (calculated as total – at-risk – infected),
These values are displayed in real-time on the graphical TimePlot component.
Flight Arrival Cycle: The FlightArrival event runs every 90 minutes and performs two main actions:
• Clears the waiting areas using pedWait.freeAll() and pedWait1.freeAll(),
• Allows the entry of a new flow of passengers into the system.
This mechanism ensures that the simulation remains dynamic through continuous passenger arrivals.
 
Exit Point: At the end of the simulation, individuals reach the pedSink block. When infected individuals exit the system at this point, they are removed from the pedInfected list and excluded from further infection calculations.
 
Graphical Outputs
Throughout the simulation, the TimePlot graph displays time-dependent curves of key variables under the titles:
 - Total People"
- "At Risk People"
- "Infected People"
This structure allows the simulation to observe both micro-level agent behaviors and macro-level infection spread trends.
System behaviors are modeled through time-sensitive events and interactions between individuals, and are visualized in detail.
<img width="320" alt="image" src="https://github.com/user-attachments/assets/52cadae7-0e5f-4e83-a35a-ca91934c5e01" />
<img width="293" alt="image" src="https://github.com/user-attachments/assets/a2ffe556-b01c-451d-96ed-8f13f81846e0" />
***System Model***
This section defines all the structural and visual components used within the simulation environment. The model includes various objects and links designed for agent guidance, interaction with physical elements, and visual representation.
Source and Sink Blocks
-pedSource: The entry point for pedestrian agents into the simulation. Operates along a defined EntryLine. Each agent’s initial speed, diameter, and infection probability are specified here. A random assignment (randomTrue()) determines whether an agent is infected at entry.
-pedSink: The endpoint where agents exit the system. If an agent is infected, it is removed from the pedInfectedcollection upon exiting.
 Movement and Service Modules
-pedGoTo, pedGoToWC, pedGoToRest: Modules that direct agents to general goals, toilets, or rest areas.
-pedServiceCheckin, pedServiceSecurity, pedService, pedService1, pedService2: Represent service points such as check-in counters, security control, and restaurants. Agents receive service for a defined duration.
-pedWait, pedWait1: Passive waiting modules where agents can sit or wait. These are cleared after each flight using freeAll().
Routing Blocks
-selectOutput, selectOutput1: Decision nodes that randomly route agents into alternative paths, creating variety in the flow pattern.
 Agent Visual Representation
-person, person_sitting: Two graphical states of the agent (standing or seated), activated depending on the isSitting attribute.
oval, oval1, oval2: Circular shapes that change based on health status:
o	oval → Healthy
o	oval1 → At risk
o	oval2 → Infected
Environmental and Fixed Objects
-wall1 – wall12: Structural barriers that define physical boundaries within the simulation. Agent movement is constrained by these.
-table: Represents desks or tables in service areas.
-image: Background image depicting the layout of the terminal.
-xrayScanner, detector, officeWorker, policeman: Static environmental elements placed to enhance realism and functional detailing.
**Analysis of Input**
In this simulation, the input parameters are configured to define pedestrian density and the initial conditions for infection transmission. The following elements are synchronized with the pedSource block:
Total Number of Entries:
Throughout the simulation, more than 1000 pedestrian agents are generated. These agents are introduced into the terminal at specific intervals via the pedSource block. The frequency of entries is modeled similarly to the real-world flight traffic pattern.
Arrival Interval:
A custom event named FlightArrival is triggered every 90 minutes to generate a new group of passengers. This event activates the pedSource, enabling a dynamic and continuous flow of agents into the system. As a result, crowd density varies over time, enhancing the realism of the simulation.
Initial Infection Rate:
Upon entry, each pedestrian is randomly assigned an infection status. The probability of a new agent being infected is approximately 10% (implemented using randomTrue(0.1)). This infection probability applies both at the beginning of the simulation and during every new entry event.
Initial Agent Attributes:
Each pedestrian agent generated via pedSource is initialized with core attributes such as speed, diameter, infection status (isInfected), and—if infected—the ability to establish connections with nearby agents via linksToAgent



**Analysis of Output**
During the simulation, the output data enables observation of how infection dynamics and pedestrian behaviors evolve over time. These outputs are presented to the user through both graphical and textual means:

TimePlot Chart:
The number of infected individuals (isInfected), at-risk individuals (isAtRisk), and healthy individuals is plotted in real time along a temporal axis. This chart provides users with the ability to track the progression of contagion trends throughout the simulation.

Graph Data:
The event block is triggered every simulated minute to update the graph. Newly infected individuals are added to the pedInfected list, and at-risk agents are identified through their connections. These results are dynamically reflected in the graphical output.

<img width="454" alt="image" src="https://github.com/user-attachments/assets/69df4744-0360-4a43-94aa-5cbbcc318555" />

<img width="468" alt="image" src="https://github.com/user-attachments/assets/d2c20fcd-4035-4ce5-9882-5a48102059c3" />

<img width="454" alt="image" src="https://github.com/user-attachments/assets/d79b8e6a-4277-445b-953d-df0355dd3bd4" />

<img width="454" alt="image" src="https://github.com/user-attachments/assets/1a87d672-b2b3-4d68-a2e5-419c7e56ca02" />

<img width="454" alt="image" src="https://github.com/user-attachments/assets/be80d099-5782-4c22-98fb-fc8f1122c7f7" />
 
<img width="454" alt="image" src="https://github.com/user-attachments/assets/bdc3b820-b792-4a92-9ceb-d904a189cc9f" />
 
Comments: In areas with high queue density, such as waiting zones, restaurants, and restrooms, infection statuses were monitored, and it was observed that the spread was higher in specific locations. For example, in queuing areas, the spread of infection increases as the queue gets longer. In restaurant and restroom areas, infection spread was observed to be minimal due to the presence of partition walls and the lack of long queues. In seated waiting areas, no transmission was observed since social distancing was maintained. However, in standing waiting areas, the rate of spread was very high due to the lack of adherence to social distancing. Finally, at the pedSink exit point, infection rates were very high, and transmission occurred during queueing for exit.

***Validation and Verification***
-The system successfully demonstrated the expected infection spread; the number of at-risk individuals consistently exceeded the number of infected individuals.
-The graphs were updated in real-time and remained consistent with the simulation scenario.

***Project Results***
This study has demonstrated that an agent-based modeling approach can effectively simulate both behavioral and epidemiological dynamics in an airport environment. By integrating individual pedestrian behaviors with infection transmission logic, the simulation created a realistic and observable model of crowd flow and disease spread.
The infection propagation mechanism—based on contact between agents—was clearly visualized through both graphical outputs (TimePlot) and text-based tracking, making it possible to monitor the evolution of infection and exposure levels in real time.
Simulation results revealed several important patterns:
-Infection risk was notably higher in queue areas and standing waiting zones, where physical distancing could not be maintained.
-In contrast, sitting areas, restaurants, and restroom zones exhibited minimal transmission, likely due to spatial separation (e.g., walls, booths) and lower agent density.
-The exit area (pedSink) showed a high infection ratio, suggesting significant transmission occurs during last-minute clustering at the end of the terminal journey.
Furthermore, the dynamic generation of passengers and the periodic update of infection statuses proved effective for simulating long-term and large-scale behavioral scenarios. The model was able to distinguish clearly between healthy, at-risk, and infected agents, allowing for fine-grained analysis.
This simulation framework could be adapted for other public spaces such as train stations, shopping malls, or event venues. It offers a valuable decision-support tool for epidemic preparedness, crowd management, and infrastructure planning in pandemic-sensitive contexts.



