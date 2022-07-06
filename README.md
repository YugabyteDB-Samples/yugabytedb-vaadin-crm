# Simple CRM With Vaadin and YugabyteDB

This is a simple customer relationships management (CRM) application that uses Vaadin as a web framework and YugabyteDB (or PostgreSQL) as a database. The app was created from the following [Vaadin tutorial](https://vaadin.com/docs/latest/tutorial) and modified to support YugabyteDB.

## Starting Database

The application works with PostgreSQL and YugabyteDB. Use any of the options below to provision a database instance.

### YugabyteDB Managed

1. Create a [YugabyteDB Managed](https://docs.yugabyte.com/preview/quick-start-yugabytedb-managed/) cluster.

2. Provide connectivity settings in the `yugabytedb-vaadin-crm/src/main/resources/application.properties` file. The connection string might look as follows:
  ```yaml
  spring.datasource.url=jdbc:postgresql://us-west-2.290e0df9-a6d2-4508-bde9-3370d1669d72.aws.ybdb.io:5433/yugabyte?ssl=true&sslmode=verify-full&sslrootcert=<YOUR_ROOT_CERT_PATH>
spring.datasource.username=<YOUR_USER>
spring.datasource.password=<YOUR_PASSWORD>
  ```

### YugabyteDB Local Cluster

1. Start a 3-node cluster with Docker:

  ```shell
  rm -r ~/yb_docker_data
  mkdir ~/yb_docker_data

  docker network create yugabytedb_network
  
  docker run -d --name yugabytedb_node1 --net yugabytedb_network \
    -p 7001:7000 -p 9000:9000 -p 5433:5433 \
    -v ~/yb_docker_data/node1:/home/yugabyte/yb_data --restart unless-stopped \
    yugabytedb/yugabyte:latest \
    bin/yugabyted start --listen=yugabytedb_node1 \
    --base_dir=/home/yugabyte/yb_data --daemon=false
  
  docker run -d --name yugabytedb_node2 --net yugabytedb_network \
    -v ~/yb_docker_data/node2:/home/yugabyte/yb_data --restart unless-stopped \
    yugabytedb/yugabyte:latest \
    bin/yugabyted start --join=yugabytedb_node1 --listen=yugabytedb_node2 \
    --base_dir=/home/yugabyte/yb_data --daemon=false
      
  docker run -d --name yugabytedb_node3 --net yugabytedb_network \
    -v ~/yb_docker_data/node3:/home/yugabyte/yb_data --restart unless-stopped \
    yugabytedb/yugabyte:latest \
    bin/yugabyted start --join=yugabytedb_node1 --listen=yugabytedb_node3 \
    --base_dir=/home/yugabyte/yb_data --daemon=false
  ```

2. Provide connection parameters in the `yugabytedb-vaadin-crm/src/main/resources/application.properties` file:
  ```yaml
  spring.datasource.url=jdbc:postgresql://us-west-2.290e0df9-a6d2-4508-bde9-3370d1669d72.aws.ybdb.io:5433/yugabyte?ssl=true&sslmode=verify-full&sslrootcert=<YOUR_ROOT_CERT_PATH>
spring.datasource.username=<YOUR_USER>
spring.datasource.password=<YOUR_PASSWORD>
  ```

### PostgreSQL

## Running Application

The project is a standard Maven project. To run it from the command line,
type `mvnw` (Windows), or `./mvnw` (Mac & Linux), then open
http://localhost:8080 in your browser.

You can also import the project to your IDE of choice as you would with any
Maven project. Read more on [how to import Vaadin projects to different 
IDEs](https://vaadin.com/docs/latest/flow/guide/step-by-step/importing) (Eclipse, IntelliJ IDEA, NetBeans, and VS Code).


## Project Structure

- `MainLayout.java` in `src/main/java` contains the navigation setup (i.e., the
  side/top bar and the main menu). This setup uses
  [App Layout](https://vaadin.com/components/vaadin-app-layout).
- `views` package in `src/main/java` contains the server-side Java views of your application.
- `views` folder in `frontend/` contains the client-side JavaScript views of your application.
- `themes` folder in `frontend/` contains the custom CSS styles.
