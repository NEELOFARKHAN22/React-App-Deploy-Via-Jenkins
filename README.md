## 🚀 React Application Deployment on Apache using Jenkins with GitHub 🛠️

## Overview
Welcome to POC 2.1! This README.md file serves as a comprehensive guide to setting up a Jenkins master-slave architecture, deploying a React application from GitHub to an Apache server via Jenkins, and ensuring high availability with zero downtime. Learn how to automate releases upon changes in the GitHub repository and gain insights into optimizing Jenkins performance.
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

