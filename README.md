# System Integration 2020 Exam Project

## Links

* [Assignment PDF Link ](sieksamen.pdf)
* [Video Link](#href)

* * *

**Authors**

- Morten Feldt
- Nur Salem
- Jörg Oertel

* * *

**Business Case**  
Assumed we are a travel agency, that so far only have email and phone as contact possibilities for customers.
Now we want a kind of chat or messenger service on our website.
This should help us to get more quickly response to customers when they have any questions, and then have to make more sales for our business for hotel, airport or attractions as tourist.
* * *

**Problem Definition**  
A travel agency has contacted our IT company DevOrgs and asked for the possibilities to get developed a chat/messenger service for their website.
This service should improve the answer time of questions, and then get some more sales to the travel agency company.
DevOrgs has accepted the request from the travel agency and then asked our team to develop this feature and solution for the travel agency company.

The travel agency has set up the following requirements for the feature:
1.	Empty messages should not be allowed – Error message should be show to customer.
2.	Messages should be written to only one of the following branches: Hotel, Airplane, Tourist.
3.	Customer should type in: Name, Year of birth, Destination beside the message and branch.
4.	Only messages contain all information as in points 2 and 3 is allowed – otherwise an error message should be shown.
5.	It should be possible to get messages in different format, ex. XML and JSON from the customer.
6.	Messages should be converted to JSON at the end in the travel agency employee.
* * *

**Problem Solution**  
Our team solution to the project was to create some microservices that all together would be the solution of the feature.  
It was build by an Eureka Discovery server, that instances of microservices could connect to, and then with Feign each of the microservices would be able to communicte with eachother.  
An gateway was implemented as API Gateway to act as frontdoor for all request to the system, and then it would master the client-side load balancing by the Ribbon technology to parse the request.  
The request was then entered in a Kafka message broker, where it would be translated and routed by three branches: hotel, airport, tourism.  
We have implemented a REST API, where hotels are stored, and then if the branch, from above, is equal to hotel, an information about hotel from the REST API i connected to the messages.
THe message is also consumed by the API Gateway to retrieve messages from all branches above. 

<img src="./images/chat_service_flow.png" style="float: left; margin-left: 20px;" />  

* * *

**Tecnology used**

- Eureka
- Feign
- Ribbon
- REST

* * *

**Architecture**  
Our architecture is build in that way, that we have an UI that communicate with the API Gateway.  
This way is used for request to the system, then it could either send a message or retrieve a message.  
If it want to send message it would go further into Kafka message broker, that translate and route the message to the right end base don topic.  
In the end the message is logged in a file for later information.

<img src="./images/chat_service_architecture.png" style="float: left; margin-left: 20px;" />  

* * *

**Instructions**

* * *

**Diagramms**

<img src="./images/message_transmission.png" style="float: left; margin-left: 20px;" />

* * *

<img src="./images/enterprisepattern.png" style="float: left; margin-left: 20px;" />

* * *

<img src="./images/chat_service_flow.png" style="float: left; margin-left: 20px;" />  

* * *

<img src="./images/chat_service_architecture.png" style="float: left; margin-left: 20px;" />  

* * *