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
   
   •  EC2 SERVER-1 (As Jenkins Server)(UBUNTU 20.04)
   
   •  EC2 SERVER-2 (As A Worker Node For Jenkins To Host Container)(UBUNTU 20.04)
   
## INSTALLATION AND SETUPS

### LOCAL SEVER (For Local Git repository)

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

     
  Step 2: Initialize Git in the project folder
      
  From your terminal, run the following commands after navigating to folder you would like to add:
  Make sure you are in the root directory of the project you want to push to GitHub and run:
   
                $ git init
  
  
  This step creates a hidden .git directory in your project folder which the git software recognizes and uses to store all the metadata and version history   for the project. 

    - Add the files to Git index
    
         $ git add -A
         or
         $ git add .
         
   The git add command is used to tell git which files to include in a commit, and the -A argument means “include all”.
   
 Commit Added Files  
         
