## CI-CD
- CICD is a set of practices that create a CICD pipeline, which allows us to have our latest version of the developed software to be integrated and deployed automatically to our production environment, or to the customers directly.




## Continuous Integration (CI): 
Developers merge/commit code to master branch multiple times a day, fully automated build and test process which gives feedback within few minutes, by doing so, you avoid the integration hell that usually happens when people wait for release day to merge their changes into the release branch.


## Continuous Delivery :
It is an extension of continuous integration to make sure that you can release new changes to your customers quickly in a sustainable way. This means that on top of having automated your testing, you also have automated your release process and you can deploy your application at any point of time by clicking on a button. In continuous Delivery the deployment is completed manually.

## Continuous Deployment: 
It goes one step further than continuous delivery, with this practice, every change that passes all stages of your production pipeline is released to your customers, there is no human intervention, and only a failed test will prevent a new change to be deployed to production.

## Jenkins:
- It is a tool to implement the CICD pipeline. It automates the processes the developed software have to be gone through before production and the deployment to the pre-production/production environment.

![](Images/ci-cd.png)





- I createda new repo on github
- Then I ssh into the repo and pushed my app folder

- Then I established connection between github and jenkins using the public and private key.
- So I created the keys first and I copied and pasted the public key into the git repo
- And provided private key to the jenkins and then I tested this connection by creating a job in jenkins

- After that I set up webhook in my github repo
- So webhook listens to the local host and github, so any changes occurs, it gets triggered and tells jenkins 



- Usually, everyone works on the different branches and then commit their code from the local host
 to git hub.
- So, I created a job in Jenkins that runs the tests on any commit from the local host
  and if all the tests pass then it automatically merges the code to the main branch.

- This avoids the corrupted code to merge to the main branch.


## Steps for setting up environment:
- Create a new Repo on github
- initialise that repo on local host by ssh
- push app folder using ssh method to the git repo
  
- If you have an error related to repo does not exist, then run following commands
- eval "$(ssh-agent -s)"

- ssh-add ~/.ssh/122

- git push -u origin main

- Now refresh the git account page and the file will be there.

 OR

- Visit your GIT-with-SSH repo on your github account
  
- Now, you'll have app folder and README file in your git repo, pushed from local host using ssh
  
- Now, in order to communicate between Jenkins and Github, we need ssh keys
- To create new keys:
- Go to .ssh folder from git terminal
- Inside .ssh folder run following command
- ssh-keygen -t rsa -b 4096 -C "your_email@example.com" (change email to your git email)
- When you press enter it'll ask for the name of the key, Give name of the key ex:Jenkins122
- After that it'll ask for password, if you don't want to set password then simply press enter
- Again will ask to confirm password, press enter
- Now it'll create the keys
- run 'ls' command and you'll be able to see your keys 'Jenkins122.pub' and 'Jenkins122'

- Now, we need to copy and paste public key (Jenkins122.pub) to our git repo and paste the private key (Jenkins122) into the Jenkins Configuration.
  
  - Run 'cat Jenkins122' and it will show the content of the key, copy whole of it and go to your repo which needs to be connected to Jenkins,
  - Go to settings option of that repo and on the left click on the deploy keys and click on the add deploy key
  - Then paste the public key and give the same name to the key and click on read and write permission
  - click add the key
  

![](Images/key.png)


- Now,Log in to Jenkins account 
- Click on the New Item on the left
- Give the item name (Job name)
- Select free style project and create the job
- now go into that job, click on congigure
- In general settings select discard old builds and give Max# of builds to keep to 3
  
![](Images/step-1.png)

- select GitHub project and give your github repo url (Give HTTP not SSH)

- In source code management select Git option and into Repository URL give SSH url of your repo
- In credential, click to add and then click to jenkins
- In the new tab under the kind option select SSH username with private key
- 
  ![](Images/key3.png)

- Give name of the private key and then copy the key from terminal by running 'cat Jenkins122' in .ssh folder
- select 'enter directly' option
- Paste the key into the key box and click add
  
## Setup a webhook:
- Go to your repo, click on settings, click on webhooks on left side
- In Payload URL give your Jenkins URL
  
![](Images/webook.png)

- Click Add webhook
- Now, in your webhooks, the webhook thatyou created should be in gray round and not the red round. Which means the Jenkins URL you entered is true
- It will turn green tick after you commit anything from local host

![](Images/webhook2.png)

- Now, your local host, Gitrepo and Jenkins are in constant contact.
  





## Continuous Delivery/Deployment:
- In this iteration, we'll deploy code into AWS :
  

- Create EC2
- Create security group - allow Jenkins ip to shh in as well any other rules required
- Create a 3rd job in jenkins: get the code from main branch  and copy (SCP) to the EC2
- Run the script to install node with any other required dependencies
- The 3rd job must only be triggered if job was successful
- First iteration run npm install & npm start manually (delivery)
- 4th job launch the app - if 3rd job was successful
- pm2 kill all - create 5th job to create DB_HOST=dp_ip
- npm start



## Prerequisites:
- Need key (file.pem) on Jenkins to allow connection between Jenkins and AWS
- We need to bypass the key asking permission when we manually ssh into our instance
- For that we need to run command in ssh shell to automate that
