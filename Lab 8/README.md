## YouTube demo video link: https://youtu.be/RB-cSP6l_5k 

## Written explanation of solution to Task 2:

In Task 2, I configured persistent storage for both MongoDB and RabbitMQ.

MongoDB is now using a PersistentVolumeClaim and runs as a replica set, ensuring data is not lost during pod restarts.

RabbitMQ has also been updated to use a PersistentVolumeClaim so message queues remain saved even if the container restarts.

These changes make both services stateful and reliable during failures or deployments.