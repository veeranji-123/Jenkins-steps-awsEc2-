## **Lab Name:** Jenkins Setup on AWS EC2

## **Launch Instance Page (Step 1)**

### **What Done:**

- Opened **AWS EC2 → Launch Instance**.
- Entered instance name → `jenkins-lab`.
- Selected **Ubuntu 24.04 LTS (Free Tier)** AMI.
- Chose instance type: **t3.micro** (1 vCPU, 1GB RAM).

### **Schema:**

```
+--------------------------------------+
| AWS Management Console               |
|--------------------------------------|
|  EC2 → Launch Instance               |
|   ├── Name: jenkins-lab              |
|   ├── AMI: Ubuntu 24.04 LTS          |
|   ├── Type: t3.micro (Free tier)     |
|   └── Key Pair: jenkins.pem          |
+--------------------------------------+
```

**Purpose:**
 Preparing a cloud-based virtual server environment for Jenkins setup.

![](<Screenshot 2025-10-27 172142.png>)

---

## **Key Pair & Storage Configuration (Step 2)**

### **What Done:**

- Selected existing **key pair:** `jenkins.pem`.
- Configured storage volume → **8 GiB** default EBS volume.

### **Schema:**

```
+-------------------------------+
| EC2 Instance Configuration    |
|-------------------------------|
| Key Pair: jenkins.pem         |
| Storage: 8 GiB (gp3)          |
| Volume Type: Root Volume      |
+-------------------------------+
```

**Purpose:**
 The `.pem` key allows secure SSH access; storage defines root disk capacity.

![](<Screenshot 2025-10-27 172241.png>)

---

## **Security Group Setup (Step 3)**

### **What Done:**

Added inbound rules:

- **SSH (Port 22)** – for admin access from anywhere.
- **Custom TCP (Port 8080)** – for Jenkins web UI access.

### **Schema:**

```
+------------------------------------------------+
| Security Group: jenkins-lab-sg                 |
|------------------------------------------------|
| Inbound Rules:                                 |
|  1. SSH (TCP/22) → Source: 0.0.0.0/0          |
|  2. Custom TCP (8080) → Source: 0.0.0.0/0     |
+------------------------------------------------+
```

**Purpose:**
 To allow terminal login and browser access to Jenkins after installation.

![](<Screenshot 2025-10-27 172320.png>)

---

## **nstance Launch Confirmation (Step 4)**

### **What Done:**

- Clicked **Launch Instance**.
- AWS displayed success message:
   `Successfully initiated launch of instance (i-07a6786c23edcf227)`

### **Schema:**

```
+------------------------------+
| AWS Cloud                   |
|------------------------------|
| Instance: jenkins-lab        |
| ID: i-07a6786c23edcf227      |
| State: Running               |
| Region: us-east-1            |
+------------------------------+
```

**Purpose:**
 Confirms that your virtual machine is now created and active in AWS.

![](<Screenshot 2025-10-27 172356.png>)

----

##  **SSH Connection Details (Step 5)**

### **What Done:**

- Copied SSH command from AWS:

  ```
  ssh -i "jenkins.pem" ubuntu@ec2-54-167-77-94.compute-1.amazonaws.com
  ```

- Opened PowerShell → navigated to Downloads → connected successfully.

### **Schema:**

```
+-------------------------------+       +-----------------------------+
| Local PC (Windows PowerShell) | <---> | EC2 Instance (Ubuntu 24.04) |
|-------------------------------|       |-----------------------------|
| Command: ssh -i jenkins.pem   |       | Hostname: ip-xx-xx-xx-xx    |
| User: ubuntu                  |       | Port: 22                    |
+-------------------------------+       +-----------------------------+
```

**Purpose:**
 Secure connection to the cloud instance using the private key.

![](<Screenshot 2025-10-27 172415.png>)

----



![](<Screenshot 2025-10-27 172440.png>)

----

## **Inside EC2 (Step 6)**

### **What Done:**

- Ran commands to check system and update packages.

- Changed hostname:

  ```
  sudo hostnamectl set-hostname jenkins-lab
  ```

- Rebooted system:

  ```
  sudo init 6
  ```

### **Schema:**

```
+----------------------------------+
| Ubuntu Terminal (EC2 Instance)   |
|----------------------------------|
| Command: hostnamectl set-hostname|
| New Hostname: jenkins-lab        |
| Command: sudo init 6 (Reboot)    |
+----------------------------------+
```

**Purpose:**
 Gives a unique server identity and ensures environment reset after change.

![](<Screenshot 2025-10-27 172623.png>)

---

## **Java Installation (Step 7)**

### **What Done:**

- Reconnected to EC2 instance after reboot.

- Installed Java:

  ```
  sudo apt install openjdk-21-jre-headless -y
  ```

- Verified Java version:

  ```
  java -version
  ```

### **Schema:**

```
+--------------------------------------------+
| Jenkins-Lab (Ubuntu)                       |
|--------------------------------------------|
| Java Installation: openjdk-21-jre-headless |
| Command: java -version                     |
| Output: openjdk 21 ...                     |
+--------------------------------------------+
```

**Purpose:**
 Installs the Java runtime — required dependency for Jenkins.

![](<Screenshot 2025-10-27 172732.png>)

### **– Adding Jenkins Repository & Installing Jenkins**

**Filename:** `Screenshot 2025-10-27 172831.png`

**Action:**

- Jenkins official repository key and list file are added.
- System updates the package list and installs Jenkins.

**Commands Used:**

```
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins
```

**Schema:**

```
[Internet Repository]
        |
        v
[Jenkins Official Repo] ---> [Ubuntu Server]
                                 |
                                 v
                          Jenkins Installed
```



![](<Screenshot 2025-10-27 172831.png>)

----

### **Jenkins Service Enable & Start**

**Filename:** `Screenshot 2025-10-27 173528.png`

**Action:**

- Jenkins service enabled and started.
- Verified using `systemctl status jenkins`.

**Commands Used:**

```
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

**Schema:**

```
[Ubuntu Server]
       |
       +--> [Jenkins Service Installed]
                |
                +--> Active (running)
                +--> Port 8080 Open
```

![](<Screenshot 2025-10-27 173528.png>)

----

### **Access Jenkins via Browser**

**Filename:** `Screenshot 2025-10-27 173603.png`

**Action:**

- Jenkins accessed using public IP and port 8080
   Example: `http://54.167.77.94:8080`

**Schema:**

```
[Web Browser] ---> (HTTP Request :8080) ---> [Jenkins Server]
                                           |
                                           +--> Jenkins Web UI
```

![](<Screenshot 2025-10-27 173603.png>)

----

### **Unlock Jenkins Page**

**Filename:** `Screenshot 2025-10-27 173717.png`

**Action:**

- Jenkins prompts for admin unlock key.
- Location: `/var/lib/jenkins/secrets/initialAdminPassword`

**Schema:**

```
[Jenkins Web UI] ---> Requests --> [initialAdminPassword]
                                         |
                                         v
                               [/var/lib/jenkins/secrets/]
```

![](<Screenshot 2025-10-27 173717.png>)

----

### **Retrieve Jenkins Admin Password**

**Filename:** `Screenshot 2025-10-27 173755.png`

**Action:**

- Password retrieved from the server.

**Command Used:**

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

**Schema:**

```
[Ubuntu Server]
       |
       +--> /var/lib/jenkins/secrets/
                      |
                      +--> initialAdminPassword (Used to unlock Jenkins)
```

![](<Screenshot 2025-10-27 173755.png>)

---

###  **Customize Jenkins (Plugin Installation)**

**Filename:** `Screenshot 2025-10-27 173819.png`

**Action:**

- Two options shown:
  - Install Suggested Plugins ✅
  - Select Plugins to Install

**Schema:**

```
[Jenkins Setup Wizard]
       |
       +--> Option 1: Install Suggested Plugins
       |
       +--> Option 2: Select Plugins Manually
```

![](<Screenshot 2025-10-27 173819.png>)

----

### **Plugin Installation Progress**

**Filename:** `Screenshot 2025-10-27 173832.png`

**Action:**

- Jenkins automatically installs:
  - Git
  - Pipeline
  - Gradle
  - Folders
  - Credentials, etc.

**Schema:**

```
[Jenkins Plugin Manager]
       |
       +--> Downloads Plugins
       |
       +--> Installs Core Integrations
                 ├── Git
                 ├── Pipeline
                 ├── Credentials
                 ├── Gradle
                 └── Email Extension
```

![](<Screenshot 2025-10-27 173832.png>)

-----

### **Jenkins Ready Dashboard**

**Filename:** `Screenshot 2025-10-27 173959.png`

**Action:**

- Jenkins setup completed.
- Option: “Start using Jenkins” → Opens dashboard.

**Schema:**

```
[Plugin Installation Complete]
        |
        v
[Jenkins Dashboard Ready]
        |
        +--> Create Jobs / Pipelines
        +--> Manage Plugins
        +--> Configure Global Settings
```

![](<Screenshot 2025-10-27 173959.png>)

----

### **Jenkins Dashboard (Welcome Page)**

**Description:**
 This is the Jenkins home screen after installation. It allows users to create new jobs, set up agents, or configure cloud builds.

**Schema:**

```
[User Browser] 
     |
     v
[Jenkins Dashboard]
     ├── Create a Job → Starts new project setup
     ├── Set up an Agent → Configure distributed build node
     └── Configure a Cloud → Add cloud-based build environments
```

![](<Screenshot 2025-10-27 174016.png>)

-----

### **Manage Jenkins Page**

**Description:**
 This section provides Jenkins administrative options such as configuring global settings, managing tools, nodes, plugins, security, and credentials.

**Schema:**

```
[Jenkins Dashboard]
      |
      v
[Manage Jenkins]
      ├── System → Configure global settings
      ├── Tools → Manage build tools (Java, Maven, etc.)
      ├── Plugins → Add or remove Jenkins plugins
      ├── Nodes → Manage build agents
      ├── Security → Control user access
      └── Credentials → Manage login keys & secrets
```

![](<Screenshot 2025-10-27 174105.png>)

---

### **enkins User Management (Single User)**

**Description:**
 Shows the Jenkins user database with one user (`admin`). This user has full administrative access.

**Schema:**

```
[Jenkins Server]
     |
     └── User Database
           └── admin (Administrator Role)
```

![](<Screenshot 2025-10-27 174234.png>)

---

### **Jenkins User Management (Multiple Users)**

**Description:**
 Now two users are added:

- **admin** → default administrator
- **veera** → new Jenkins user

**Schema:**

```
[Jenkins Server]
     |
     └── User Database
          ├── admin (Administrator)
          └── veera (Custom user)
```

![](<Screenshot 2025-10-27 174405.png>)

----

### **Create New Jenkins Job**

**Description:**
 A new item called **“my-first-job”** is being created.
 The user selects **Freestyle Project**, a basic type for Jenkins builds.

**Schema:**

```
[Dashboard] 
     |
     └── New Item
           └── Freestyle Project → my-first-job
```

![](<Screenshot 2025-10-27 174605.png>)

----

###  **Job Configuration Page (Empty Build)**

**Description:**
 Shows the `my-first-job` configuration panel with build options like “Build Now”, “Configure”, “Delete Project”.

**Schema:**

```
[Jenkins Dashboard]
     └── Job: my-first-job
           ├── Status
           ├── Build Now
           ├── Configure
           └── Credentials
```

![](<Screenshot 2025-10-27 174734.png>)

----

###  **Jenkins Job Dashboard**

**Description:**
 Displays the main Jenkins dashboard with all jobs listed.
 The job **my-first-job** appears under the “All” tab, ready for execution.

**Schema:**

```
[Jenkins Dashboard]
     └── Job List
          └── my-first-job → Status: Ready (No builds yet)
```

![](<Screenshot 2025-10-27 174808.png>)
