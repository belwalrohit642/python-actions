# Python Application Deployment Workflow
This GitHub Actions workflow automates the deployment of a Python application to an EC2 server when changes are pushed to the main branch or when a pull request is made to the main branch. The workflow includes two jobs: "build" and "deploy."

# Workflow Structure
The workflow is triggered by pushes to the main branch and pull requests targeting the main branch. It consists of two jobs: "build" and "deploy."

# Build Job
The "build" job is responsible for setting up the Python environment, installing dependencies, and running linting and tests using flake8 and pytest.

1- Checkout Files: This step checks out the repository code for the workflow to access.
2- Set Up Python 3.10: This step sets up Python 3.10 as the environment using actions/setup-python.
3- Install Dependencies: This step upgrades pip and installs required dependencies, including flake8 and pytest. If a requirements.txt file exists, the required packages are also installed.
4- Lint with flake8: This step performs linting using flake8 to identify syntax errors and undefined names in the code. It generates a summary of the linting statistics and source code references.
5- Test with pytest: This step runs tests using pytest. It sets the PYTHONPATH to include the 'src' directory and executes the tests.

#Deploy Job
The "deploy" job deploys the Python application to an EC2 server upon successful execution of the "build" job.

1- Checkout Files: This step checks out the repository code again to ensure the latest changes are available for deployment.
2- Deploy to Server: This step uses the easingthemes/ssh-deploy action to deploy files to the target EC2 server. It requires SSH access to the server, which is provided through the EC2_SSH_KEY, HOST_DNS, and USERNAME secrets.
3- Prepare SSH Key: This step prepares the SSH private key by creating a key.pem file and setting the appropriate permissions.
4- Install Dependencies on EC2: This step connects to the EC2 server using SSH, updates the package list, and installs python3-pip. It then navigates to the target directory and installs the application's dependencies using the requirements.txt file.
5- Run the Python Application on EC2: This step connects to the EC2 server via SSH and starts the Python application by executing python3 ~/src/app.py in the target directory. The application is run in the background using the & operator.

# Secret Requirements
To use this workflow, ensure that the following secrets are configured in your GitHub repository:

1- EC2_SSH_KEY: SSH private key with access to the EC2 server.
2- HOST_DNS: EC2 instance's DNS or IP address.
3- USERNAME: Username to use when connecting to the EC2 server.
4- TARGET_DIR: Directory on the EC2 server where the application will be deployed.
