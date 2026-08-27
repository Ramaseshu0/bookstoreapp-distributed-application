# BookStoreApp-Distributed-Application

---

## About this project
This is an Ecommerce project currently in active development, where users can add books to a cart and complete purchases.

The application is developed using Java, Spring, and React. It utilizes Spring Cloud Microservices and the Spring Boot Framework extensively to create a robust distributed system.

---

## Frontend Checkout Flow
![CheckOutFlow](https://user-images.githubusercontent.com/14878408/103235826-06d5ca00-4969-11eb-87c8-ce618034b4f3.gif)

## Architecture
All microservices are developed using Spring Boot. These applications are registered with a Eureka discovery server.

The Frontend React App makes requests to an NGINX server which acts as a reverse proxy. The NGINX server redirects the requests to the Zuul API Gateway.

Zuul routes the requests to the appropriate microservice based on the URL route. Zuul also registers with Eureka to retrieve the IP/domain for the microservices while routing requests.

---

## Run this project in Local Machine

> Frontend App

Navigate to the "bookstore-frontend-react-app" folder and run the following commands to start the Frontend React Application:

```
yarn install
yarn start
```

> Backend Services

To start the Backend Services, follow the steps below using an IDE (IntelliJ/Eclipse) or the Command Line.

Import this project into your IDE and run all Spring Boot projects, or build all the jars by running the "mvn clean install" command in the root parent pom.

All services will be available on the ports mentioned below.

Note: Running services individually does not provide full monitoring metrics. To see metrics like JVM memory, Tomcat error counts, and other data, use the Docker deployment method.

> Using Docker (Recommended)

1. Start the Docker Engine on your machine.
2. Run "mvn clean install" at the root of the project to build all microservice jars.
3. Run "docker-compose up --build" to start all containers.

Use the "Postman Api collection" located in the Postman directory to make requests to various services.

Services will be exposed on these ports:

```
Api Gateway Service       : 8765
Eureka Discovery Service  : 8761
Consul Discovery          : 8500
Account Service           : 4001
Billing Service           : 5001
Catalog Service           : 6001
Order Service             : 7001
Payment Service           : 8001
```

---

### Service Discovery
This project supports both Eureka and Consul as discovery services.

- When running services locally, Eureka is used for service discovery.
- When running via Docker, Consul is utilized as the service discovery mechanism.

Consul is preferred in the Docker environment due to its expanded feature set. Local execution uses Eureka to minimize the overhead of managing a local Consul agent.

---

### Troubleshooting

If you encounter issues during startup or API failures, it may be due to schema updates (new columns or tables). Currently, the project focuses on core logic rather than automated DB migrations.

To resolve most database-related issues, clear or drop the "bookstore_db" database. If problems persist, please raise an issue in the repository for assistance.

---

## Deployment (Planned Architecture)
AWS is the designated cloud provider for this project.

The project is designed to be deployed across multiple Regions and Availability Zones.

- The React App, Zuul, and Eureka will serve as public-facing services located in a public subnet.
- All microservices will be containerized and deployed via AWS ECS within a private subnet.
- Private subnets will use a NAT Gateway for external internet requests.
- A Bastion host will be used for SSH access into the private subnet microservices.

Refer to the AWS Architecture diagram below for details:

![Bookstore Final](https://user-images.githubusercontent.com/14878408/65784998-000e4500-e171-11e9-96d7-b7c199e74c4c.jpg)

---

## Monitoring
The project includes two powerful monitoring setups:

1. Prometheus and Grafana.
2. TICK stack monitoring.

Prometheus operates on a pull model. To avoid hardcoding hostnames, the system uses Consul discovery to provide target hosts dynamically. This ensures that as service instances scale, they are automatically tracked by Prometheus.

The TICK stack (Telegraf, InfluxDB, Chronograf, Kapacitor) provides both push and pull capabilities. Services push metrics to InfluxDB, while Telegraf is configured to pull metrics from specific targets. Grafana or Chronograf can be used for visualization, and Kapacitor handles alerting rules.

The "docker-compose" configuration manages all monitoring containers.

Dashboards are available at the following ports:

```
Grafana   : 3030
Zipkin     : 9411
Prometheus : 9090
Telegraf   : 8125
InfluxDb   : 8086
Chronograf : 8888
Kapacitor  : 9092 
```

For the initial Grafana login, use the following credentials:
Username: admin
Password: admin

---

**Screenshots of Tracing in Zipkin**

![Zipkin](https://user-images.githubusercontent.com/14878408/65939069-6b426a80-e442-11e9-90fd-d54b60786d41.png)

---

![Zipkin](https://user-images.githubusercontent.com/14878408/65939165-bb213180-e442-11e9-90fd-d54b60786d41.png)

---

**Screenshots of Monitoring in Grafana**

![Grafana 1](https://user-images.githubusercontent.com/14878408/66936473-65ac6d80-f05b-11e9-9e7d-9652059438cd.png)

![Grafana 2](https://user-images.githubusercontent.com/14878408/66936524-79f06a80-f05b-11e9-8898-1002813aad8e.png)

---

**Screenshots of Monitoring in Chronograf (TICK)**

![Chronograf 1](https://user-images.githubusercontent.com/14878408/66934353-f8e3a400-f057-11e9-82ab-eda7a230c09d.png)

![Chronograf 2](https://user-images.githubusercontent.com/14878408/66934482-2e888d00-f058-11e9-8dea-f1f275765265.png)

---

> Account Service

To obtain an "access_token", use the following credentials:

```
clientId : "93ed453e-b7ac-4192-a6d4-c45fae0d99ac"
clientSecret : "client.devd123"
```

Current system users:

```
Admin 
userName: "admin.admin"
password: "admin.devd123"

Normal User 
userName: "devd.cores"
password: "cores.devd123"
```

To retrieve the accessToken for the Admin User:
"curl 93ed453e-b7ac-4192-a6d4-c45fae0d99ac:client.devd123@localhost:4001/oauth/token -d grant_type=password -d username=admin.admin -d password=admin.devd123"

---

## Maintainer
This project is maintained by Chinmaya Sri Rama Seshu Pasupuleti.

Chinmaya is a Data Engineer with over 4 years of experience in building enterprise data pipelines, distributed data processing, and cloud-based architectures. With a strong background in Java, Python, and SQL, he focuses on maintaining the scalability and reliability of distributed applications.

- GitHub: https://github.com/Ramaseshu0
- LinkedIn: https://www.linkedin.com/in/rama-seshu/
- Email: pramaseshu@outlook.com