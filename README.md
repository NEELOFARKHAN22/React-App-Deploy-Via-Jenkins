# 🚀 React Application Deployment on Apache using Jenkins with GitHub 🛠️

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

### Step 1: Configure Jenkins to Use Slave Nodes

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

## Integrating GitHub with Jenkins

### Step 1: Install GitHub Plugin

1. Log in to the Jenkins master web interface.

2. Navigate to the "Manage Jenkins" > "Manage Plugins" section.

3. Click on the "Available" tab and search for "GitHub Integration Plugin".

4. Check the box next to the "GitHub Integration Plugin" and click "Install without restart" button.
   
### Step 2: Configure GitHub Credentials

1. Go to "Manage Jenkins" > "Manage Credentials" > "Jenkins" > "Global credentials".

2. Click on "Add Credentials" and select the appropriate kind of credentials (e.g., Username with password, SSH username with private key).

3. Enter your GitHub username and password or SSH private key, and provide an optional description.

4. Click "OK" to save the credentials.
   
### Step 3: Set Up Webhooks in GitHub

1. In your GitHub repository, go to "Settings" > "Webhooks" > "Add webhook".

2. Enter the Payload URL, which should be in the format: `http://JENKINS_SERVER/github-webhook/`.

3. Select the events you want Jenkins to trigger builds for (e.g., Pushes, Pull requests).
   
5. Click "Add webhook" to save the configuration.
### Step 4: Create a Jenkins Job

1. In the Jenkins dashboard, click on "New Item" to create a new job.

2. Enter a name for the job and select the appropriate job type (e.g., Freestyle project, Pipeline).

3. Under the "Source Code Management" section, choose "Git" and enter the URL of your GitHub repository.

4. Optionally, specify the branch or branches to build.

5. Under the "Build Triggers" section, select "GitHub hook trigger for GITScm polling".

6. Configure the build steps according to your project requirements.

7. Save the job configuration.
   
### Step 5: Test the Integration

1. Make a change to your GitHub repository (e.g., push a new commit, create a pull request).

2. Monitor the Jenkins dashboard to see if the Jenkins job is triggered automatically.

3. Check the build console output to verify that the build ran successfully.
## Deploying React App with Apache via Jenkins
Deploying a React app with Apache allows you to host your application on a web server, making it accessible to users over the internet. Jenkins automates the build and deployment process, ensuring that your app is always up-to-date.

### Step 1: Set Up Jenkins Job
Use the same job as used in Github integration.
### Step 2: Configure Jenkins Job Commands

1. In the Jenkins job configuration, add an "Execute shell" build step.
2. Enter the following commands:
    ```bash
    cd /var/jenkins/workspace/apache-react
    sudo apt-get update
    sudo npm install -y
    sudo npm run build
    cd 
    sudo apt-get install apache2 -y
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
    sudo a2dissite 000-default.conf
    sudo chown -R www-data:www-data /var/jenkins/workspace/apache-react/build
    sudo chmod -R 755 /var/jenkins/workspace/apache-react/build
    sudo systemctl restart apache2
   ```
This script automates the deployment of a React app with Apache using Jenkins. Here's a simplified explanation of each step:

1. **Navigate to App Directory**: Change directory to the location where the React app is stored (`/var/jenkins/workspace/apache-react`).
   
2. **Update Packages**: Update the package list to ensure the latest versions are available (`sudo apt-get update`).

3. **Install Node.js Dependencies**: Install the necessary Node.js dependencies for building the React app (`sudo npm install -y`).

4. **Build React App**: Execute the build command to generate a production-ready version of the React app (`sudo npm run build`).

5. **Install Apache2**: Install the Apache web server to host the React app (`sudo apt-get install apache2 -y`).

6. **Configure Apache Virtual Host**: Create a configuration file (`react.conf`) for Apache, specifying the server settings and document root for the React app.

7. **Enable Site**: Enable the newly created Apache site configuration (`sudo a2ensite react.conf`), making the React app accessible through the specified domain.

8. **Disable Default Site**: Disable the default Apache site configuration (`sudo a2dissite 000-default.conf`) to prevent conflicts.

9. **Set Permissions**: Set appropriate ownership and permissions for the React app directory to ensure Apache can access the files (`sudo chown -R www-data:www-data /var/jenkins/workspace/apache-react/build` and `sudo chmod -R 755 /var/jenkins/workspace/apache-react/build`).

10. **Restart Apache**: Restart the Apache web server to apply the changes and make the React app accessible (`sudo systemctl restart apache2`).

By running this script in Jenkins, you automate the deployment process, making it easier to manage and update your React app.

## Applying SSL/TLS Certificate to Domain   

- Applying an SSL/TLS certificate involves installing Certbot, obtaining the certificate, configuring your web server (e.g., Apache), and setting up automatic renewal to ensure continuous security.
  
### Step 1: Install Certbot

```
    sudo snap install --classic certbot
    sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
### Step 2: Obtain SSL Certificate
```
    sudo certbot --apache
```
### Setting Up SSL Certificate Auto-Renewal
### Step 1: Open Crontab Configuration

    ```bash
        sudo crontab -e
    ```
### Step 2: Add Renewal Command
```
 0 0 1 */2 * certbot renew --quiet --no-self-upgrade
```
This line specifies that Certbot should renew SSL certificates on the 1st day of every second month at midnight (00:00).

## High Availability & Zero Downtime for React Application

- Ensuring that your React application is always available and never experiences any downtime is super important for making sure users have a smooth experience. This guide will show you how to make your React app highly available by using a method called link and unlink.

### Steps:

1. **Build React Application**:
   - Deploy your React application to `/var/jenkins/workspace/workspace/react-deploy2`.
   - Ensure all required files are present in this directory.

2. **Copy Build Folder**:
   - After running `npm run build`, copy the build folder to a new directory:
     ```bash
     mkdir /home/cloud_user/new-server
     cp -r /var/jenkins/workspace/workspace/react-deploy2/build /home/cloud_user/new-server/
     ```

3. **Update Apache Configuration**:
   - Edit the Apache configuration file to point to the new build folder:
     ```bash
     sudo nano /etc/apache2/sites-available/react.conf
     ```
   - Inside the configuration file, update the `DocumentRoot` to `/home/cloud_user/new-server/build`.

4. **Restart Apache Service**:
   - Restart the Apache service to apply the changes:
     ```bash
     sudo systemctl restart apache2
     ```

5. **Link Build Folder to Jenkins Workspace**:
   - Create a symbolic link from the Jenkins workspace to the new build folder:
     ```bash
     ln -s /home/cloud_user/new-server/build /var/jenkins/workspace/workspace/react-deploy
     ```
6. **Execute Shell Commands in Jenkins Job**:
   - In the Jenkins job configuration, add the following commands in the "Execute Shell" section:
     ```bash
     sudo systemctl restart apache2
     #sudo ln -s /var/jenkins/workspace/workspace/react-deploy-2 /home/cloud_user/new-server/
     cd /home/cloud_user/new-server/react-deploy-2
     sudo npm run build
     cp -r build/ ~/new-server/
     sudo systemctl restart apache2
     ```
In case of downtime, ensuring high availability and zero downtime for your React application.

## Conclusion

- By following these steps, can achieve high availability and zero downtime for your React application using a link and unlink approach. This ensures a reliable and uninterrupted user experience even during server failures.

## 🌟 Thank you ! 🌟

