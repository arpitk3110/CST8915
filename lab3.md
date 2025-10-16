## YouTube demo video link : https://youtu.be/wIWmVl4cAmc

## Links to the three service repositories:


### order-service - https://github.com/arpitk3110/order-service-cst8915

### product-service - https://github.com/arpitk3110/product-service-8915-py

### store-front - https://github.com/arpitk3110/store-front-cst8915


### Questions:

1. What challenges did you encounter when configuring environment variables in the GitHub Actions workflow?

   - One of the challenges was making sure the environment variables were placed in the right part of the GitHub Actions YAML file. At first, it was confusing to figure out which job they should go under and how the syntax had to be indented properly. Another issue was remembering to replace the placeholder URLs with the actual Azure Web App URLs, otherwise the store-front couldn’t connect to the backend services.

2. How does deploying microservices on Azure Web App Service differ from running them locally?

   - When running services locally, everything is directly under my control, and I can rely on local .env files and localhost URLs to connect components. On Azure Web App Service, deployment requires configuring environment variables in the portal, setting up the correct runtime stack, and relying on Azure to manage scaling and infrastructure. Another difference is debugging—locally I can quickly check logs in the terminal, while on Azure I have to use the portal or Azure CLI to see logs and troubleshoot.

3. Why is it important to use environment variables for configurations in a cloud environment?

   - Using environment variables is important in the cloud because it separates sensitive or changing configuration values (like connection strings, API URLs, or keys) from the application code. This makes the application more secure and portable since the same code can run in different environments just by changing the variables. It also helps avoid hardcoding values into the codebase, which reduces mistakes and makes scaling or updating services much easier.