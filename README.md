1) Wikimedia Producer Spring Boot Project Setup and Implementation: 
Objective: Build a Spring Boot Kafka producer application that streams real-time changes from Wikimedia and publishes them to a Kafka topic.

Implementation Details:

Project Setup: Created a Spring Boot application with Kafka dependencies (Spring Kafka, LaunchDarkly EventSource for SSE support).

Kafka Topic Creation: Configured Kafka topic for Wikimedia real-time event data using either a Kafka Admin client or external script (kafka-topics.sh).

SSE Integration: Used EventSource from LaunchDarkly to connect to the Wikimedia EventStreams endpoint (SSE URL).

Message Streaming: Implemented a custom WikimediaChangesHandler class that listens to incoming SSE events and sends each message to Kafka using KafkaTemplate<String, String>.

Execution: Initiated the EventSource within an @EventListener(ApplicationReadyEvent.class) or similar lifecycle hook to begin streaming on application startup.

Professional Insight: This design decouples event ingestion from downstream consumers, enabling scalable real-time processing of Wikimedia data.

2) Kafka Consumer Spring Boot Project Setup: 
Objective: Create a dedicated Spring Boot Kafka consumer application to receive and process data published by the Wikimedia producer.

Implementation Details:

Project Initialization: Set up a separate Spring Boot service with Kafka and JPA dependencies.

Kafka Listener Configuration: Defined consumer group ID, deserialization strategy, and bootstrap servers in application.properties.

Database Integration: Added MySQL database configuration and created entity classes for data persistence.

Professional Note: Modularizing the producer and consumer services follows microservices best practices, supporting independent scaling and maintenance.

3) Kafka Consumer Implementation: 
Objective: Consume Wikimedia change messages and trigger appropriate downstream actions.

Implementation Details:

Kafka Listener: Used @KafkaListener(topics = "${wikimedia.topic.name}", groupId = "myGroup") to consume messages.

Deserialization: Consumed raw JSON or plain text, depending on producer format.

Processing Logic: Implemented business logic in the listener method to parse, log, or transform the incoming data.

Professional Perspective: Proper error handling, logging, and idempotency checks were incorporated to ensure robustness and observability.

4) Save Wikimedia Data into MySQL Database: 
Objective: Persist real-time Wikimedia change data into a relational database for analysis, history tracking, or auditing.

Implementation Details:

Entity Definition: Created a WikimediaData entity with fields like id and wikiEventData using JPA annotations.

Repository Layer: Used Spring Data JPA's JpaRepository to abstract DB operations.

Persistence Logic: Integrated the repository into the consumer logic to save incoming messages directly into MySQL.

Technical Impact: Enables long-term storage and querying of event data, allowing for insights, reporting, or dashboarding.

5) Refactor Code to Externalize the Topic Name – Remove Hardcoded Values: 
Objective: Enhance maintainability and environment flexibility by moving Kafka topic name configuration out of source code.

Implementation Details:

Config Externalization: Moved hardcoded topic name to application.properties or application.yml using a key like wikimedia.topic.name.

Dynamic Injection: Injected topic name using Spring’s @Value("${wikimedia.topic.name}") annotation.

Config Management Ready: Made the service compatible with centralized config management tools like Spring Cloud Config.

Professional Justification: This approach supports environment-specific configurations (dev, QA, prod), aligns with 12-Factor App principles, and simplifies deployment automation.

