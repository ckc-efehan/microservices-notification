# Notification Service

## Overview
This is a microservice that handles email notifications for an online shop. It listens to Kafka events when orders are placed and sends confirmation emails to customers.

## Features
- Consumes order events from Kafka
- Sends email notifications to customers when orders are placed
- Uses Spring Mail for email functionality
- Configurable email templates

## Technologies
- Java 21
- Spring Boot 3.4.4
- Spring Kafka
- Spring Mail
- Lombok
- Maven

## Prerequisites
- JDK 21
- Maven
- Kafka broker running on localhost:9092
- SMTP server for email (configured to use Mailtrap for testing)

## Installation and Setup

### Clone the repository
```bash
git clone https://github.com/yourusername/notification-service.git
cd notification-service
```

### Build the application
```bash
mvn clean install
```

### Run the application
```bash
mvn spring:boot run
```
Or run the JAR file directly:
```bash
java -jar target/notification-service-0.0.1-SNAPSHOT.jar
```

## Configuration

### Application Properties
The service can be configured through the `application.properties` file:

```properties
# Application settings
spring.application.name=notification-service
server.port=8090

# Mail Properties
spring.mail.host=your-smtp-host
spring.mail.port=your-smtp-port
spring.mail.username=your-username
spring.mail.password=your-password
spring.mail.protocol=smtp

# Kafka Config
spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=notificationService
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.springframework.kafka.support.serializer.JsonDeserializer
spring.kafka.consumer.properties.spring.json.type.mapping=event:com.online_shop.microservices.order_service.event.OrderPlacedEvent
```

## Usage
The service automatically starts listening to the "order-placed" Kafka topic when it runs. When an order is placed, it receives an event with the order number and customer email, then sends a confirmation email to the customer.

### Event Format
The service expects events in the following format:
```json
{
  "orderNumber": "12345",
  "email": "customer@example.com"
}
```

## Testing
The project includes test configurations using TestContainers for integration testing with Kafka.

To run the tests:
```bash
mvn test
```
