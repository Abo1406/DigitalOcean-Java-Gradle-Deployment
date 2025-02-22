# DigitalOcean-Java-Gradle-Deployment
## Overview
This project demonstrates how to set up a server on DigitalOcean and deploy a Java Gradle application on it. The deployment follows security best practices by creating a separate Linux user for running the application.

## Technologies Used
- **DigitalOcean** – Cloud hosting platform
- **Linux (CentOS)** – Operating system for the server
- **Java 17 JDK** – Application runtime
- **Gradle** – Build and dependency management tool

## Project Steps
1. **Setup and Configure a Server on DigitalOcean**
   - Create a Droplet (CentOS-based) on DigitalOcean.
   - Configure firewall and SSH access.

2. **Build and Transfer the Application**
   - Build the Java application using Gradle:
     ```bash
     gradle clean build
     ```
   - Transfer the JAR file from IntelliJ to the DigitalOcean machine using SCP:
     ```bash
     scp java-react-example.jar your-user@your-server-ip:/home/your-user/
     ```

3. **Install Java 17 JDK on the Droplet**
   ```bash
   sudo yum install -y java-17-openjdk
   ```

4. **Configure the Firewall**
   - Allow access to port 7071:
     ```bash
     sudo firewall-cmd --add-port=7071/tcp --permanent
     sudo firewall-cmd --reload
     ```

5. **Run the Application**
   ```bash
   java -jar /home/your-user/java-react-example.jar
   ```

6. **Verify the Application**
   ```bash
   curl http://16.151.246.222:7071
   ```

## Automating Deployment with a Systemd Service (Optional)
Create a systemd service file to ensure the app runs on startup:
```bash
sudo nano /etc/systemd/system/java-app.service
```
Add the following content:
```ini
[Unit]
Description=Java Application
After=network.target

[Service]
User=your-user
ExecStart=/usr/bin/java -jar /home/your-user/java-react-example.jar
Restart=always

[Install]
WantedBy=multi-user.target
```
Save and exit, then enable the service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable java-app
sudo systemctl start java-app
```

## Repository Structure
```
/
├── java-react-example.jar  # The Java application
├── README.md               # Documentation
├── .gitignore              # Git ignore file
└── deployment-scripts/     # (Optional) Deployment scripts
```

## Author
**Abdurrahman ATA**
- LinkedIn: www.linkedin.com/in/abdurrahman-a-a5a601127
---
### License
This project is open-source and available for educational purposes.

