# Jira Webhook Consumer Lambda
Lambda to pull messages from SQS and pass to API service for processing


## Why This Service is Necessary

This service acts as a critical integration bridge between JIRA and the Service API, providing reliable, asynchronous processing of JIRA webhook events. Here's why it's essential:

### 1. **Reliable Event Processing**

- **SQS Integration**: Receives JIRA webhook events through AWS SQS, ensuring message durability and preventing data loss
- **Retry Mechanism**: SQS provides built-in retry capabilities for failed message processing
- **Dead Letter Queue Support**: Failed messages can be routed to dead letter queues for investigation

### 2. **Asynchronous Decoupling**

- **Non-blocking Operations**: JIRA doesn't need to wait for Service responses, improving JIRA performance
- **Independent Scaling**: The Service API and JIRA can scale independently without affecting each other
- **Fault Tolerance**: If the Service API is temporarily unavailable, messages remain queued for later processing

### 3. **Event Routing and Transformation**

- **Event Type Handling**: Processes different JIRA issue events (`issue_created`, `issue_updated`, `issue_assigned`, `issue_generic`)
- **Endpoint Routing**: Routes events to appropriate Service API endpoints based on event type
- **Data Transformation**: Standardizes JIRA webhook payloads for consumption by the Service API

### 4. **Scalability and Performance**

- **Auto-scaling**: AWS Lambda automatically scales based on SQS queue depth
- **Cost Efficiency**: Pay-per-execution model with no idle resource costs

### 5. **Monitoring and Observability**

- **Structured Logging**: Comprehensive logging for debugging and monitoring
- **Event Tracking**: Tracks message processing with unique message IDs and issue keys
- **Error Handling**: Graceful error handling with detailed error logging

## How It Works

1. JIRA sends webhook events to an SQS queue through Producer Lambda: https://github.com/mikemalkhasyan/jira-webhook-queue-producer
2. This Lambda function is triggered by SQS messages
3. The function processes each message, extracting JIRA issue data
4. Based on the event type, it forwards the data to the appropriate Service API endpoint
5. Success/failure responses are logged for monitoring and debugging

This architecture ensures that JIRA-to-Service API integration is reliable, scalable, and maintainable.
