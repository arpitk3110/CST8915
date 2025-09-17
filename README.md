# Lab 2 - CST8915 Full-stack Cloud-native Development
In this lab, you will:
1. Refactor the [**Algonquin Pet Store**](https://github.com/ramymohamed10/Lab1_25F_CST8915_Algonquin_Pet_Store) application to comply with the first four factors of the [**12-Factor App**](https://12factor.net/): **Codebase**, **Dependencies**, **Configuration**, and **Backing Services**.
2. Deploy each component of the application on its own Azure VM: 
   - **RabbitMQ** on a dedicated VM (as the backing service) 
   - **order-service** on a dedicated VM  
   - **product-service** on a dedicated VM  
   - **store-front** on a dedicated VM  
3. Configure all services to communicate across VMs using environment variables, ensuring that the `order-service` connects to the external `RabbitMQ` instance.  

## Refactoring for 12-Factor Compliance
### Factor 1: Codebase
A single codebase should be used, with version control, for each microservice.

1. **Create a separate GitHub repository** under your GitHub account for each service.
   - For example, create three repositories:
      - `order-service`
      - `product-service`
      - `store-front`
2. **Copy the content** for each service from the existing codebase on [Algonquin Pet Store](https://github.com/ramymohamed10/Lab1_25F_CST8915_Algonquin_Pet_Store).
   - The `order-service` code is found in the `order-service` directory.
   - The `product-service` code is found in the `product-service` directory.
   - The `store-front` code is found in the `store-front` directory.
3. **Push each service** to its respective repository. This way, each service will have its own codebase, tracked separately.
### Factor 2: Dependencies
Explicitly declare and isolate dependencies.
1. **Ensure all dependencies are declared in a dependency management file.** 
   - For the **order-service (Node.js)**, ensure all required modules are listed in the `package.json` file.
   - Dependencies for the **product-service (Rust)** should be explicitly declared in the `Cargo.toml` file.
    - Generally, you need to include a manifest file that declares the dependencies of your app. 
      - For example, if you rewrite `product-service` in Python, you must ensure a `requirements.txt` file is present.
   
   The **Algonquin Pet Store App** services are **already compliant**. All dependencies for each service are properly declared.
2. **Use dependency isolation.** 
   - **Node.js (Order-Service)**: Use `npm install` to install dependencies in the `node_modules` directory and avoid using global dependencies. This ensures that all modules are isolated to the application and not globally installed on the machine.
      - The `node_modules` folder will be created locally within the project directory, ensuring dependencies are specific to the app.
   - **Rust (Product-Service)**: In Rust, dependencies are isolated by using the **Cargo** package manager. When you run `cargo build` or `cargo install`, it installs dependencies locally within the `target` directory. 
      - The `target` folder will be created locally within the project directory, ensuring dependencies are specific to the app.
   - **Python**:
     - In Python, to ensure dependency isolation, create and activate a **virtual environment** for the project. This prevents global installation of libraries and ensures all dependencies are specific to the project.
     - Create a virtual environment using `venv`:
       ```bash
       python -m venv venv
       ```
     - Activate the virtual environment:
       - On macOS/Linux:
         ```bash
         source venv/bin/activate
         ```
       - On Windows:
         ```bash
         venv\Scripts\activate
         ```
     - Install dependencies using `pip` and ensure they are listed in `requirements.txt`:
       ```bash
       pip install -r requirements.txt
       ```
      - A `.venv` folder will be created locally within the project directory, ensuring dependencies are specific to the app.
      - This isolates Python dependencies to the virtual environment, preventing any conflicts with system-level libraries.
### Factor 3: Configurations
Store configuration in the environment.

Extract any hard-coded configuration from the code (such as database URLs, RabbitMQ connection strings, ports, etc.) into environment variables.

1. **order-service**
   - Configure the **order-service** to use environment variables instead of hard-coded values.
   - You have been provided with an updated [index.js](order-service-algonquin-pet-store/index.js) file for the **order-service**, located within folder: `order-service-algonquin-pet-store`. 
   - Navigate to your **order-service repository** and locate the `index.js` file. Open the file and **replace its content** with the content in the updated [index.js](order-service-algonquin-pet-store/index.js) file.
      - This updated file ensures that configuration values like the **RabbitMQ connection string** and **port number** are retrieved from environment variables instead of being hard-coded.
      - The updated `index.js` file now uses the `dotenv` library to load environment variables from a `.env` file **during development only**.
   - You need to create a `.env` file in the root directory of your order-service. This file will store all environment-specific configurations, such as the RabbitMQ connection string and the port number. The `.env` file should not be included in version control (Git), so make sure you add it to `.gitignore`.
   - Here’s what the content of your `.env` file should look like:
      ```bash
      # RabbitMQ connection string
      RABBITMQ_CONNECTION_STRING=amqp://your_username:your_password@<VM_IP>:5672/

      # Port for the order-service to listen on
      PORT=3000
       ```

      - RABBITMQ_CONNECTION_STRING: Replace your_username, your_password, and <VM_IP> with the appropriate values for your RabbitMQ server. This connection string tells the order-service where RabbitMQ is running.
      - PORT: The port number on which your order-service will run. If you do not define this, the service will default to port 3000.
   - You will need to install the dotenv package to load environment variables from the .env file into your application. Run the following command in your order-service directory:
      ```bash
      npm install dotenv
      ```
   - Once the `.env` file is set up and the dotenv package is installed, restart your order-service and test it to ensure it is correctly using the environment variables.

2. **product-service**
   - Configure the **product-service** to use environment variables instead of hard-coded values.
   - You have been provided with an updated [main.rs](product-service-algonquin-pet-store/main.rs) file for the **product-service**, located within the folder: `product-service-algonquin-pet-store`. 
   - Navigate to your **product-service repository** and locate the `main.rs` file. Open the file and **replace its content** with the content in the updated [main.rs](product-service-algonquin-pet-store/main.rs) file.
      - This updated file ensures that configuration values like the **port number** are retrieved from environment variables instead of being hard-coded.
      - The updated `main.rs` file now uses the `dotenv` crate to load environment variables from a `.env` file **during development only**.
   - You need to create a `.env` file in the root directory of your product-service. This file will store environment-specific configurations such as the port number. The `.env` file should not be included in version control (Git), so make sure you add it to `.gitignore`.
   - Here’s what the content of your `.env` file should look like:
      ```bash
      # Port for the product-service to listen on
      PORT=3030
      ```

      - `PORT`: The port number on which your **product-service** will run. If you do not define this, the service will default to port 3030.
   - You will need to add the `dotenv` crate to your **Cargo.toml** file to load environment variables from the `.env` file into your application. Add the following line to your `Cargo.toml` dependencies:
      ```toml
      [dependencies]
      dotenv = "0.15"
      ```
   - Once the `.env` file is set up and the `dotenv` crate is added, run `cargo build` to install the dependencies and restart your **product-service**. Test it to ensure it is correctly using the environment variables.

3. **store-front**
   - Configure the **store-front** to use environment variables instead of hard-coded values.
   - You have been provided with an updated [**`src`**](store-front-algonquin-pet-store/src) folder that you should use to replace the **`src`** folder of your **store-front** Vue.js application in your repository. This updated folder will include code that utilizes environment variables instead of hard-coded values.

   - In a Vue.js application, you can use `.env` files to store environment-specific configurations. Vue.js requires environment variable names to start with `VUE_APP_`.
   - Create a `.env` file in the root directory of your **store-front** application. This file will store all environment-specific configurations, such as API URLs for the order-service and product-service. The `.env` file should not be included in version control (Git), so make sure you add it to `.gitignore`.
   - Here’s what the content of your `.env` file should look like:
      ```bash
      # API URL for the order-service
      VUE_APP_ORDER_SERVICE_URL=http://localhost:3000

      # API URL for the product-service
      VUE_APP_PRODUCT_SERVICE_URL=http://localhost:3030
      ```

      - `VUE_APP_ORDER_SERVICE_URL`: Replace this with the correct URL for your **order-service** (e.g., your deployed service URL in development).
      - `VUE_APP_PRODUCT_SERVICE_URL`: Replace this with the correct URL for your **product-service** (e.g., your deployed service URL in development).
      - In your Vue.js application code, use `process.env` to access these environment variables. For example:
         ```javascript
         const orderServiceUrl = process.env.VUE_APP_ORDER_SERVICE_URL;
         const productServiceUrl = process.env.VUE_APP_PRODUCT_SERVICE_URL;
         ```
      - Once the `.env` file is set up, run your development server to ensure it is correctly using the environment variables:
         ```bash
         npm run serve
         ```
      - To use environment variables in production, you can create a `.env.production` file with production-specific values, like this:
         ```bash
         # Production API URL for the order-service
         VUE_APP_ORDER_SERVICE_URL=https://api.production.com/order-service

         # Production API URL for the product-service
         VUE_APP_PRODUCT_SERVICE_URL=https://api.production.com/product-service
         ```
      - To build the application for production, run:
         ```bash
         npm run build
         ```
      - Test your Vue.js application to ensure it is correctly using the environment variables in both development and production environments.
### Factor 4: Backing Services
Treat backing services (like databases, message brokers) as attached resources.

- In this section, you will configure RabbitMQ as a backing service for your application by using a RabbitMQ instance hosted on an Azure VM. 

#### Steps to Set Up RabbitMQ on Azure VM

1. **Deploy RabbitMQ on Azure VM**:
   - Create a VM and follow the steps described in lab 1 to setup RabbitMQ on the VM: [**RabbitMQ Instructions**](https://github.com/ramymohamed10/Lab1_25F_CST8915_Algonquin_Pet_Store/tree/main/RabbitMQ) 
 
   - Ensure that **ports 5672** (used by RabbitMQ) and **15672** (used by the RabbitMQ Management UI) are open and accessible on the VM. You can do this by configuring the **network security group (NSG)** associated with your VM in the Azure portal.


4. **Retrieve the RabbitMQ Connection String**:
   - After creating the user, you can form your RabbitMQ connection string.
   - The connection string format is:
     ```bash
     amqp://your_username:your_password@<VM_IP>:5672/
     ```
     Replace `your_username` and `your_password` with the credentials you created for RabbitMQ, and `<VM_IP>` with the public IP address of your RabbitMQ Azure VM.

5. **Configure Your Application to Use RabbitMQ**:
   - In your **order-service** `.env` file, update the `RABBITMQ_CONNECTION_STRING` environment variable with the new RabbitMQ connection string:
     ```bash
     RABBITMQ_CONNECTION_STRING=amqp://your_username:your_password@<VM_IP>:5672/
     ```
   - This will ensure that your services communicate with the RabbitMQ instance running on the Azure VM.

6. **Test the RabbitMQ Setup**:
   - Test your application by sending requests to the **order-service** to verify that they can communicate with RabbitMQ on the Azure VM.
   - You can monitor the status of RabbitMQ by logging into the RabbitMQ management UI.



## Suggested VM Sizes

For this lab, each service runs on its **own dedicated Azure VM**.  
The following sizes are recommended to balance cost and performance:

| Component | Suggested Size | vCPU | RAM | Rationale |
|-----------|----------------|------|-----|-----------|
| **store-front (Vue.js)** | **B1ms** | 1 | 2 GB | Lightweight runtime; enough to build and serve without swapping. |
| **order-service (Node.js)** | **B1ms** | 1 | 2 GB | Handles API calls comfortably for lab workloads. |
| **product-service (Rust)** | **B2s** | 2 | 4 GB | Rust builds require more memory/CPU; this size keeps compile times reasonable. |
| **RabbitMQ** | **B2s** | 2 | 4 GB | Message broker benefits from extra RAM/CPU; Management UI stays responsive. |

- Stop/deallocate VMs once you are done to save credits.  


## Submission

For this second lab, you will demonstrate both your ability to refactor the services for 12-Factor compliance and your understanding of the concepts through reflection.

### What to Submit
1. **Demo Video (Max 5 minutes)**  
   - Record a short demo video showing your running application deployed across **four separate Azure VMs**.  
   - At minimum, your demo should include:  
     - The **store-front** running in your browser (served from its own VM).  
     - The **public IPs** of the four VMs (`store-front`, `order-service`, `product-service`, `RabbitMQ`) briefly shown in the Azure portal or terminal.  
     - Confirmation that the **order-service** and **product-service** are using **environment variables** (e.g., show `.env` files exist or logs proving configuration values are loaded).  
     - A demonstration that:  
       - The **store-front** loads products from the `product-service` VM.  
       - An order is placed through the `order-service` VM.  
       - The `order-service` successfully communicates with **RabbitMQ** hosted on its dedicated VM (e.g., show message activity or the RabbitMQ Management UI).  
   - Upload your video to **YouTube** (unlisted is fine).  
   - Copy the video link into your `README.md` file in the submission repository.  

2. **Reflection Questions**  
   Answer the following in your `README.md` file. Keep your answers **short but thoughtful** (≈1 short paragraph each).  

   1. What changes did you make to the `order-service` and `product-service` to comply with the **Configurations** and **Backing Services** factors of the 12-Factor App methodology?  
   2. Why is it important to use environment variables instead of hard-coding configurations in your application?  
   3. Why is it important to have **separate repositories** for each microservice? How does this help maintain independence and scalability of each service?  

3. **GitHub Repository (Submission Repo)**  
   - You must create **one GitHub repository** for your lab submission (this is separate from the three service repositories).  
   - This submission repository must include:  
     - A `README.md` file with:  
       - The YouTube demo video link.  
       - Your written answers to the Reflection Questions.  
       - Links to the three service repositories you created:  
         - `order-service` 
         - `product-service`  
         - `store-front`  
     - (Optional) Notes about setup challenges or lessons learned.  

### How to Submit
- Push your work to a **public GitHub repository** (the submission repository).  
- Submit the link to your submission repository as your final lab deliverable in **Brightspace**.  
