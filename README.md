# E-commerce Payment Service

## Overview

The **E-commerce Payment Service** is a Spring Boot-based microservice responsible for handling payment processing within the e-commerce platform. It integrates with payment gateways like Stripe to manage transactions, refunds, and payment confirmations.

## Features

- **Payment Processing**: Handles credit card and other payment methods securely using Stripe.
- **Transaction Management**: Manages transaction records, including successful payments and refunds.
- **Webhook Handling**: Listens for payment status updates from the payment gateway.
- **Integration with Other Services**: Communicates with the  [Auth-service](https://github.com/juansebstt/ecommerce-auth-service)  for user authentication and  [API-Gateway](https://github.com/juansebstt/ecommerce-api-gateway)  for routing requests.

## Technologies Used

- **Java 17**
- **Spring Boot**
- **Spring Web** - For building RESTful APIs.
- **Spring Security** - For securing endpoints and ensuring authenticated access.
- **Stripe API** - For payment processing.
- **Maven** - For dependency management and build tool.

## Prerequisites

Before running this service, ensure you have the following installed:

- **Java 17**
- **Maven** (for building the project)
- An active Stripe account for payment processing.

## Installation

1. Clone the repository:

    ```bash
    git clone https://github.com/juansebstt/ecommerce-payment-service.git
    ```

2. Navigate to the project directory:

    ```bash
    cd ecommerce-payment-service
    ```

3. Build the project using Maven:

    ```bash
    mvn clean install
    ```

4. Run the project:

    ```bash
    mvn spring-boot:run
    ```


## Configuration

The Payment Service uses a configuration file for managing properties like payment gateway settings. Here are the key configuration options from `application.yaml`:

```yaml
server:
  port: 8082

stripe:
  api-key: your_stripe_api_key
```

- Replace `your_stripe_api_key` with your actual Stripe API key.

## Usage

Once the Payment Service is running, it can handle payment-related requests through the following endpoints:

- **Create Payment**: `POST /payments`
- **Refund Payment**: `POST /payments/refund`
- **Webhook Listener**: `POST /payments/webhook`

### Sample Payment Request

```
POST /payments
Content-Type: application/json

{
  "amount": 5000,  # Amount in cents
  "currency": "usd",
  "source": "tok_visa",  # Token generated by Stripe.js
  "description": "Order payment"
}
```

## Inter-Service Communication

The Payment Service connects with the following microservices:

- **[E-commerce API Gateway](https://github.com/juansebstt/ecommerce-api-gateway)**:
    - The gateway routes payment requests to this service. It ensures that only authenticated users (via the Auth Service) can initiate payments.
- **[E-commerce Auth Service](https://github.com/juansebstt/ecommerce-auth-service)**:
    - The Payment Service requires JWT tokens issued by the Auth Service to validate user identities during payment processing.

## Testing

You can test the Payment Service using Postman or any other API client. For the payment request, you need to provide the payment details as shown above.

## Testing Webhooks

To test Stripe webhooks locally, follow these steps:

1. **Use Stripe CLI**: Install the [Stripe CLI](https://stripe.com/docs/stripe-cli) if you haven't already.
2. **Start the CLI**: Run the following command to start listening for events:

    ```bash
    stripe listen --forward-to localhost:8082/payments/webhook
    ```

3. **Trigger Events**: Use the Stripe CLI to trigger events for testing:

    ```bash
    stripe trigger payment_intent.succeeded
    ```

   This simulates a successful payment event.

4. **Check Logs**: You can check the logs of your Payment Service to see if the webhook was received and processed correctly.

## Testing Payment Data with Stripe

To test payment processing, you can use the following test card numbers provided by Stripe:

- **Visa**: `4242 4242 4242 4242`
- **MasterCard**: `5555 5555 5555 4444`
- **American Express**: `3782 8224 6310 005`

### Sample Payment Request for Testing

```
POST /payments
Content-Type: application/json

{
  "amount": 5000,  # Amount in cents ($50.00)
  "currency": "usd",
  "source": "tok_visa",  # Use the token generated from Stripe.js or a test token
  "description": "Test payment"
}
```

### Note:

- Ensure you set your Stripe account to "Test Mode" when running tests to avoid real charges.
- In case of any issues, check the Stripe Dashboard for logs and error messages related to your API calls.

## Contributing

Feel free to submit pull requests or open issues if you find bugs or want to contribute to the project.