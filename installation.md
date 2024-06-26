# Installing Jenkins on Ubuntu

## Step 1: Update System Packages

First, update your system packages to ensure you have the latest versions.

```sh
sudo apt update
sudo apt upgrade -y
```

![Update System Packages](Images/image2.png)

## Step 2: Install Java

Jenkins requires Java to run. Install OpenJDK 11, which is a recommended version for Jenkins.

```sh
sudo apt install fontconfig openjdk-17-jre
```

Verify the installation:

```sh
java -version
```

![Install Java](Images/image1.png)

## Step 3: Add Jenkins Repository

Add the Jenkins Debian repository to your system:

```sh
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```

Then, add the repository to your system:

```sh
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

![Add Jenkins Repository](Images/image2.png)

## Step 4: Install Jenkins

Update the package list again and install Jenkins:

```sh
sudo apt-get update
sudo apt-get install jenkins
```

![Install Jenkins](Images/image2.png)

## Step 5: Start and Enable Jenkins

Start the Jenkins service and enable it to start on boot:

```sh
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

Check the status to ensure Jenkins is running:

```sh
sudo systemctl status jenkins
```

![Start and Enable Jenkins](Images/image3.png)

## Step 6: Adjust Firewall

If you have UFW (Uncomplicated Firewall) running, allow traffic on port 8080 (the default port for Jenkins):

```sh
sudo ufw allow 8080
sudo ufw status
```

## Step 7: Setup Jenkins

Open your web browser and go to `http://your_server_ip_or_domain:8080`. You will be prompted to unlock Jenkins.

Retrieve the initial admin password:

```sh
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Enter this password in the web interface.

![Setup Jenkins](Images/image5.png)
![Setup Jenkins](Images/image6.png)

## Step 8: Install Suggested Plugins

After unlocking Jenkins, you will be prompted to install plugins. Choose "Install suggested plugins."

![Setup Jenkins](Images/image7.png)
![Setup Jenkins](Images/image8.png)

## Step 9: Create Admin User

Create an admin user with your desired credentials.

![Setup Jenkins](Images/image9.png)

## Step 10: Jenkins is Ready

After the setup is complete, you will be redirected to the Jenkins dashboard, indicating that Jenkins is ready for use.

![Setup Jenkins](Images/image10.png)

![Setup Jenkins](Images/image11.png)

![Setup Jenkins](Images/image12.png)


## Extra Plugins to Install
- Blue Ocean
- Multibranch webhook plugin
- Git Plugin

### Example of jenkins webhook
-  http://<your-instance-ip>/multibranch-webhook-trigger/invoke?token=my-token
