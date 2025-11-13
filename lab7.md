## YouTube demo video link:

https://youtu.be/zJs0PWVoTro


### 1. Whether RabbitMQ is a stateless or stateful application 

RabbitMQ is a stateful application because it stores important data such as message queues, user credentials, and routing information. These states must remain consistent even if the pod restarts or moves to another node. Without saving this state, RabbitMQ would lose all pending messages. That’s why it requires persistent storage to work reliably in Kubernetes.

### 2. The implications of running RabbitMQ without persistent storage

Running RabbitMQ without persistent storage means that all the data inside it—like message queues and configurations—will be lost if the pod is restarted. This can cause message loss and disrupt communication between services. Applications depending on RabbitMQ will fail to receive or process pending messages. In real-world systems, this can lead to inconsistent transactions or data loss.

### 3. What happens when the RabbitMQ pod is deleted or restarted

When the RabbitMQ pod is deleted or restarted without any volume attached, Kubernetes will create a fresh pod with no previous data. This means all the queues, messages, and user settings are erased and RabbitMQ starts as a blank instance. As a result, any in-transit or undelivered messages are permanently lost, affecting the stability of connected microservices.

### 4. Potential solutions to this problem (research-based)

A good solution is to configure PersistentVolumeClaims (PVCs) for RabbitMQ so that its data remains stored even if the pod is recreated. Deploying RabbitMQ as a StatefulSet instead of a Deployment also ensures stable network names and dedicated storage. This approach provides data durability and better fault tolerance. Additionally, using clustered RabbitMQ nodes can further improve reliability.

### 5. Does using Azure Service Bus solve the issues identified with RabbitMQ Configuration in this Lab?

Yes, using Azure Service Bus can effectively solve these issues because it is a fully managed cloud messaging service with built-in reliability and persistence. It automatically takes care of message storage, delivery retries, and scaling without needing manual volume configuration. Unlike RabbitMQ in Kubernetes, Azure Service Bus won’t lose data when pods restart. This makes it a more resilient option for cloud-based applications.