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

To implement the Jenkins pipeline described, you'll need several plugins. Here's a list of the key plugins required and their purposes:

1. **Git Plugin**:
   - **Purpose**: Allows Jenkins to check out code from a Git repository.
   - **Plugin Name**: `git`
   - **Installation**: `Manage Jenkins` -> `Manage Plugins` -> `Available` -> Search for "Git Plugin"

2. **Docker Pipeline Plugin**:
   - **Purpose**: Provides Docker commands for Jenkins Pipeline.
   - **Plugin Name**: `docker-workflow`
   - **Installation**: `Manage Jenkins` -> `Manage Plugins` -> `Available` -> Search for "Docker Pipeline"

3. **Pipeline Plugin**:
   - **Purpose**: Enables the use of Pipeline scripts in Jenkins.
   - **Plugin Name**: `workflow-aggregator`
   - **Installation**: `Manage Jenkins` -> `Manage Plugins` -> `Available` -> Search for "Pipeline"

4. **SSH Agent Plugin**:
   - **Purpose**: Provides SSH credentials to Jenkins jobs.
   - **Plugin Name**: `ssh-agent`
   - **Installation**: `Manage Jenkins` -> `Manage Plugins` -> `Available` -> Search for "SSH Agent"

5. **Credentials Binding Plugin**:
   - **Purpose**: Allows credentials to be bound to environment variables for use in Jenkins Pipelines.
   - **Plugin Name**: `credentials-binding`
   - **Installation**: `Manage Jenkins` -> `Manage Plugins` -> `Available` -> Search for "Credentials Binding"

6. **SonarQube Scanner Plugin**:
   - **Purpose**: Integrates SonarQube code quality analysis with Jenkins.
   - **Plugin Name**: `sonar`
   - **Installation**: `Manage Jenkins` -> `Manage Plugins` -> `Available` -> Search for "SonarQube Scanner"

7. **Email Extension Plugin**:
   - **Purpose**: Provides advanced email notifications in Jenkins Pipelines.
   - **Plugin Name**: `email-ext`
   - **Installation**: `Manage Jenkins` -> `Manage Plugins` -> `Available` -> Search for "Email Extension"

8. **Pipeline Utility Steps Plugin**:
   - **Purpose**: Provides utility steps for Jenkins Pipelines.
   - **Plugin Name**: `pipeline-utility-steps`
   - **Installation**: `Manage Jenkins` -> `Manage Plugins` -> `Available` -> Search for "Pipeline Utility Steps"

### Optional Plugins:

Depending on your specific needs, you might also find these plugins useful:

1. **Pipeline: Stage View Plugin**:
   - **Purpose**: Provides a visualization of pipeline stages.
   - **Plugin Name**: `pipeline-stage-view`
   - **Installation**: `Manage Jenkins` -> `Manage Plugins` -> `Available` -> Search for "Pipeline: Stage View"

2. **Mailer Plugin**:
   - **Purpose**: Provides email notifications.
   - **Plugin Name**: `mailer`
   - **Installation**: `Manage Jenkins` -> `Manage Plugins` -> `Available` -> Search for "Mailer"

3. **Pipeline: GitHub Plugin**:
   - **Purpose**: Integrates GitHub with Jenkins Pipelines.
   - **Plugin Name**: `pipeline-github`
   - **Installation**: `Manage Jenkins` -> `Manage Plugins` -> `Available` -> Search for "Pipeline: GitHub"

### Installation Steps:
1. Go to `Manage Jenkins` -> `Manage Plugins`.
2. Under the `Available` tab, search for each of the plugins listed above.
3. Select the plugins and click `Install without restart` or `Download now and install after restart`.
