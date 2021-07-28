# When to use a Microflow vs a Nanoflow

**Offline first:** always start with nanoflows.  
**Online application:** always start with nanoflows.


Using Nanoflows for all logic maximizes its benefits: Faster execution and response times & lower consumption of resources on the server. Nanoflows achieve these benefits because they execute client-side (in the browser), this means that there is no client-server communication happening when running a Nanoflow (1). Because there is no client-server communication the execution of actions is no longer delayed by transfer of data and instructions.   Additionally by executing actions client-side instead of server-side a smaller load is being placed on the server, this means that the same cloud node is capable of server a higher number of concurrent users. However you want to avoid creating an excessive load on the client, running too complex actions on the client device can create an unnecessarily big load on the device. This is a trade-off you need to be aware off. 


When choosing between placing your logic in a Nanoflow vs a Microflow you want to balance the benefits of either option, there is no best way to split this that covers all scenarios.
 
## Differences

### Faster execution (by using a Nanoflow):
Any message between the browser and server takes time, this increases with the amount of data that is being transferred and the latency between the client-device and the server. When choosing a Nanoflow there will be minimal to no communication. This is especially valuable and impactful with mobile devices, but can still have significant benefits on desktop devices too. 
  
### Faster execution (by using a Microflow):
Nanoflows will be faster in execution when the client already has all the data needed for the execution. If the nanoflow needs to retrieve additional data that isn't present yet a nanoflow will be less efficient.

Let's go through a use-case to clarify this:  
After the user submits a new invoice, your action needs to calculate the total revenue and update the portfolio. If we break down this action we see that we have an Invoice object in the UI that needs validation and saving, that in it self can be done in a Nanoflow. Next we need to calculate the total portfolio value, we can assume that this requires retrieving a large set of records before we're able to execute the aggregate calculation.  
Doing this in a nanoflow would require us to retrieve a large amount of data from the database, through the server into the client just to do a calculation, all in JavaScript (and memory). This requires the server to retrieve a large amount of data and transpose the structure to the client and this contradicts both our benefits; A large amount of data needs to be send to the server, this can significantly reduce response times; Retrieving and sharing a large data set can have a significant impact on the performance of the server.

If we look at the same situation as a microflow we take a small delay to communicate with the server, there are some resources consumed by the validation, when doing the retrieve and aggregation the runtime will optimize these actions by running the aggregate queries on the database, the data never leaves the database and never makes it to the browser.  When we compare the two scenarios, aggregating in a microflow will result in the retrieve and aggregate to be executed on the database and minimal data is transferred to the client. With the nanoflow a significant amount of additional data needs to be transferred from the database through the server to the browser after which we'll have to do an expensive set of iterations to calculate the aggregate.   

Conclusion: for actions limit the use of nanoflows to execute logic that applies to data that is already present in the browser.


### More reliability and security when using Microflows:
If you are executing logic in a nanoflow all logic is executed client side. While the platform doesn’t allow changes to be made in the nanoflow logic at runtime, there is still an inherent risk with executing logic only client side. A malicious person could always attempt to circumvent the logic and send data straight to the server. For example logic for payment processing should never be executed in a nanoflow to prevent exploitation of the logic. This means that if you are in a highly regulated space or working with financial transactions you more than likely will put more validation and process logic in microflows than when you'd build a simple data entry system with just 1 user role.
Nanoflows are a secure way of executing logic, however data can be send to the runtime without it passing through the nanoflow and you will need to make sure that your microflows validate enough.

Example: Validating the format and presence of a phone nr can be done in a Nanoflow, while an exploit can cause bad data this will have no impact. Validating and processing the amounts and account numbers in payments or invoices is something you want to do in a microflow to make sure that all logic is securely ran on the server where everything is in your control. 



## The best of both worlds: Combining Nanoflows & Microflows
A Nanoflow is able to call microflows, this way we can use the benefits of both technologies. Knowing the differences between the two technologies we can now design a process that is both fast and secure. 

You can split the logic between a Nanoflow and a Microflow if that creates performance benefits, when splitting your logic make sure you still follow the guideliness on splitting the logic. Don't split logic for the sake of splitting, the logic should support being split as well if it makes the logic harder to understand keep everything in the microflow. 

Interaction with the UX, opening or closing pages should take place in the Nanoflows. Attempt to keep the logic and validation separate from the interaction with the workflow. This will make it significantly easier to follow a process through all the logic and allows you to re-use logic more frequently.


1. Always start with a Nanoflow  
    Exception, if all your Nanoflow is doing is call a microflow and maybe close or open a page, skip the Nanoflow the extra step of a Nanoflow does not have any benefits. 
    

2. Follow the VLI pattern  
    Process **V**alidation should happen in the Nanoflow, no data needs to be transmitted until it's valid.   
    Execute all **L**ogic in a Nanoflow that processes small data sets and doesn't impact your security.  
    **I**nteraction with UX should ideally be placed in the Nanoflows and be annotated if this happens in the microflow.  

3.  




*(1). The exception to no client=server communication is when you explicitly trigger the execution of a microflow. At that point the client will reach out to the server and will load any additional data as instructed by the microflow.*