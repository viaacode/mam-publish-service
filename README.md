# MediaHaven Publish Service

This service publishes fragments in MediaHaven based on their fragment id.
Publishes are sequentially and they wait for the object to actually be published
before responding.

## Usage

Run this project with the Mule Runtime and fill in the properties.

On RabbitMQ, publish a message in the following format:

### Request

```json
{
    "correlation_id": "abc123",
    "fragment_id": "9a3cb...acec50",
    "cp": "someorganisation"
}
```

The service will respond on the response queue with a message that looks like
the following:

### Response: success

```json
{
    "correlation_id": "abc123",
    "fragment_id": "9a3cb...acec50",
    "outcome": "success",
    "message": ""
}
```

### Response: failed

```json
{
    "correlation_id": "abc123",
    "fragment_id": "9a3cb...acec50",
    "outcome": "failed",
    "message": "Failed to publish fragment"
}
```

## Notes

Since the Mule AMQP connector refuses to create queues, this service uses the
RabbitMQ REST API to declare the queue before publishing to it.
