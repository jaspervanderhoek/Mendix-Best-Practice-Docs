# Extension Pattern

Many of our core systems are in need of change, either because you're buying a system that doesn't fit your needs completely or because your business is evolving. 


### Reader note
*This document will talk about Core Systems, Monoliths, and legacy systems. When it comes to extending systems there are no difference between any of these types of systems. The extension pattern that we have identified applies to any large existing systems that you want to customize or enhance. This could even be your large Mendix monolithical system you've build 3 years ago and needs an extension.*


## Criteria
* Business Value 
* Implementation Effort 
* Maintenance Effort
* ROI

* Architectural agility


## Common Anti-Patterns 
In our software career we've been drilled to re-use everything and get the largest ROI out of our previous investments. It's very common that organizations are looking to get all their moneys worth out of that multi-million dollar investment of their Core system.  
As a result the system should re-use any of the data and logic that is already in the existing system. This is the most common anti-pattern. **Do not re-use your logic and data storage**, this will restrict you in your future ability to maintain or enhance this process.


## Best-Practice

Enhance your Core-system by leaving it as standard as possible (or make the minimal amount of changes).  
>*By limiting work in the core-system there are less to no chances on regression issues. Future upgrades will be significantly faster because of the lack of customization.*  

Bring all logic into the extension, this allows for the visibility and flexibilty in all logic.  
>*When changes are required in the future, only one system needs changing. This limits development effort and regression testing significantly. Any effort rebuilding logic or data structures will be saved later when working on new functionality*

Drive towards future flexibility and value. 
>*Introduce flexibility in your architecture to allow for future changes and adaptability. This will allow future project to be developed with less effort and faster response but also reduces the maintenance effort since the solution is less complex and less complexity means less work*  

Copy data into your extension application and use it as the system of record for the data for that process. 
>Copy the data into your extension application and allow the application to manage and enrich this data. Push any updates to the master data back but don't attempt to force having all data in a single place. Use (OData) interfaces for easy retrieve of all data. 



## Example



### Data Dump
 
to enhance or customize existing core-systems, such as ERPs but also TeamCenter, eBR, etc. Where we leave the core system as standard as possible and build (several) extensions next to it. These extentions include all functionality required to complete ‘the process’, this means replicating some data and logic to preserve the flexibility and speed that Mendix brings to the table. If you replicating data/logic you get a solution that has the benefit of the different technology, web and mobile capabilities, but doesn’t get the speed and flexibility that we promis. 
 
