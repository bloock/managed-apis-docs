---
stoplight-id: evh9x8wolk8p4
---

# Installation

You have two primary methods to set up and run the Bloock Managed API:

1. [Docker Setup Guide](#docker-setup-guide)
2. [Standalone Setup](#standalone-setup)

Each method has its advantages and use cases.

## Docker Setup Guide

Docker offers a convenient way to package and distribute the API, along with its required dependencies, in a self-contained environment. It's an excellent choice if you want a quick and hassle-free setup, or if you prefer isolation between your application and the host system.

## Option 1: Pull and Run the Docker Image

This option is straightforward and ideal if you want to get started quickly. Follow these steps:

1. **Pull the Docker Image:**

   - Open your terminal or command prompt.

   - Run the following command to pull the Docker image from [DockerHub](https://hub.docker.com/repository/docker/bloock/managed-api/general):

     ```bash
     docker pull bloock/managed-api
     ```

     This command fetches the latest version of the Bloock Managed API image from [DockerHub](https://hub.docker.com/repository/docker/bloock/managed-api/general). We maintain a Docker repository with the latest releases of this repository.

2. **Create a `.env` File:**

   - In your project directory, create a `.env` file. You can use a text editor of your choice to create this file.

   - This file will contain the configuration for the API, including environment variables. Refer to the [Variables](#variables) section for a list of environment variables and their descriptions.

   - In the `.env` file, define the environment variables you want to configure for the API. Each environment variable should be set in the following format:

     ```txt
     VARIABLE_NAME=VALUE
     ```

   - Here's an example of what your `.env` file might look like:

     ```txt
     BLOOCK_DB_CONNECTION_STRING=file:bloock?mode=memory&cache=shared&_fk=1
     BLOOCK_API_KEY=your_api_key
     BLOOCK_WEBHOOK_SECRET_KEY=your_webhook_secret_key
     BLOOCK_CLIENT_ENDPOINT_URL=https://bloock.com/endpoint/to/send/file
     ```

     > **NOTE:** For the **BLOOCK_DB_CONNECTION_STRING** environment variable, you have the flexibility to specify your own MySQL or PostgreSQL infrastructure. Clients can provide their connection string for their database infrastructure. See the [Database](#database-support) section for available connections.

3. **Run the Docker Image with Environment Variables:**

   - Run the following command to start the Bloock Managed API container while passing the `.env` file as an environment variable source:

   ```bash
   docker run --env-file .env -p 8080:8080 bloock/managed-api
   ```

   - This command maps the `.env` file into the container, ensuring that the API reads its configuration from the file. Viper automatically read these environment variables and make them accessible to the code.

   - By default, the above command runs the Docker container in the foreground, displaying API logs and output in your terminal. You can interact with the API while it's running.

     3.1. **Running Docker in the Background (Daemon Mode)**

   - Append the `-d` flag to the docker run command as follows:

   ```bash
   docker run -d --env-file config.txt -p 8080:8080 bloock/managed-api
   ```

   The `-d` flag tells Docker to run the container as a background process. You can continue using your terminal for other tasks while the Bloock Managed API container runs silently in the background.

4. **Access the API:**

   - After running the Docker image, the Bloock Managed API will be accessible at `http://localhost:8080`.

   - You can now make API requests to interact with the service.

By following these steps, you can quickly deploy the Bloock Managed API as a Docker container with your customized configuration.

## Option 2: Use Docker Compose with Database Containers

If you need a more complex setup, such as using a specific database like **MySQL**, **Postgres** or **MemDB**, Docker Compose is your choice. Follow these steps:

1. **Choose the Docker Compose File:**

   - In our [repository](https://github.com/bloock/bloock-managed-api), you will find Docker Compose files for different database types:

     - `docker-compose-mysql.yaml` for MySQL
     - `docker-compose-postgres.yaml` for PostgreSQL
     - `docker-compose.yaml for MemDB` (SQLite)

2. **Copy the Chosen Docker Compose File:**

   - Choose the Docker Compose file that corresponds to the database type you want to use. For example, if you prefer MySQL, copy `docker-compose-mysql.yaml`.

3. **Configure Environment Variables:**

   - Open the Docker Compose file in a text editor. Inside the file, locate the environment section for the api service. Here, you can specify environment variables that configure the API.

   - Refer to the [Variables](#variables) section for a list of environment variables and their descriptions.

4. **Set Environment Variables:**

   - In the `environment` section, you can set environment variables in the following format:

     ```yaml
     VARIABLE_NAME: "VALUE"
     ```

   - Here's an example of what your `environment` section might look like:

     ```yaml
     BLOOCK_DB_CONNECTION_STRING: "file:bloock?mode=memory&cache=shared&_fk=1"
     BLOOCK_API_KEY: "your_api_key"
     BLOOCK_WEBHOOK_SECRET_KEY: "your_webhook_secret_key"
     BLOOCK_CLIENT_ENDPOINT_URL: "https://bloock.com/endpoint/to/send/file"
     ```

5. **Run Docker Compose:**

   - Open your terminal, navigate to the directory where you saved the Docker Compose file, and run the following command:

   ```bash
    docker-compose -f docker-compose-mysql.yaml up
   ```

   Replace `docker-compose-mysql.yaml` with the name of the Docker Compose file you selected.

   5.1. **Running Docker in the Background (Daemon Mode)**

   - Append the `-d` flag to the docker run command as follows:

   ```bash
   docker-compose -f docker-compose-mysql.yaml up -d
   ```

   The `-d` flag tells Docker to run the container as a background process. You can continue using your terminal for other tasks while the Bloock Managed API container runs silently in the background.

6. **Access the API:**

   - After running the Docker Compose command, the Bloock Managed API will be accessible at http://localhost:8080. You can make API requests to interact with the service.

By following these steps, you can quickly set up the Bloock Managed API with your chosen database type using the provided Docker Compose files.

## Standalone Setup

Running the API as a standalone application provides more control and flexibility, allowing you to customize and integrate it into your specific environment. Choose this option if you have specific requirements or if you want to modify the API's source code.

## Option 3: Clone the GitHub Repository

You can also run this service as a common Golang binary if you need it.

### Standalone Requirements

- Makefile toolchain
- Unix-based operating system (e.g. Debian, Arch, Mac OS X)
- [Go](https://go.dev/) 1.20

To deploy the API as a standalone application, follow these steps:

1. **Clone the Repository or Download the Latest Release:**

   1.1. **Clone the Repository:**

   - Open your terminal and navigate to the directory where you want to clone the [repository](<(https://github.com/bloock/bloock-identity-managed-api)>).

   - Run the following command to clone the [repository](<(https://github.com/bloock/bloock-identity-managed-api)>):

   ```bash
    git clone https://github.com/bloock/managed-api.git
   ```

   Instead of cloning the repository, it's recommended to download the latest release to ensure you have the most stable and up-to-date version of the Bloock Managed API.

   1.2 **Download the Latest Release:**

   - Visit the [repository's releases page](https://github.com/bloock/bloock-managed-api/releases) on GitHub.

   - Look for the latest release version and select it.

   - Under the Assets section, you will find downloadable files. Choose the appropriate file for your operating system (e.g., Windows, macOS, Linux).

   - Download the selected release file to your local machine.

2. **Navigate to the Repository:**

   - Change your current directory to the cloned repository or downloaded the release file:

   ```bash
    cd managed-api
   ```

3. **Set Up Configuration:**

   - Inside the repository, you'll find a `config.yaml` file.

   - Open `config.yaml` in a text editor and configure the environment variables as needed, following the format described in the [Variables](#variables) section. For example:

   ```yaml
   BLOOCK_DB_CONNECTION_STRING: "file:bloock?mode=memory&cache=shared&_fk=1"
   BLOOCK_API_KEY: "your_api_key"
   BLOOCK_WEBHOOK_SECRET_KEY: "your_webhook_secret_key"
   BLOOCK_CLIENT_ENDPOINT_URL: "https://bloock.com/endpoint/to/send/file"
   ```

4. **Run the Application:**

   - To run the application, execute the following command:

   ```bash
    go run cmd/main.go
   ```

   This command will start the Bloock Managed API as a standalone application, and it will use the configuration provided in the config.yaml file.

5. **Access the API:**

   - After running the application, the Bloock Managed API will be accessible at http://localhost:8080. You can make API requests to interact with the service.
