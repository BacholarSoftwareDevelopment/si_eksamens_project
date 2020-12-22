# System Integration 2020 Exam Project

**Authors**

- Morten Feldt
- Jörg Oertel

**Links**

- [Assignment PDF Link ](sieksamen.pdf)
- [Video Link](https://youtu.be/yo55DGu1Cns)

**Project Links**

-  [si_eksamens_registry_discovery](https://github.com/BacholarSoftwareDevelopment/si_eksamens_registry_discovery/blob/main/README.md)
-  [si_eksamens_gateway](https://github.com/BacholarSoftwareDevelopment/si_eksamens_gateway/blob/main/README.md)
-  [si_eksamens_hotels](https://github.com/BacholarSoftwareDevelopment/si_eksamens_hotels/blob/main/README.md)
-  [si_eksamens_producer](https://github.com/BacholarSoftwareDevelopment/si_eksamens_producer/blob/main/README.md)
-  [si_eksamen_consumer](https://github.com/BacholarSoftwareDevelopment/si_eksamen_consumer/blob/main/README.md)

---

**Technology used**

- Eureka
- Feign
- Ribbon
- REST
- Spring boot
- Kafka

---

**Instructions on how to run the project**

1. Run **Eureka Discovery Server** for activate Eureka Discovery Server on port 8761. 
2. Run **API-Gateway** for activate API-Gateway on port 8080.
3. Run **Hotels** for activate WebService about hotels.
4. Run **Kafka Zookeeper and Kafka broker** if you don't have kafka already running please use the docker-compose-yml file provided in [si_eksamens_producer](https://github.com/BacholarSoftwareDevelopment/si_eksamens_producer/blob/main/README.md)
5. Run **Producer** for activate producer of message broker.
6. Run **Consumer** for activate consumer of messages based on id and topic.

**Adding more brokers to the Kafka-Cluster**

1. Duplicate the existing properties file for each new broker:
    * cp config/server.properties config/server-1.properties
    * cp config/server.properties config/server-2.properties
2. edit these new files and set the following properties:

`
//config/server-1.properties:
broker.id=1
listeners=PLAINTEXT://:9093
log.dir=/tmp/kafka-logs-1
 
//config/server-2.properties:
broker.id=2
listeners=PLAINTEXT://:9094
log.dir=/tmp/kafka-logs-2
`

3. start those kafka-brokers:

`
bin/kafka-server-start.sh config/server-1.properties
 
bin/kafka-server-start.sh config/server-2.properties
`

## Run following in **PostMan** for **produce** message:

**URL:**  

http://localhost:8080/message  

**Request:**  

POST  

**Header - Key:**  

Content-Type  

**Header - Value:**  

application/json 

**Body:**  

id - number  
topic - hotel, airport, tourism  
name - text  
city - text  
message - text  

**EXAMPLE**  
{  
    "id":1,  
    "topic":"hotel",  
    "name":"Morten",  
    "city":"København",  
    "message":"Hi, I am asking for hotels in København"  
}  

---

## Run following in **PostMan** for **consume** message:

**URL:**  

http://localhost:8080/message/{id}/{topic}  

**Request:**  

GET  

**URL parameters:**  

id - number  
topic - hotel, airport, tourism  

**EXAMPLE**    

http://localhost:8080/message/1/hotel

**If you choose topic **hotel** in step 7 above, you will see an information from Web about hotel id and name from Webservice.**

**You can also choose to follow those to links for your convenience**

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/940d466c03cba116b397)

https://www.getpostman.com/collections/940d466c03cba116b397


---

**Business Case**  

There is a travel agency, that so far only provides an email and phone service for their customers, if they want to get in touch with them. Now they want to implement a chat  service on their website. This should help them to increase response time and customer can be reached more quickly and more efficient. Questions are categorized by topics and for a specific city 

    • Hotels
    • Tourism
    • Airport

Because of this new way of communicating with their customers, they will be more efficient in their task to provide good customer service. It also provides a more detailed information flow, because of those three topics. Not only their business can profit from it, but also the various hotels, restaurants or airports their send their customer to. [1.0 business model]
The next step would be to make deals with hotels and restaurants to get a percentage for each walk-in they get because of them.

---

<img src="./images/business_model.png" style="float: center; height: 30%; width: 30%;" />

<i style="font-size:15px;">This diagram shows the new intended business model of the travel agency</i>

---

**Problem Definition**  

A travel agency has contacted our IT company DevOrgs and asked us for developing a chat/messenger service for their website.  
DevOrgs has accepted the request from the travel agency and asked our team to develop this feature for their travel agency company.
The travel agency has set up the following requirements for the feature:

    1. Empty messages should not be allowed – Error message should be shown to the customer, agency.
    2. Messages should be written to only one of the following branches: Hotel, Airplane, Tourist. 
    3. Customer should beside the message provide their name, id, city and branch. 
    4. Only messages contain all information as in points 2 and 3 are allowed – otherwise an error message should be shown. 
    5. It should be possible to send messages in different formats:  XML and JSON
    6. Messages should be converted to JSON if received by the client or travel agency. 

---

<img src="./images/message_flow.png" style="float: center; height: 20%; width: 20%;" />

<i style="font-size:15px;">This diagram shows the message flow if message is delivered successfully or an error occurred</i>

---

**Problem Solution**  

Our team solution to the project was to create various micro-services that together would be the solution of the feature.
It was build by an Eureka Discovery server, that instances of micro-services could connect to, and then with Feign each of the micro-services would be able to communicate with each-other.
An gateway was implemented as API Gateway to act as front-door for all request to the system, and then it would master the client-side load balancing by the Ribbon technology to parse the request.
The request was then entered in a Kafka message broker, where it would be translated and routed by three branches: hotel, airport, tourism.
We have implemented a REST API, where hotels are stored, and then if the branch, from above, is equal to hotel, an information about hotel from the REST API i connected to the messages. The message is also consumed by the API Gateway to retrieve messages from all branches above.

---

<img src="./images/chat_service_flow.png" style="float: center; height: 60%; width: 60%;" />

<i style="font-size:15px;">The diagram shows the flow of the planned chat service</i>

---
  

<img src="./images/chat_service_architecture.png" style="float: center; height: 30%; width: 30%;" />

<i style="font-size:15px;">The diagram shows the architecture of the planned chat service</i>

---

<img src="./images/message_transmission.png" style="float: center; height: 60%; width: 60%;" />

<i style="font-size:15px;">On this diagram we wanted to show how a message travels between two applications and Kafka </i>

---

<img src="./images/enterprise_pattern.png" style="float: center; height: 60%; width: 60%;" />

<i style="font-size:15px;">Enterprise pattern: Content_base_router with a translator for transforming messages </i>

---
