# Docker Jenkins Pipeline to Implement CI/CD Workflow #
---
## AGENDA ##

    • Availability of the application and its versions in the GitHub
    • Track their versions every time a code is committed to the repository
    • Create a Docker Jenkins Pipeline that will create a Docker image from the Dockerfile and host it on Docker Hub
    • It should also pull the Docker image and run it as a Docker container
    • Build the Docker Jenkins Pipeline to demonstrate the continuous integration and continuous delivery workflow


  ## REQUIRED TOOLS 
  
    • Docker: To build the application from a Dockerfile and push it to Docker Hub
    • Docker Hub: To store the Docker image
    • GitHub: To store the application code and track its revisions
    • Git: To connect and push files from the local system to GitHub
    • Linux (Ubuntu): As a base operating system to start and execute the project
    • Jenkins: To automate the deployment process during continuous integration
   
## SERVERS 

   •  LOCAL SEVER (Local Git repository)(UBUNTU 20.04)
   
   •  EC2 SERVER-1 (As A Worker Node For Jenkins To Host Container)(UBUNTU 20.04)
   
   •  EC2 SERVER-2 (As Jenkins Server)(UBUNTU 20.04)
   
## INSTALLATION AND SETUPS

### LOCAL SEVER (For Local Git repository)(UBUNTU 20.04)

#### GIT 

Updat your Repository:

           $ sudo apt update
   
Installing Git:   

           $ sudo apt install git
           
Confirm the git installation by checking for a git version: 

          $ git --version
          
Set your global user name and email. Example: 

         $ git config --global user.name "Lubos Rendek"
         $ git config --global user.email "web@linuxconfig.org"
         
Alternatively, set your configuration directly by editing the ~/.gitconfig file:
           
           [user]
           name = Lubos Rendek
           email = web@linuxconfig.org

List global git settings to confirm your git configuration: 

         $ git config --list
         

#### CREATING GITHUB ACCOUNT


  1. Open https://github.com in a web browser, and then select Sign up.
  
          
  

<img width="785" alt="github1" src="https://user-images.githubusercontent.com/104076975/175820993-6ecec34f-71db-4de0-802a-a0a556e84a7d.png">


  2. Enter your email address.
  
    
  <img width="406" alt="github2" src="https://user-images.githubusercontent.com/104076975/175821032-ca2748b1-0a06-44a7-baa0-7fca145f8d55.png">

  3. Create a password for your new GitHub account, and Enter a username, too. Next, choose whether you want to receive updates and announcements via           email, and then select Continue.
 
 
 <img width="455" alt="githu3" src="https://user-images.githubusercontent.com/104076975/175821071-cfcbebe4-2bf8-4104-b429-72620a85f03f.png">

 
  4. Verify your account by solving a puzzle. Select the Start Puzzle button to do so, and then follow the prompts.
 
  5. After you verify your account, select the Create account button.
  
  6. Next, GitHub sends a launch code to your email address. Type that launch code in the Enter code dialog, and then press Enter.
  
  <img width="417" alt="github4" src="https://user-images.githubusercontent.com/104076975/175821218-ae217700-af44-48be-9f18-785542239fcb.png">

  7. GitHub asks you some questions to help tailor your experience. Choose the answers that apply to you in the following dialogs:

   • How many team members will be working with you?
   • What specific features are you interested in using?

  8. On the Where teams collaborate and ship screen, you can choose whether you want to use the Free account or the Team account. To choose the Free             account, select the Skip personalization button.
  
     GitHub opens a personalized page in your browser.
  
  <img width="703" alt="github5" src="https://user-images.githubusercontent.com/104076975/175821356-c2d756c8-c669-4fe9-bae4-d9b369f4b161.png">
  
  #### INITIALISION LOCAL GIT REPOSITORY AND PUSHING TO GIT HUB
  
  Step 1: Create a new GitHub Repo
     Sign in to GitHub and create a new empty repo page. You can choose to either initialize a README or not. It doesn’t really matter because we’re just        going to override everything in this remote repository anyways.
     ![repo2](https://user-images.githubusercontent.com/104076975/175821907-3465b814-26bd-4cfb-b533-83e3993e6cf3.png)

     
  Step 2: Initialize Git in the project folder (folder includes application code and dockerfile)
      
  From your terminal, run the following commands after navigating to folder you would like to add:
  Make sure you are in the root directory of the project you want to push to GitHub and run:
   
                $ git init
  
  
  This step creates a hidden .git directory in your project folder which the git software recognizes and uses to store all the metadata and version history   for the project. 

###### Add the files to Git index
    
         $ git add -A
         or
         $ git add .
         
   The git add command is used to tell git which files to include in a commit, and the -A argument means “include all”.
   
 ###### Commit Added Files  
 
         $ git commit -m 'Added my project'
         
  The git commit command creates a new commit with all files that have been “added”. the -m 'Added my project' is the message that will be included           alongside the commit, used for future reference to understand the commit.  
  
 ###### Add new remote origin (in this case, GitHub)
 
         $ git remote add origin git@github.com:sammy/my-new-project.git
         
   In git, a “remote” refers to a remote version of the same repository, which is typically on a server somewhere (in this case GitHub.) “origin” is the   default name git gives to a remote server (you can have multiple remotes) so git remote add origin is instructing git to add the URL of the default remote server for this repo.    
   
###### Rename Branch 

      $ git branch -m main
   
###### Push to GitHub
 
          $ git push -u -f origin main
          
   With this, there are a few things to note. The -f flag stands for force. This will automatically overwrite everything in the remote directory. We’re only using it here to overwrite the README that GitHub automatically initialized. If you skipped that, the -f flag isn’t really necessary.       
   
  The -u flag sets the remote origin as the default. This lets you later easily just do git push and git pull without having to specifying an origin since we always want GitHub in this case.
  
  
### EC2 SERVER-1 (As A Worker Node For Jenkins To Host Container)(UBUNTU 20.04)

#### DOCKER

 To use the latest version of Docker, we will install it from the official Docker repository. So, start by adding the GPG key for the official Docker repository to your system, after that add the repository configuration to the APT source with the following commands.
 
      $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
      $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" 
      
 Now update the APT package cache to include the new Docker packages to the system using the following command.
 
     $ sudo apt update
     
 Next, install the Docker package as shown.
 
     $ sudo apt install docker-ce
     
     ![dock](https://user-images.githubusercontent.com/104076975/175823357-59cbc9dd-56ea-4d17-80d1-8d88669bc966.png)

     
   During the Docker package installation process, the package installer triggers the systemd (system and service manager) to automatically start and enable the docker service. Using the following commands to confirm that the docker service is active and is enabled to automatically start at system startup. Also, check its status:
   
       $ sudo systemctl is-active docker
       $ sudo systemctl is-enabled docker
       $ sudo systemctl status docker
       
       ![dock1](https://user-images.githubusercontent.com/104076975/175823363-3ed1cfda-5c2b-42cd-93dc-985c86688fda.png)

       
 To check the version of Docker CE installed on your system, run the following command:
 
       $ docker version
       
 

You can view available docker usage commands by running the docker command without any options or arguments:

       $ docker
       
       
   ![dock4](https://user-images.githubusercontent.com/104076975/175823382-7c574ab7-a2f3-4002-94a3-7730749b0c7f.png)

###### Manage Docker as a non-root User with sudo Command

    By default, the Docker daemon binds to a UNIX socket (instead of a TCP port) which is owned by the user root. Therefore the Docker daemon always runs as the root user and to run the docker command, you need to use sudo.
    Besides, during the Docker package installation, a group called docker is created. When the Docker daemon starts, it creates a UNIX socket accessible by members of the docker group (which grants privileges equivalent to the root user).

To run the docker command without sudo, add all non-root users who are supposed to access docker, in the docker group as follows. In this example, the command adds the currently logged on user ($USER) or username to the docker group:

       $ sudo usermod -aG docker $USER
         OR
       $ sudo usermod -aG docker username
       
To activate the changes to groups, run the following command:

       $ newgrp docker 
       $ groups
       
  
  #### Installing the Default JRE in Ubuntu
 
 To install default Open JDK 11, first update the software package index:
 
  $ sudo apt update
  
Next, check for Java installation on the system.
  
  $ java -version

If Java is not currently installed, you will get the following output.

    Command 'java' not found, but can be installed with:

    sudo apt install openjdk-11-jre-headless  # version 11.0.10+9-0ubuntu1~20.04, or
    sudo apt install default-jre              # version 2:1.11-72
    sudo apt install openjdk-8-jre-headless   # version 8u282-b08-0ubuntu1~20.04
    sudo apt install openjdk-13-jre-headless  # version 13.0.4+8-1~20.04
    sudo apt install openjdk-14-jre-headless  # version 14.0.2+12-1~20.04
    
Now run the following command to install the default OpenJDK 11, which will provide Java Runtime Environment (JRE).

  $ sudo apt install default-jre
  
Once Java installed, you can verify the installation with:

   $ java -version
   
You will get the following output:

    openjdk version "11.0.15" 2022-04-19
    OpenJDK Runtime Environment (build 11.0.15+10-Ubuntu-0ubuntu0.20.04.1)
    OpenJDK 64-Bit Server VM (build 11.0.15+10-Ubuntu-0ubuntu0.20.04.1, mixed mode, sharing)
    
 
 
 ###  EC2 SERVER-2 (As Jenkins Server)(UBUNTU 20.04)
 
 #### Installing the Default JRE in Ubuntu
 
 To install default Open JDK 11, first update the software package index:
 
  $ sudo apt update
  
Next, check for Java installation on the system.
  
  $ java -version

If Java is not currently installed, you will get the following output.

    Command 'java' not found, but can be installed with:

    sudo apt install openjdk-11-jre-headless  # version 11.0.10+9-0ubuntu1~20.04, or
    sudo apt install default-jre              # version 2:1.11-72
    sudo apt install openjdk-8-jre-headless   # version 8u282-b08-0ubuntu1~20.04
    sudo apt install openjdk-13-jre-headless  # version 13.0.4+8-1~20.04
    sudo apt install openjdk-14-jre-headless  # version 14.0.2+12-1~20.04
    
Now run the following command to install the default OpenJDK 11, which will provide Java Runtime Environment (JRE).

  $ sudo apt install default-jre
  
Once Java installed, you can verify the installation with:

   $ java -version
   
You will get the following output:

    openjdk version "11.0.15" 2022-04-19
    OpenJDK Runtime Environment (build 11.0.15+10-Ubuntu-0ubuntu0.20.04.1)
    OpenJDK 64-Bit Server VM (build 11.0.15+10-Ubuntu-0ubuntu0.20.04.1, mixed mode, sharing)
    
   #### JENKINS
 
###### Step 1 — Installing Jenkins
  
  First, add the repository key to the system:
  
     $ curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    
     $ echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d      /jenkins.list > /dev/null
     
     $ sudo apt-get update
     
     $ sudo apt-get install jenkins
     
###### Step 2 — Starting Jenkins
        
    start Jenkins by using systemctl:
      
      $ sudo systemctl start jenkins
      
   Since systemctl doesn’t display status output, we’ll use the status command to verify that Jenkins started successfully:
   
      $ sudo systemctl status jenkins
      
 ###### Step 3 — Opening the Firewall
    
    By default, Jenkins runs on port 8080. We’ll open that port using ufw:
      
       $ sudo ufw allow 8080
       
   Check ufw’s status to confirm the new rules:
   
       $ sudo ufw status
       
###### Step 4 — Setting Up Jenkins

   To set up your installation, visit Jenkins on its default port, 8080, using your server domain name or IP address: http://your_server_ip_or_domain:8080
   You should receive the Unlock Jenkins screen, which displays the location of the initial password:
   
  
![jenkins1](https://user-images.githubusercontent.com/104076975/175861562-37d35604-d67b-4c5a-be4a-fb9e420c6143.png)

In the terminal window, use the cat command to display the password:

     $ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     
 Copy the 32-character alphanumeric password from the terminal and paste it into the Administrator password field, then click Continue.
 
 The next screen presents the option of installing suggested plugins or selecting specific plugins:
 
 <img width="983" alt="jenkins2" src="https://user-images.githubusercontent.com/104076975/175861766-5feb5097-ea56-4d93-907f-b41675acd72e.png">

  We’ll click the Install suggested plugins option, which will immediately begin the installation process.
  
<img width="982" alt="j3" src="https://user-images.githubusercontent.com/104076975/175861880-366f7aed-bf30-4f89-918a-0ab292ef5ab7.png">

When the installation is complete, you’ll be prompted to set up the first administrative user. It’s possible to skip this step and continue as admin using the initial password we used above, but we’ll take a moment to create the user.

<img width="1009" alt="j4" src="https://user-images.githubusercontent.com/104076975/175861962-2fa02ffe-4aff-4f63-9de8-f536b93a6331.png">

Enter the name and password for your user,then save and continue

You’ll receive an Instance Configuration page that will ask you to confirm the preferred URL for your Jenkins instance. Confirm either the domain name for your server or your server’s IP address:

<img width="1015" alt="j5" src="https://user-images.githubusercontent.com/104076975/175862152-a4ed8b2e-7aba-4434-9777-72188a0444d9.png">

After confirming the appropriate information, click Save and Finish. You’ll receive a confirmation page confirming that “Jenkins is Ready!”:
Click Start using Jenkins to visit the main Jenkins dashboard:
