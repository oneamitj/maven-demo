# Jenkins Deployment
## Note:
- SonarQube: Make sure SonarQube server is running and accessible from Jenkins.

- Datadog: Replace with actual monitoring tool setup as needed.

- Docker Compose: Ensure docker-compose.yml and docker-compose.prod.yml (for production) are properly defined.

- Email Notification: Adjust the email configuration as per your requirements.


## Push to Docker Hub:

- This stage logs into Docker Hub using credentials stored in Jenkins (you need to set up Docker Hub credentials in Jenkins with ID docker-hub-credentials).

- The Docker image is then pushed to Docker Hub.

- Deploy to Test/Production:

These stages now include steps to pull the Docker image from Docker Hub before deploying using Docker Compose.


## Set up Jenkins Credentials:

1. In Jenkins, go to "Manage Jenkins" > "Manage Credentials" > "Global" > "Add Credentials".
2. Choose "Username with password" and enter your Docker Hub username and password.
3. Save it with the ID docker-hub-credentials.


## Set up SSH Credentials:

1. In Jenkins, go to "Manage Jenkins" > "Manage Credentials" > "Global" > "Add Credentials".
2. Choose "SSH Username with private key" and enter the SSH key details.
3. Save it with the ID ssh-credentials.

## Test/Prod Server/VM Setup:

- Ensure that the test server has Docker and Docker Compose installed.
- Make sure the Jenkins user has SSH access to the test server.


# maven-demo

Clone this to your workspace:

    git clone https://github.com/davidmoten/maven-demo.git

##Import to Eclipse

To import this maven structured project into Eclipse:

* **File - Import - Maven - Existing Maven Projects**
* open the `maven-demo` folder and hit **Finish**

The project includes a dependency on *r-tree*, a main class `Thing` and a test class `ThingTest`.


##Build 
To build from the command line (to produce a jar for instance):

    cd maven-demo
    mvn clean install

You'll see the tests have been run and a jar has been built and put in the *target* directory.
