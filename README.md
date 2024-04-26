## 🚀 React Application Deployment on Apache using Jenkins with GitHub 🛠️

## Overview
Setting up a Jenkins master-slave architecture, deploying a React application from GitHub to an Apache server via Jenkins, and ensuring high availability with zero downtime. 

## Configure Jenkins Master Node on Ubuntu Server
### Step 1: Update and Upgrade System Packages
```
sudo apt update
sudo apt upgrade -y
```
### Step 2: Install Java
    ```
        sudo apt install openjdk-11-jdk -y
    ```
Check the installed Java version:
```
 java -version
```
You should see output similar to:
```
openjdk version "11.0.7" 2020-04-14
OpenJDK Runtime Environment (build 11.0.7+10-post-Ubuntu-3ubuntu1)
OpenJDK 64-Bit Server VM (build 11.0.7+10-post-Ubuntu-3ubuntu1, mixed mode, sharing)
```
### Step 3: Installing Jenkins

1. Import the Jenkins repository's GPG keys using the following command:
   ```bash
   curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io- 
   2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > 
   /dev/null
   ```
2. Add the Jenkins repository to your system's sources.list:
   ```
     echo deb [signed-by=/usr/share/keyrings/jenkins-        
    keyring.asc]  https://pkg.jenkins.io/debian-stable binary/ |     sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
   ```
3. Update the package index and install Jenkins:
   ```
   sudo apt update
   sudo apt install jenkins
   ```
4. Check the status of the Jenkins service to ensure it's running:
   ```
    sudo systemctl status jenkins
   ```
### Step 4: Accessing Jenkins Web Interface
To access the Jenkins web interface, follow these steps:

1. Open a web browser and navigate to the following URL:
   ```
   http://YOUR_SERVER_IP_OR_DOMAIN:8080
   ```
### Step 5: Finalizing Jenkins Setup

After accessing the Jenkins web interface and completing the initial setup, you will be prompted to:

1. **Copy the path** from the Jenkins dashboard (usually displayed in the browser's address bar).
2. **Paste the path** into your server's terminal or command prompt.
   ```
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
4. **Copy the generated password** from the Jenkins dashboard.
5. **Paste the password** into the Jenkins login page.
6. **Log in** using the **admin credentials** provided during setup.
### Step 6: Installing Jenkins Plugins

Once logged in, you'll be directed to the Jenkins web interface. Here, you can:

1. **Install Plugins**: Jenkins will prompt you to install plugins to extend its functionality. Choose recommended or specific plugins based on your project needs.
   
2. **Explore Jenkins Web UI**: Familiarize yourself with navigation menus, project dashboards, build histories, and configuration options.

3. **Configure Jenkins Settings**: Adjust security settings, set up build triggers, define job configurations, and configure email notifications.

4. **Explore Additional Options**: Discover features such as pipeline scripting, distributed builds, and advanced automation capabilities.
### Adjusting Firewall for Jenkins Installation

Before installing Jenkins on a firewall-protected remote Ubuntu server, it's essential to adjust the firewall settings to allow access to the Jenkins server.

### Step 1: Open Port 8080

You must open port 8080 on your firewall to allow incoming connections to Jenkins. If you're restricting access to specific IP addresses, follow the appropriate command below:

### Restricting Access to Specific IP Address or Range

```bash
sudo ufw allow proto tcp from 192.168.121.0/24 to any port 8080
```
Allowing Access from Anywhere

If you need to allow access from anywhere to a specific port on your firewall, you can do so using the following command:

```bash
sudo ufw allow 8080
```
## Configure Jenkins Slave Node on Ubuntu Server

- To configure a Jenkins slave node on your Ubuntu server, follow the initial steps outlined in the [master node configuration](#configure-jenkins-master-node-on-ubuntu-server) README section.

- After completing the initial steps, proceed with configuring the Jenkins slave node as required for your environment. This may include specifying the Jenkins master's address, setting up SSH keys for communication, and configuring node-specific settings in the Jenkins web interface.

- By referencing the master node configuration, you ensure consistency and efficiency in setting up your Jenkins environment.
## Setting up Jenkins Master-Slave Architecture

### Step 3: Configure Jenkins to Use Slave Nodes

####  Add each slave node as a new node configuration

To configure Jenkins to use slave nodes, follow these steps:

1. Log in to the Jenkins master web interface.

2. Navigate to the `Manage Jenkins` > `Manage Nodes and Clouds` section.

3. Click on the `New Node` or `New Agent` button to add a new node configuration.

4. Enter a descriptive name for the slave node in the `Node name` field.

5. Select the option to `Permanent Agent` or `Permanent Node` depending on your Jenkins version.

6. Specify the necessary details for the slave node:
   - **Remote root directory**: The directory where Jenkins will store files on the slave node.
   - **Labels**: Assign one or more labels to the node to identify its capabilities or characteristics.
   - **Launch method**: Choose `Launch agent via SSH` to connect to the slave node using SSH.
   - **Host**: Enter the IP address or hostname of the slave node.
   - **Credentials**: Select or add SSH credentials to authenticate with the slave node. This typically involves providing a username and private key.
   - **Java Path**: Specify the path to the Java executable on the slave node (if necessary).
   - **Usage**: Determine how Jenkins should use this node, such as for builds or as part of a cloud environment.

7. Click on the `Save` or `Add` button to save the configuration changes.

8. After saving the configuration, Jenkins will attempt to connect to the slave node using the provided SSH credentials. If successful, the slave node will appear in the list of available nodes.

9. Repeat these steps for each additional slave node you want to add to Jenkins.

By following these steps, you can configure Jenkins to use slave nodes via SSH, enabling distributed builds and improving the efficiency of your Jenkins environment.




## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Architecture](#architecture)
3. [Setup Instructions](#setup-instructions)
    - [Setting up Jenkins Master Node](#setting-up-jenkins-master-node)
    - [Configuring Jenkins Worker Node](#configuring-jenkins-worker-node)
    - [Integrating GitHub with Jenkins](#integrating-github-with-jenkins)
    - [Optimizing Jenkins Performance](#optimizing-jenkins-performance)
    - [Deploying React App with Apache via Jenkins](#deploying-react-app-with-apache-via-jenkins)
    - [Enabling SSL](#enabling-ssl)
4. [Automating Releases](#automating-releases)
5. [Ensuring High Availability](#ensuring-high-availability)
6. [Additional Notes](#additional-notes)
## Prerequisites
- Two servers: one for Jenkins master node and the other for the app server slave node.
- Code hosted on GitHub repository.
- Basic knowledge of Jenkins, Apache, and React.
## Architecture
The POC 2.1 setup involves a Jenkins master-slave architecture where the master node orchestrates tasks and the slave node executes them. GitHub serves as the repository for the React application code.
## Setup Instructions

### Setting up Jenkins Master Node
- Install necessary dependencies like JDK on the master node.
- Configure Jenkins master node to handle incoming tasks.
### Configuring Jenkins Worker Node

To set up the worker node, follow these steps:

1. **Install Jenkins Agent**: Install Jenkins agent software on the worker node. This software allows the worker node to communicate with the Jenkins master node.

2. **Configure Jenkins Master-Worker Communication**: In the Jenkins master node configuration, generate a secret token. Use this token when setting up the worker node to establish a secure connection between the master and worker nodes.

3. **Connect Worker Node to Jenkins Master**: On the worker node, run the agent software and provide the necessary information, including the Jenkins master node's URL and the secret token. This connects the worker node to the master node, enabling task distribution.

4. **Verify Connection**: Once configured, verify that the worker node appears as online in the Jenkins dashboard. This indicates that the connection between the master and worker nodes is successful.

By configuring the worker node in this manner, you ensure seamless communication between the Jenkins master and worker nodes, enabling efficient task distribution and execution.
### Integrating GitHub with Jenkins

Integrating GitHub with Jenkins is crucial for automating the deployment process in your POC. By connecting the worker node to your GitHub repository, you enable Jenkins to pull code automatically from GitHub and deploy it to the slave node whenever changes occur. This automation streamlines the development workflow, ensuring that the latest version of your application is always deployed without manual intervention.

Benefits of integrating GitHub with Jenkins in your POC include:

1. **Automated Deployment**: Jenkins automatically detects changes in the GitHub repository and triggers the deployment process, eliminating the need for manual intervention and reducing deployment time.

2. **Version Control Integration**: By pulling code directly from GitHub, Jenkins ensures that the deployed application reflects the latest changes committed to the repository, maintaining version control and consistency across environments.

3. **Efficient Collaboration**: GitHub integration enables seamless collaboration among team members by providing a centralized platform for code sharing and version management. Jenkins extends this collaboration by automating the deployment pipeline, enabling developers to focus on coding without worrying about deployment logistics.

4. **Continuous Integration and Delivery (CI/CD)**: Integrating GitHub with Jenkins facilitates CI/CD practices by automating the build, test, and deployment phases of the development lifecycle. This ensures that code changes are quickly validated and deployed, accelerating time to market and improving overall software quality.

By integrating GitHub with Jenkins in your POC, you establish a robust deployment pipeline that promotes efficiency, collaboration, and continuous delivery, ultimately enhancing the agility and reliability of your application deployment process.
### Optimizing Jenkins Performance
Enhance Jenkins' performance by removing unnecessary plugins, ensuring smooth operation even during heavy workloads.

### Deploying React App with Apache via Jenkins

To deploy the React app with Apache via Jenkins, follow these steps:

1. **Run Jenkins Job**: Utilize Jenkins to execute shell commands for deploying the React app on the Apache server. Ensure proper permissions and configurations for Apache.

```bash
# Job 1: Deploying React app with Apache via Jenkins worker node shell commands
cd /var/jenkins/workspace/apache-react
sudo apt-get update
sudo apt-get install -y nodejs npm
sudo apt-get upgrade -y
node -v
sudo npm install -y
sudo n latest 
sudo npm cache clean -f 
sudo apt-get update -y
sudo npm install pm2 -g && pm2 update -y
sudo pm2 start npm --name "react-app" -- start   
sudo npm run build 
sudo npm start 
cd /etc/apache2
sudo systemctl start apache2
cd /etc/apache2/sites-available
echo neelofar | sudo -S touch react.conf
cd /etc/apache2/sites-available
sudo sh -c 'cat <<-EOF > /etc/apache2/sites-available/react.conf
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/jenkins/workspace/apache-react/build
    ServerName freelyy.zapto.org
    ServerAlias freelyy.zapto.org
    <Directory /var/jenkins/workspace/apache-react/build>
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOF'
sudo a2ensite react.conf
sudo  a2dissite 000-default.conf
sudo chown -R www-data:www-data /var/jenkins/workspace/apache-react/build
sudo chmod -R 755 /var/jenkins/workspace/apache-react/build
sudo systemctl restart apache2
```
### Enabling SSL

Secure communication is vital for protecting sensitive data and ensuring user privacy. To enable SSL for your Apache server, follow these steps:

- **Use Certbot Tool**: Certbot is a widely used tool for automatically obtaining and renewing SSL certificates. It simplifies the process of enabling SSL encryption for your Apache server.

- **Integration with Jenkins Job**: The provided script for Job 1 includes commands for setting up SSL certificates using Certbot. These commands automate the process of obtaining and configuring SSL certificates for your Apache server.

- **Ensure Data Integrity and User Privacy**: By enabling SSL encryption, you enhance the security of data transmitted between the client and server. SSL certificates encrypt the communication, protecting it from eavesdropping and ensuring the integrity of transmitted data.

- **Automatic Renewal**: Certbot also handles the automatic renewal of SSL certificates, ensuring that your server remains secure and up-to-date with the latest encryption standards without manual intervention.

By integrating Certbot into your Jenkins deployment pipeline and automating the SSL certificate setup process, you ensure that your application is served securely over HTTPS, providing users with a safe and reliable browsing experience.
### Automating Releases

Automating releases is a crucial aspect of maintaining a continuous delivery pipeline. By setting up Jenkins jobs to automatically trigger releases upon changes in the GitHub repository, you streamline the deployment process and ensure that the latest version of your application is always available to users.

To automate releases effectively, utilize the "Delete workspace before build starts" option in the Jenkins job configuration. This option ensures that each new release from GitHub starts with a clean workspace, eliminating any remnants of previous builds and ensuring a fresh environment for the deployment process.

By deleting the workspace before starting a new build, you prevent any potential conflicts or issues that may arise from leftover artifacts or configurations from previous builds. This practice helps maintain consistency and reliability in your deployment pipeline, ensuring that each release is built and deployed from a clean slate.

With automated releases triggered by changes in the GitHub repository and the use of the "Delete workspace before build starts" option, you can confidently deploy new versions of your application with minimal manual intervention, accelerating the release cycle and delivering updates to users promptly.
### Ensuring High Availability

Ensuring high availability is critical for providing users with uninterrupted access to your application. In your POC setup, you've implemented a clever solution using symbolic links (symlinks) to enhance high availability.

Here's how it works:

- **Symlinks for High Availability**: You've created a separate directory for hosting your React application and made a symbolic link to this directory from the Apache server's root directory. This approach ensures that even if the original path of the Apache server changes, the symlinked directory still points to the correct location of the React application.

- **Flexible Configuration**: By utilizing symlinks, you maintain flexibility in configuring your Apache server without impacting the accessibility of your application. If the Apache server's path changes or if you need to relocate your React application, you can simply update the symlink to reflect the new path, ensuring seamless accessibility for users.

- **Redundancy and Failover**: Symlinks enable redundancy and failover mechanisms by providing an alternative path to access your application. In the event of server failures or maintenance, you can switch to a backup server or directory by updating the symlink, minimizing downtime and ensuring continuous availability.

By leveraging symbolic links to establish high availability for your application, you enhance its reliability and resilience, providing users with consistent access to your services.
## Additional Notes
Explore the provided shell scripts for Job 2 below:

### Job 2: Checking Webhook Functionality for Automation

In Job 2, you'll verify the functionality of webhooks for automating tasks triggered by events in your GitHub repository. This ensures that your deployment pipeline responds promptly to changes in your codebase, streamlining the development process.

Here's the shell script for Job 2:

```bash
# Job 2: Checking webhook functionality for automation
sudo systemctl restart apache2
cd /home/cloud_user/new-server/react-deploy-2
npm i
sudo npm run build
cp -r build/ ~/new-server/
sudo systemctl restart apache2
```
## 🌟 Thank you ! 🌟

