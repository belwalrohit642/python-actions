# Python Application Deployment Workflow
This GitHub Actions workflow automates the deployment of a Python application to an EC2 server when changes are pushed to the main branch or when a pull request is made to the main branch. The workflow includes two jobs: "build" and "deploy."<br>

# Workflow Structure
The workflow is triggered by pushes to the main branch and pull requests targeting the main branch. It consists of two jobs: "build" and "deploy."<br>
<br>

# Build Job
The "build" job is responsible for setting up the Python environment, installing dependencies, and running linting and tests using flake8 and pytest.<br>
<br>
1- Checkout Files: This step checks out the repository code for the workflow to access.<br>
2- Set Up Python 3.10: This step sets up Python 3.10 as the environment using actions/setup-python.<br>
3- Install Dependencies: This step upgrades pip and installs required dependencies, including flake8 and pytest. If a requirements.txt file exists, the required packages are also installed.<br>
4- Lint with flake8: This step performs linting using flake8 to identify syntax errors and undefined names in the code. It generates a summary of the linting statistics and source code references.<br>
5- Test with pytest: This step runs tests using pytest. It sets the PYTHONPATH to include the 'src' directory and executes the tests.<br>
<br>
# Deploy Job<br>
The "deploy" job deploys the Python application to an EC2 server upon successful execution of the "build" job.<br>
<br>
1- Checkout Files: This step checks out the repository code again to ensure the latest changes are available for deployment.<br>
2- Deploy to Server: This step uses the easingthemes/ssh-deploy action to deploy files to the target EC2 server. It requires SSH access to the server, which is provided through the EC2_SSH_KEY, HOST_DNS, and USERNAME secrets.<br>
3- Prepare SSH Key: This step prepares the SSH private key by creating a key.pem file and setting the appropriate permissions.<br>
4- Install Dependencies on EC2: This step connects to the EC2 server using SSH, updates the package list, and installs python3-pip. It then navigates to the target directory and installs the application's dependencies using the requirements.txt file.<br>
5- Run the Python Application on EC2: This step connects to the EC2 server via SSH and starts the Python application by executing python3 ~/src/app.py in the target directory. The application is run in the background using the
![Screenshot from 2023-08-09 21-50-11](https://github.com/belwalrohit642/python-actions/assets/96739082/33cfce63-b9b3-49ae-81cc-f1260b9368aa)
 & operator.<br>
<br>
# Secret Requirements<br>
To use this workflow, ensure that the following secrets are configured in your GitHub repository:<br>
<br>
1- EC2_SSH_KEY: SSH private key with access to the EC2 server.<br>
2- HOST_DNS: EC2 instance's DNS or IP address.<br>
3- USERNAME: Username to use when connecting to the EC2 server.<br>
4- TARGET_DIR: Directory on the EC2 server where the application will be deployed.<br>
<br>


# FLOW OF EXECUTION<br>

Firstly create an EC2 instance manually from the AWS management clone in which the Hello app is going to deploy using GitHub actions.<br>
<br>
![Screenshot from 2023-08-09 21-46-39](https://github.com/belwalrohit642/python-actions/assets/96739082/71da7353-e813-4c3b-a02b-a01486d9adb2)<br>

After then, add secrets to GitHub such as SSH_PRIVATE_KEY,HOST_DNS,USERNAME,TARGET_DIR.<br>
<br>
![Screenshot from 2023-08-09 21-50-11](https://github.com/belwalrohit642/python-actions/assets/96739082/33cfce63-b9b3-49ae-81cc-f1260b9368aa)
<br>

Now Go to the Actions in the GitHub to check the  build and deploy.<br>
<br>
![Screenshot from 2023-08-09 22-00-46](https://github.com/belwalrohit642/python-actions/assets/96739082/cf8e039c-b0f2-4b51-91e8-88e8fe7dcd4e)
<br>
As you see the pipeline is executed successfully,Now check in the AWS EC2 instance that the Hello app is running.
<br>
![Screenshot from 2023-08-09 22-03-05](https://github.com/belwalrohit642/python-actions/assets/96739082/4423e52b-f908-4c36-99f2-8d0ae5a4236c)
<br>



























