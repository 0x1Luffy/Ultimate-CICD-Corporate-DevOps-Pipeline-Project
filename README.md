# BoardgameListingWebApp

## Deployment Video

<video width="700" controls>
  <source src="video/deployment-overview.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

## Description

**Board Game Database Full-Stack Web Application.**
This web application displays lists of board games and their reviews. While anyone can view the board game lists and reviews, they are required to log in to add/edit the board games and their reviews. The 'users' have the authority to add board games to the list and add reviews, and the 'managers' have the authority to edit/delete the reviews on top of the authorities of users.

## Technologies

- Java
- Spring Boot
- Amazon Web Services (AWS) EC2
- Kubernetes (K8s)
- Jenkins (CI/CD)
- Docker
- Thymeleaf
- Thymeleaf Fragments
- HTML5
- CSS
- JavaScript
- Spring MVC
- JDBC
- H2 Database Engine (In-memory)
- JUnit test framework
- Spring Security
- Twitter Bootstrap
- Maven

## Features

- Full-Stack Application
- UI components created with Thymeleaf and styled with Twitter Bootstrap
- Authentication and authorization using Spring Security
  - Authentication by allowing the users to authenticate with a username and password
  - Authorization by granting different permissions based on the roles (non-members, users, and managers)
- Different roles (non-members, users, and managers) with varying levels of permissions
  - Non-members only can see the boardgame lists and reviews
  - Users can add board games and write reviews
  - Managers can edit and delete the reviews
- Deployed the application on AWS EC2 and Kubernetes Cluster
- CI/CD automation using Jenkins pipeline
- JUnit test framework for unit testing
- SonarQube integration for code quality analysis
- Dockerized application for containerized deployment
- Spring MVC best practices to segregate views, controllers, and database packages
- JDBC for database connectivity and interaction
- CRUD (Create, Read, Update, Delete) operations for managing data in the database
- Schema.sql file to customize the schema and input initial data
- Thymeleaf Fragments to reduce redundancy of repeating HTML elements (head, footer, navigation)

## CI/CD Pipeline Overview (Jenkins)

This project uses Jenkins for an automated CI/CD pipeline with the following stages:

1. **Start** - Pipeline execution begins
2. **Tool Install** - Installs required dependencies
3. **Git Checkout** - Pulls the latest code from GitHub repository
4. **Compile** - Compiles the Java application
5. **Test** - Runs unit tests using JUnit
6. **File System Scan** - Scans for vulnerabilities in the file system
7. **SonarQube Analysis** - Performs static code analysis
8. **Quality Gate** - Verifies the SonarQube quality gate
9. **Build** - Builds the application artifact
10. **Build and Tag Docker Image** - Creates a Docker image with a version tag
11. **Docker Image Scan** - Scans the Docker image for security vulnerabilities
12. **Push Docker Image** - Pushes the Docker image to a container registry
13. **Deploy to K8s** - Deploys the application to a Kubernetes cluster
14. **Verify the Deployment** - Ensures the application is running correctly on Kubernetes
15. **End** - Marks the end of the pipeline

## Kubernetes Deployment

The application is deployed on a Kubernetes cluster with the following setup:

- **Pods** running the Spring Boot application
- **Deployment** for managing the app replicas
- **Service** to expose the application
- **Ingress** (optional) for external access
- **Load Balancer** (for cloud-based clusters)
- **Persistent Volume** for database storage (if applicable)

To check the running pods and services:
```sh
kubectl get pods -n webapps
kubectl get svc -n webapps
```

To access the application:
- If deployed on **cloud Kubernetes (AWS, GCP, Azure)**, get the LoadBalancer IP:
  ```sh
  kubectl get svc -n webapps
  ```
  Then access it via:
  ```
  http://<EXTERNAL-IP>:8080
  ```
- If deployed on **local Kubernetes (Minikube, Kind, etc.)**, use:
  ```sh
  minikube service boardgame-ssvc -n webapps --url
  ```
  or port-forward:
  ```sh
  kubectl port-forward svc/boardgame-ssvc -n webapps 8080:8080
  ```
  Then access:
  ```
  http://localhost:8080
  ```


## How to Run Locally

1. Clone the repository:
   ```sh
   git clone https://github.com/your-repo-url.git
   ```
2. Open the project in your IDE of choice
3. Run the application:
   ```sh
   mvn spring-boot:run
   ```
4. To use initial user data, use the following credentials:
   - **Username:** bugs | **Password:** bunny (User role)
   - **Username:** daffy | **Password:** duck (Manager role)
5. You can also sign up as a new user and customize your role to explore the application! 😊

---

This project is continuously evolving. Contributions and suggestions are welcome! 🚀

