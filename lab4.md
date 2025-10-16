## YouTube demo video link: https://youtu.be/egH7UNvQe44

### 1. What are the main differences between a Docker image and a Docker container?

   - A Docker image is like a blueprint that contains the application code, dependencies, and configuration needed to run an application, while a Docker container is the actual running instance created from that image. Think of the image as a recipe and the container as the cooked dish you can use and interact with. Images are read-only, but containers are live and can be started, stopped, and removed as needed.

### 2. Explain how Docker's layered architecture improves efficiency.

  - Docker images are built in layers, where each instruction in a Dockerfile creates a new layer on top of the previous one. This layered approach improves efficiency because common layers can be shared across different images, which saves disk space and reduces build times. It also allows Docker to reuse unchanged layers when rebuilding an image, speeding up development and deployment.

### 3. Why does each container get its own writable layer?

  - Each container gets its own writable layer on top of the read-only image layers so it can make changes, like writing logs or temporary files, without affecting the original image. This ensures the image stays clean and reusable, while each container can have its own unique data or state during runtime. When the container is removed, its writable layer is also removed, keeping things lightweight.

### 4. What are the benefits of using Docker Compose over running containers individually?

  - Docker Compose makes it easier to manage multi-container applications by letting you define all services, networks, and volumes in a single YAML file. Instead of running each container with long individual docker run commands, you can bring the whole system up or down with one command. This simplifies setup, ensures consistency, and is especially useful in development and testing environments where multiple containers need to work together.

