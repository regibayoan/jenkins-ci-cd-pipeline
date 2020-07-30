# jenkins-ci-cd-pipeline

### Project Description
1. Three EC2 instances (Master, Slave1, Slave2)
2. Master = where Jenkins is installed, Slave1 = Test environment, Slave2 = Production environment  
3. Install Jenkins on Master 
4. Setup a Jenkins Master-Slave cluster on AWS
5. CI/CD pipeline triggered by webhook

**Step 1:** Check the status of Jenkins Master  
<img src="images/Screenshot%202020-07-27%2022.43.31.png" width="550" height="100">   
**Step 2:** Setup admin account and login into Jenkins Dashboard  
<img src="images/Screenshot%202020-07-27%2022.47.12.png" width="550" height="100">  
**Step 3:** Go to **Manage Jenkins** -> **Configure Global Security**  
**Step 4:** Change **Agents** to **Random**. Then **Save**    
<img src="images/Screenshot%202020-07-27%2022.59.36.png" width="550" height="100">  
**Step 5:** Now go to **Manage Nodes** -> **New Node**   
**Step 6:** Add **Slave1** and set as **Permanent Agent** -> **OK**  
<img src="images/Screenshot%202020-07-27%2023.19.20.png" width="550" height="100">   
**Step 7:** Go to **Launch method** and change it to **Launch agent by connecting it to master**  
**Step 8:** Then add the current working directory path to **/home/ubuntu/jenkins** -> **Save**  
<img src="images/Screenshot%202020-07-27%2023.25.07.png" width="550" height="250">  
**Step 9:** Make another node **Slave2** and **Copy Existing Node** (Slave1) -> **Save**  
<img src="images/Screenshot%202020-07-27%2023.26.48.png" width="550" height="200">  
**Step 10:** You should see the list of nodes that we have on the Jenkins Dashboard  
<img src="images/Screenshot%202020-07-27%2023.37.09.png" width="550" height="100">  
**Step 11:** Download **FileZilla**    
**Step 12:** **In Filezilla:**     
**Step 13:** Copy the **Slave1 IP Address** as **Host**. Username: **ubuntu**. Leave the password field empty. **Port 22**    
**Step 14:** **Don't start the connection yet**   
**Step 15:** Before connecting, add your private key. Go to **Edit** -> **Settings** -> **SFTP** -> Add your key file -> **OK**  
<img src="images/Screenshot%202020-07-27%2023.47.37.png" width="800" height="100">  
**Step 16:** Click on **Quickconnect** --> **OK**  
**Step 17:** Go to Jenkins Dashboard, Click on **Slave1**. Download the **agent.jar** file by clicking on it.  (The second link down works)  
<img src="images/Screenshot%202020-07-27%2023.55.26.png" width="1000" height="300">    
**Step 18:** Now **drag and drop** the **agent.jar** file to the ubuntu folder in FileZilla  
<img src="images/Screenshot%202020-07-27%2023.58.26.png" width="1000" height="40">  
<img src="images/Screenshot%202020-07-27%2023.58.37.png" width="600" height="300">  
**Step 19:** Let's verify if the file has been transferred to Slave1. Login to **Slave1** instance    
**Step 20:** Change hostname so it's easier to know which instance it is    
<img src="images/Screenshot%202020-07-28%2000.00.46.png" width="600" height="50">    
**Step 21:** Now do **ls** command to verify if **agent.jar** is present    
<img src="images/Screenshot%202020-07-28%2000.01.46.png" width="800" height="80">    
**Step 22:** Perform **Steps 17 to 21** for **Slave2** as well **(Tip: Rename agent.jar file for Slave2 before transferring in FileZilla)**    
**Step 23:** Verify that the **agent.jar** file has been transferred to **Slave2** as well  
**Step 24:** Next, **install openjdk on both Slave1 and Slave2**      
Commands: **sudo apt update** -> **sudo apt install openjdk-8-jdk**    
**Step 25:** Next, we connect **Slave1** and **Slave2** to the AWS Jenkins Server. Go to the Jenkins Dashboard -> **Slave1** -> Copy the command line and run in slaves. It should show **"Connected"**    
**Step 26:** If there's error encountered like below, edit security group inbound rules of **Master** in AWS to allow Port mentioned in the error    
<img src="images/Screenshot%202020-07-28%2001.14.22.png" width="800" height="100">    
<img src="images/Screenshot%202020-07-28%2001.16.27.png" width="550" height="100">        
<img src="images/Screenshot%202020-07-28%2001.21.49.png" width="800" height="70">    
**Step 27:** Repeat steps for Slave2 as well    
<img src="images/Screenshot%202020-07-28%2001.24.30.png" width="800" height="250">     
**Important Note: Don't end the sessions we just created. Duplicate the sessions to perform further operations on Slave1 and Slave2**    
**Step 28:** Now that both Slave1 and Slave2 are connected, it should look like this    
<img src="images/Screenshot%202020-07-28%2001.28.22.png" width="800" height="150">    
**Step 29:** After we have successfully created the Jenkins Master-Slave cluster on AWS, we will now create a CI/CD pipeline triggered by Git Webhook    
**Step 30:** Install Docker in both Slave1 and Slave2    
**Commands: sudo apt update -> sudo apt-get remove docker docker-engine docker.io -> sudo apt install docker.io -> sudo systemctl start docker -> sudo systemctl enable docker -> sudo systemctl status docker**    
**Step 31:** Open Jenkins Dashboard. **Create a new job** (Freestyle Project) for **Slave1** --> Name the project as **Test** -> **OK**  
<img src="images/Screenshot%202020-07-28%2002.23.52.png" width="500" height="150">    
**Step 32:** Select GitHub project then paste your repository link    
**Step 33:** Click on **Restrict where this project can be run** -> Add **Slave1** there    
<img src="images/Screenshot%202020-07-28%2002.30.37.png" width="800" height="400">    
**Step 34:** Go to Source Code Management, click on Git, add your Git repository here as well -> Click on **Save**    
<img src="images/Screenshot%202020-07-28%2002.33.57.png" width="800" height="500">    
**Step 35:** Copy files from **https://github.com/hshar/devopsIQ.git** to your repository for sample files    
<img src="images/Screenshot%202020-07-28%2002.41.50.png" width="550" height="230">    
**Step 36:** Click on **Build Now**, if the buiding is successful there wil be a **blue circle** in the building history     
**Step 37:** Click on the blue circle of Build #1    
<img src="images/Screenshot%202020-07-28%2002.45.20.png" width="500" height="140">    
**Step 38:** Let's verify that build was successful by looking at Slave1 and see if the repository has been cloned into Test job  
**Commands: ls -> cd workspace -> ls -> cd Test -> ls**   
<img src="images/Screenshot%202020-07-28%2002.48.06.png" width="550" height="150">  
**Step 39:** Now we will deploy the website that we have stored in our repository  
**Step 40:** Go back to Test job in Jenkins -> Configure -> Build -> Execute Shell -> Add commands below    
**Commands:** sudo docker rm -f $(sudo docker ps -a -q)  
sudo docker build /home/ubuntu/workspace/Test -t test  
sudo docker run -it -p 82:80 -d test  
**Step 41:** Click on **Save**    
<img src="images/Screenshot%202020-07-28%2003.03.46.png" width="550" height="300">    
**Step 42:** Before building our job again we must add one arbitrary container in **Slave1**    
**Step 43:** Add a container by performing **sudo docker run -it -d ubuntu**    
<img src="images/Screenshot%202020-07-28%2003.00.49.png" width="550" height="75">    
**Step 44:** Once the container is open, open Jenkins Dashboard and **build the project**    
**Step 45:** Once build is successful, open browser and go to **Slave1 IP:82**  
**Step 46:** Now enter **Slave1 IP:82/devopsIQ**. We should see the blue background  
**Step 47:** Now we will create a new project -> Name it **Prod**    
<img src="images/Screenshot%202020-07-28%2003.10.31.png" width="1000" height="275">    
**Step 48:** Click on **GitHub project** and add the same GitHub url    
<img src="images/Screenshot%202020-07-28%2003.12.03.png" width="800" height="300">  
**Step 49:** **Click on Restrict where this project can be run** -> **Slave2** -> **Source Code Management** -> **Git**    
<img src="images/Screenshot%202020-07-28%2003.13.53.png" width="800" height="400">  
**Step 50:** Build -> Execute shell    
**Commands:** sudo docker rm -f $(sudo docker ps -a -q)  
sudo docker build /home/ubuntu/workspace/Prod -t production  
sudo docker run -it -p 82:80 -d production    
<img src="images/Screenshot%202020-07-28%2003.16.51.png" width="900" height="300">    
**Step 51:** Again, add one container to the Slave2 -> **sudo docker run -it -d ubuntu**    
**Step 52:** Now that we have the arbitrary container, we can go to Jenkins dashboard and **build the project**    
**Step 53:** Build the project **Prod**    
<img src="images/Screenshot%202020-07-28%2003.23.38.png" width="550" height="350">  
**Step 54:** Now go to browser again and visit  **Slave2 IP:82/devopsIQ**. We should see the blue background as well    
**Step 55:** Next, we will set it up where Prod job will be triggered only when Test job is compeleted   
**Step 56:** Go to **Test job** -> **Configure** -> Add **Post-Build Actions** -> **Build Other Projects** -> Project to build = **Prod** -> Select **Trigger only if build is stable**    
**Step 57:** Click on **Save**. Now we will run jobs using pipeline    
**Step 58:** Go to **Manage Jenkins** -> **Manage Plugins** -> **Available** -> Search for **Build Pipeline** -> **Install without restart**    
<img src="images/Screenshot%202020-07-28%2003.32.56.png" width="1500" height="400">    
**Step 59:** Go back to Jenkins Dashboard and click the **+** sign -> Enter view name -> **Build Pipeline View** -> **OK**       
<img src="images/Screenshot%202020-07-28%2003.33.17.png" width="150" height="75">    
<img src="images/Screenshot%202020-07-28%2003.35.22.png" width="1200" height="200">  
**Step 60:** There add the Build Pipeline View title -> **Select Initial Job** as **Test** -> **OK**     
**Step 61:** The build pipeline should look like below      
<img src="images/Screenshot%202020-07-28%2003.37.42.png" width="1000" height="200">  
**Step 62:** Click on Run then refresh the page once -> The build pipeline should have turned the Prod green as well    
<img src="images/Screenshot%202020-07-28%2003.39.07.png" width="1000" height="200">  
**Step 63:** Now we will commit on GitHub, which should trigger our Jenkins job  
**Step 64:** Go to Jenkins Dashboard -> **Test** -> **Configure** -> **Build Triggers** -> Check the **GitHub hook trigger for GITScm polling** -> **Save**      
**Step 65:** Now configure GitHub Webhook. **Settings** -> Click on **Webhooks** -> Add Webhooks -> Insert Jenkins server address as shown   **JenkinsServerIPAdress:8080/github-webhook/**  
<img src="images/Screenshot%202020-07-28%2003.56.38.png" width="1000" height="250">    
<img src="images/Screenshot%202020-07-28%2003.58.41.png" width="1000" height="200">   
**Step 66:** Go to the master terminal to trigger a build    
**git clone git-repositoryurl**    
<img src="images/Screenshot%202020-07-28%2003.58.58.png" width="1000" height="150">  
**Step 67:** Now we will try to modify the website from the master terminal. Go to the Master terminal -> devopsIQ -> index.html    
**Step 68:** Make the modification in the title and body of the html    
<img src="images/Screenshot%202020-07-28%2004.01.50.png" width="1000" height="150">  
**Step 69:** Finally, commit and push the changes to GitHub. Jenkins should pick this up and automatically change the website for both **Test server** (Slave1) and **Production server** (Slave2)    
<img src="images/Screenshot%202020-07-28%2004.08.14.png" width="1500" height="320">  
<img src="images/Screenshot%202020-07-28%2004.08.35.png" width="1500" height="200">  

 









 
