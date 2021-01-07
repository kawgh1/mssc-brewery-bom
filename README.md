##### Part of John Thompson's Microservices course

# Default Port Mappings - For Single Host
| Service Name | Port | 
| --------| -----|
| [Brewery Beer Service](https://github.com/kawgh1/mssc-beer-service) | 8080 |
| [Brewery Beer Order Service](https://github.com/kawgh1/mssc-beer-order-service) | 8081 |
| [Brewery Beer Inventory Service](https://github.com/kawgh1/mssc-beer-inventory-service) | 8082 |

# MSSC Brewery BOM (Bill of Materials)

Since this BOM is not published to Maven Central - which is outside scope - this BOM is not being used for the microservices
- All Beer Works microservices will inherit from John Thompson's com.github.sfg-beer-works brewery-BOM so they can actually work with on the Cloud

- Still provides a good example of a local computer BOM implementation


# How to do a Maven release on Maven Central with ossrh
    - # mvn release:prepare -P ossrh
   - [John Thompson's lesson in Microservices](https://www.udemy.com/course/spring-boot-microservices-with-spring-cloud-beginner-to-guru/learn/lecture/17179256)
   - [John Thompson's Maven course](https://www.udemy.com/course/apache-maven-beginner-to-guru/)  
     
Deconstruction Process - 12/28/2020

	- Where We Are At
		- Beer Monolith has been broken down into 3 independent microservices
			- Beer Service, Beer Inventory and Beer Order Service
		- Each microservice is using its own in-memory database, its own repository
		- Setup inter-service communication for read operations
			- the Beer API is dependent on Beer Inventory to get inventory data 
			- the Beer Order Service is dependent on Beer Service to get data about the Beer objects themselves (descriptions, properties, etc.)
			- So we can see how these services are starting to work together

	- What's Not Working
		- Order Allocation is not working
		- Beer 'Brewing" is not working
		- Monolith was using Spring Events and scheduled jobs for these features
			- Events and consumers of events is broken
		- The 3 services are using Maven, but there is a high degree of duplication in Maven POM files


	- Next Steps
		- Establish a Maven BOM (Bill of Materials) to reduce duplication in Maven POMs
			- Also - good technique for standardization and compliance across the enterprise
			- By making all subsystems and microservice POMs dependent on a BOM, don't have to go in and update each one every time some version is updated or patched
		- Setup MySQL for database - will help with trouble shooting, and configuration for target deployment
		- Transaction to JMS (Java Message Service) to publish events as messages
		- Build Saga's to coordinate microservice actions for events
			- ie Order Allocation