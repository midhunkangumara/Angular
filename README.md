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
           note: port 80 should added in inbound rule of the security group in aws or which port the Container will be exposed
           
   •  EC2 SERVER-2 (As Jenkins Server)(UBUNTU 20.04)
          note: port 8080 should added in inbound rule of the security group in aws for jenkins
   
## INSTALLATION AND SETUPS

### LOCAL SEVER (For Local Git repository)(UBUNTU 20.04)

###### It is used to create local git reppo for keeping application coad , files and Dockerfile

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
       
       
#### DOCKER HUB  ACCOUNT & REPOSITORY
  
  ###### Docker Hub Account
  
   1. Go to the Docker Hub signup page.

   2. Enter a username that is also your Docker ID.

    Your Docker ID must be between 4 and 30 characters long, and can only contain numbers and lowercase letters.

   3. Enter a unique, valid email address.

   4. Enter a password between 6 and 128 characters long.

   5. Click Sign up.

    Docker sends a verification email to the address you provided.

   6. Click the link in the email to verify your address.
  
  • Once you register and verify your Docker ID email address, you can log in to Docker Hub and Docker Support.
  
  • You can also log in using the docker login command.When you use the docker login command, your credentials are stored in your home directory in             .docker/config.json. The password is base64 encoded in this file.
  
 ###### Docker Hub Repository
 
 To create a repository, sign into Docker Hub, click on Repositories then Create Repository:
 
 ![repos-create](https://user-images.githubusercontent.com/104076975/175922290-a5314307-bf58-4eea-82e5-1d4b3d38f4a0.png)

  When creating a new repository:

   • You can choose to put it in your Docker ID namespace, or in any organization where you are an owner.

   • The repository name needs to be unique in that namespace, can be two to 255 characters, and can only contain lowercase letters, numbers, hyphens (-),       and underscores (_).
   
    Note: You cannot rename a Docker Hub repository once it has been created.
    
   • The description can be up to 100 characters and is used in the search result.
   • You can link a GitHub or Bitbucket account now, or choose to do it later in the repository settings.
   
   ![repo-create-details](https://user-images.githubusercontent.com/104076975/175923022-9b8024c8-cae8-4ff1-b9f9-bbe8d1d6bd71.png)

    After you hit the Create button, you can start using docker push to push images to this repository.
    
    Note: For pushing image you have to login from the terminal and the name of the repsitory and the name of the image should be same

  #### Installing the Default JRE in Ubuntu 
  (For connect this server as node to Jenkins server)
  
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

#### Jenkins - Installing Plugins

 Step 1: To install a plugin, go to the Jenkins Dashboard and click on Manage Jenkins.
 

  ![jip1](https://user-images.githubusercontent.com/104076975/175931526-e1944cec-b8cd-4acf-8429-327c7c0794e3.png)

Step 2: Scroll down and select Manage Plugins.

![jip2](https://user-images.githubusercontent.com/104076975/175931550-d23f242e-f492-437a-a785-91ed346349d5.png)


Step 3: Go to the Available tab and in the filter option, search for the plugins which you want to install.

Step 4: Select that plugins and click on Install without restart button. You can also choose Download now and install after restart button.

![jip3](https://user-images.githubusercontent.com/104076975/175931626-77c2be1f-38c1-44f0-9318-b72f51608a53.png)

  Once the installation has been completed successfully, click on Go back to the top page link.
  
 ###### REQUIRED PLUGINS
 
 Please install these following plugins if they are not installed (You have to inatall the jenkins suggested plugins in the begining)
 
     • Pipeline
     • Git 
     • GitHub Branch Source
     • Pipeline:GitHub Groovy Libraries
     • SSH Slave
     • Slack Notification Plugin Version 
     
###### Slack-Jenkins Integration

First, we need to configure slack on our machine.
1. Create a slack account: https://slack.com/  if you haven't any
2. configure the Jenkins integration: https://myspace.slack.com/services/new/jenkins-ci

![s1](https://user-images.githubusercontent.com/104076975/175867481-7d602fab-a864-46a3-924f-32dd7d260f60.png)

First, install ‘Jenkins-ci’ and then Add configuration and set channel and all thing like

![s2](https://user-images.githubusercontent.com/104076975/175867549-fe471030-69c4-4d83-a9e5-64af03fd8f58.png)

 
![s3](https://user-images.githubusercontent.com/104076975/175867555-fe52ff8b-573a-47db-a4d9-5ba35f136b64.png)

After that, we need to set configuration on Jenkins Slack Notifications plugin.
For Jenkins to notify slack, we need to install in Jenkins.Go to manage jenkins in dashboard then to Manage plugins and install Slack Notifications plugin.

Connect Jenkins to your Slack : We will do this in Manage Jenkins → Configure System
Scroll all the way down on the Configure System page until you see the settings for Slack.

![sl](https://user-images.githubusercontent.com/104076975/175931710-62c5c0e0-b598-4f76-b0b8-2a069ca9d02b.png)


  •  Workspace: Your team’s workspace name.
  • Credential:
     - Click Add → Jenkins and a pop-up will appear.
     - Under the Kind dropdown, select secret text.
     
     Copy and paste your Slack Token into Secret, and click Add.
     
     If you forgot to save/copy your token, go to:
     Slack app directory (https://<YOURWORKSPACE>.slack.com/apps/manage) → Jenkins CI → Configurations → Edit Configuration → Integration Settings
    
 • Default channel/member id: Enter the channel name (i.e. #channel) or user ID (i.e. UUU123UU4) you want the notification to be sent to.
 
   Once everything is filled out, click Save.
   
  #####  MANAGING CREDENTIALS IN JENKINS
   
   To add credentials in Jenkins:
   
  1. Click Manage Jenkins from the menu.
  
  2. Scroll down to the Security heading and click Manage Credentials.
  
  ![mc1](https://user-images.githubusercontent.com/104076975/175932628-91eef672-580a-4b03-a10a-1095cc973251.png)

  3. Click Jenkins under the Stores scoped to Jenkins heading.
  
![mc2](https://user-images.githubusercontent.com/104076975/175932661-5aca07f5-dec6-4afe-8c85-2b35011585f5.png)

  
  4. Click Global credentials (unrestricted) under the System heading.
      ![mc3](https://user-images.githubusercontent.com/104076975/175932680-9b5f83b9-5f47-49be-a68b-fccf12ecf114.png)


  5. If no credentials exist, you can click the How about adding some credentials? link, otherwise click Add Credentials from the left.
  6. Select the type of credentials you want to store from the Kind field’s dropdown box, complete the fields and click OK. You can add the following types      of credentials:
      •  Usernames and passwords
      •  SSH usernames and private keys
      •  Secret files
      •  Secret text
      •  Certificates
      
![mc4](https://user-images.githubusercontent.com/104076975/175932742-12f370e9-6ebe-4a7b-a9a3-48c80b8315ec.png)

![mc5](https://user-images.githubusercontent.com/104076975/175932783-0d3a1ea0-e6dc-4d9a-8227-739d23e63d51.png)



###### Credentials For Docker Hub Login

1. Click Manage Jenkins from the menu.
2. Scroll down to the Security heading and click Manage Credentials.
3. Click Jenkins under the Stores scoped to Jenkins heading.
4. Click Global credentials (unrestricted) under the System heading.
5. Click Add Credentials from the left.
6. From the Kind field’s dropdown box select 'Username with password',complete the fields and click OK.
7. 
![cdhl](https://user-images.githubusercontent.com/104076975/175934111-eef73ba1-047c-4f98-968b-cbadb86a6e71.png)


###### Credentials For GitHub

 1. To add GitHub personal access token go to settings of your github account in the left side dashboard select Developer settings

![cfg1](https://user-images.githubusercontent.com/104076975/175934082-466ae1e5-07dc-4b0a-b616-e0ab181ea27c.png)

2. Select Personal access tokens and select Generate new token.

![cfg2](https://user-images.githubusercontent.com/104076975/175934155-3b766e67-93ba-40cd-8305-6460c237b83c.png)


3.  Fill the 'note' section and select desired Expiration time and select repo in 'Select scopes' thenk click on Generate token
  
  ![cfg3](https://user-images.githubusercontent.com/104076975/175934253-7cf98fb6-cf57-4ca5-ab07-bcd372832704.png)

 Copy the Personal access tokens.
4.  In Jenkins create credential as secret text credental and seve

![cfg4](https://user-images.githubusercontent.com/104076975/175934294-5a1f1019-97c8-4f93-8483-72640a2f6866.png)

5. To integrate Github goto Manage Jenkins → Configuration System ,
    scroll down to GitHub and under the github select Add github server and fillup the fields and select github credential
    ![Screenshot from 2022-06-27 15-00-28](https://user-images.githubusercontent.com/104076975/175908055-6848ebb8-d2f1-4185-b079-483b34bfb544.png)

    

##### ADDING NODE TO JENKINS SERVER

 If you want to run the script in another server you have to add it as a node .
 Here EC2 SERVER-1 will be a Node 
 
 1. In the Dashboard jenkins click Manage jenkins → Manage nodes
 
 
 ![Screenshot from 2022-06-27 14-23-47](https://user-images.githubusercontent.com/104076975/175900271-c176d256-5261-41db-8713-4503de5e7cf2.png)

 2. From the dashboad click on New Node ,give name of node 'WEB-SERVER' , mark 'Permanent Agent' the click on create
 
![Screenshot from 2022-06-27 14-31-07](https://user-images.githubusercontent.com/104076975/175901810-32bc0a55-0832-49a8-8b57-4153d5bb30b9.png)
3. In the next page fill up the fields and save
4. After saving you can see the node in the next page click on the node 
5. Now you have to download 'jenkins-agent.jnlp' , 'agent.jar ' files
  by clicking on 'LAUNCH' icon you can download 'jenkins-agent.jnlp' file and bu clicking on 'agent.jar' you can download 'agent.jar' file 
    
![Screenshot from 2022-06-27 14-33-50](https://user-images.githubusercontent.com/104076975/175903207-7249bda7-f966-4ec7-b4f9-0f18b47dbe6b.png)

6. Copy agent.jar , jenkins-agent.jnlp files to EC2 SERVER-1 and go  to the location of these file and ru the command that shown in the jenkin node page where you download agent.jar , jenkins-agent.jnlp files it will connect to jenkins server as node


   

### PIPELINE SETUP

###### CRETING A PIPE LINE IN JENKINS
 
 Here we are creating Scripted Jenkins pipeline
 Once you are logged in to your Jenkins dashboard:
 
 1. Click on New item in Dash board
 2. Then give item name and select Pipeline option and click ok
 
 ![pipelinesetup1](https://user-images.githubusercontent.com/104076975/175934460-f9aa45e1-13c3-4512-abbf-6f66c6871d6d.png)

3. In the  next page select 'Pipeline' in the top bar
   then click on 'Definition ' select 'pipeline sckript'
   
   ![pls2](https://user-images.githubusercontent.com/104076975/175934484-1742a99c-7795-4320-81f2-52d25550e75d.png)


4. You can copy past your pipeline script or you create it in the given space 
   you can generate pipeline syntax for that click 'Pipeline Syntax' it will open a new page where you can generate pipe syntax
   
   pipeline script example :
   
   
          
         node ('WEB-SERVER') {
              try {
             notifyStarted()
         stage('cloning'){
               checkout([
                          $class: 'GitSCM',
                          branches: [[name: 'main']],
                          userRemoteConfigs: [[
                             url: 'https://github.com/midhunkangumara/Angular.git',
                             
                          ]]
                         ])
             }
         
         stage("Docker build"){
             sh 'docker version'
             sh 'docker build -t angular-web .'
             sh 'docker image list'
             sh 'docker tag angular-web kmmidhun/angular-web:1'
           }
         stage("Docker Login"){
                 withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
                     sh 'docker login -u kmmidhun -p $PASSWORD'
                 }
             }
         stage("Push Image to Docker Hub"){
                 sh 'docker push  kmmidhun/angular-web:1'
             }
         
          stage("pull image from repo"){
                        sh 'pwd'
                        sh 'hostnamectl'
                        sh 'docker version'
                        sh 'docker pull kmmidhun/angular-web:1'
                        }
          stage('running container'){
                      sh 'docker stop myweb'
                      sh 'docker run -d --rm --name myweb -p 80:80 kmmidhun/angular-web:1'
                      }
                        notifySuccessful()
           } catch (e) {
             currentBuild.result = "FAILED"
             notifyFailed()
             throw e
           }
         }    
         def notifyStarted() {
          
           slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
         
         }
         def notifySuccessful() {
           slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}) Web URL: (http://44.202.241.246:80)")
         }
         
         def notifyFailed() {
           slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
         
         }
         
   note: name of container image should be same as the DockerHub repository
   
###### [ The script will run in the WEB-SERVER node where it will clone the github repository that we pushed from Local Server and docker will create container image by using docker file then by using dockerhub credential that we created earlier will use to login to the Dockerhub and the created image will be push to hub .Then docker will pull that image and  run it in the server (you can use different node here am using single node) The through Slack integration the script will send notifications to slack channel or user]

##### GITHUB WEBHOOK

For triggering pipeline we are using GitHub WebHook.

Step 1: go to your GitHub repository and click on ‘Settings’.
   
Step 2: Click on 'Webhooks' .

Step 3: click on ‘Add webhook’.

![Screenshot from 2022-06-27 15-11-47](https://user-images.githubusercontent.com/104076975/175911972-d76f569c-1961-4674-b3f5-166e88bc58c6.png)

 Step 4: In the ‘Payload URL’ field, paste your Jenkins environment URL. At the end of this URL add /github-webhook/. In the ‘Content type’
         select:‘application/json’ and leave the ‘Secret’ field empty. And you can choose 'Which events would you like to trigger this webhook? '
         based on the event it will trigger pipeline

  
![Screenshot from 2022-06-27 15-16-04](https://user-images.githubusercontent.com/104076975/175913848-b0766d4f-4ef1-4ffd-ab03-0bf2b8cc7449.png)

Step 5: In jenkins go configuration of our pipeline under 'Build Trigger' select 'GitHub hook trigger for GITScm polling ? '
Step 6: If you are using pipeline script in Description you have to use that GitHub repository which we created the web hook
        if you are using script from that github repository the select Pipeline Script from SCM and choos that GitHub repository
        
 ##### BUILD STATUS NOTIFICATION
 
 Here am using slack integration to sent status to a slack channel or member 
 By using a script shown below the pipeline will use the Slack integration to sent status notification
 
 
  
  
    node {
              try {
             notifyStarted()
  
           <....the build script....>
           
           
                          notifySuccessful()
           } catch (e) {
             currentBuild.result = "FAILED"
             notifyFailed()
             throw e
           }
         }    
         def notifyStarted() {
          
           slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
         
         }
         def notifySuccessful() {
           slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL}) 
           Web URL: (http://44.202.241.246:80)")
         }
         
         def notifyFailed() {
           slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
         
         }
         
        
        
