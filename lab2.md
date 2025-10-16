## YouTube demo video link : https://youtu.be/Q2gdqOdMZEw

## Links to the three service repositories :

 - order-service : https://github.com/arpitk3110/order-service-cst8915
 - product-service : https://github.com/arpitk3110/product-service-8915
 - store-front : https://github.com/arpitk3110/product-service-8915

 ## 1. What changes did you make to the order-service and product-service to comply with the Configurations and Backing Services factors of the 12-Factor App methodology?

 ## - Order-Service:

To comply with the Configurations and Backing Services factors of the 12-Factor App, all hard-coded values such as the RabbitMQ connection string and port number were replaced with environment variables stored in a .env file. The index.js file was updated to use the dotenv library to load these values at runtime. The order-service was configured to connect to RabbitMQ running on a dedicated Azure VM

## - Product-Service:
The hard-coded port number was replaced with an environment variable stored in a .env file, and main.rs was updated to use the dotenv crate to load it at runtime.


 ## 2. Why is it important to use environment variables instead of hard-coding configurations in your application?

Using environment variables instead of hard-coding configurations makes the application more flexible and portable across different environments (development, testing, production).It allows sensitive information like passwords or API keys to be kept secure and out of the codebase.

 ## 3. Why is it important to have separate repositories for each microservice? How does this help maintain independence and scalability of each service?

Having separate repositories for each microservice ensures that each service has its own codebase, version control, reducing dependencies between services.This independence allows teams to develop, test, and deploy services without affecting others, improving maintainability.

