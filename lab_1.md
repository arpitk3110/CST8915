### Youtube Video Link : https://youtu.be/3-hlDB-2VpQ 

## Technical Explanation of each service

### Order Service (Node.js)

The Order Service is responsible for handling customer orders placed through the store front. Built using Node.js, it acts as the backend that receives requests from the Vue.js front-end whenever a customer buys a product. Once an order is submitted, the service sends the order details to RabbitMQ, which queues them for processing. This approach allows the order system to remain responsive and scalable.

### Product Service (Rust)

The Product Service manages the product catalog of the pet store, including product names, prices, and stock availability. It is implemented in Rust using the Warp web framework, making it fast, safe, and efficient. Other services, like the store front and the order service, communicate with it through a REST API to fetch product details in real time.

### Store Front (Vue.js)

The Store Front is the main customer-facing website, developed using the Vue.js JavaScript framework. It provides an interactive interface where users can browse products, add items to their cart, and place orders. Behind the scenes, the store front communicates with the Product Service to display product information and with the Order Service to submit customer orders.

