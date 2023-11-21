

****Jenkins CI/CD pipeline with GitHub webhook integration for Deploying Docker application on EC2 instances using the declarative pipeline.****

**Steps:**

**1. **First of all, go to the AWS portal and create a new instance. As

Name: **Master-Server**
AMI: **ubuntu**.
Instance type: **t2.micro (free tier)**.
Key pair login: Create > projects.pem.
(**Download the .pem file.**)

Click on Launch Instance.
![image](https://user-images.githubusercontent.com/51720295/209477578-54c46a7b-805d-49ca-8806-6e0a41ce627d.png)

**2. **Now, connect to the EC2 instance that you have created. Copy the SSH from the server:
![image](https://user-images.githubusercontent.com/51720295/209477586-e30d974c-2ff1-42bc-b3a4-c5b45b967dba.png)

**3. **Go to the download folder, where the .pem file is placed and open the terminal in the same location, and paste the SSH.

**4. **No we will Install Docker. By running this Command:
“**Sudo apt-install docker.io**”

**5. **Write a Docker file for running your application, and Push your Docker file in Project Repo on Github.
![image](https://user-images.githubusercontent.com/51720295/209477598-960511c7-874e-4bc4-8f5d-3dcf865f5f5b.png)

**6. **Build an image of the Docker file by running this Command:
It will give some warning but it will create an image of your application.
“**sudo docker build . -t todo-app**”
![image](https://user-images.githubusercontent.com/51720295/209477607-ad7ce525-bfa5-4936-bd5a-ad649db1dd5e.png)

**7. **Now we will install Jenkins on the machine, by following this link
**https://www.jenkins.io/doc/book/installing/linux/**

**8. **Now, we will allow ports **8080** and **8001** or **8000** (on which you want to run your application) for this machine from a security group. We can find the security group in the EC2 description. Now, here we need to allow “**Inbound Rule**” as below:
![image](https://user-images.githubusercontent.com/51720295/209477612-12336912-ff05-493e-8c50-cc51d171bac9.png)

**9. **Now check if it got installed by running “**jenkins — version**” and “**docker –version**”.
![image](https://user-images.githubusercontent.com/51720295/209477622-46d5d9a4-966e-41c5-bb6f-51a3e234cb1a.png)

**10. **Now, Copy the Public Ip of the machine and paste it to the browser to access the Jenkins portal with port no 8080. (As your Jenkins will Run on Port 8080).
”**Public Ip of EC2:8080**”
![image](https://user-images.githubusercontent.com/51720295/209477625-82980c94-098e-4c81-9789-e4c5843aef5d.png)

**11. **We need an Administrator Password to unlock this. For that, go to the provided highlighted path in the upper screenshot.
“**cat /var/lib/Jenkins/secrets/initialAdminPassword**”
![image](https://user-images.githubusercontent.com/51720295/209477628-6f91b171-e75a-4ddc-a513-5d7687d7ca5c.png)

Paste this password in the “**Administrator Password**” Column and Continue.

**12. **Now Click on, “**Install Suggested Plugins**”
![image](https://user-images.githubusercontent.com/51720295/209477629-6ce78914-ba7f-4418-a47b-e855af52a6aa.png)

**13. **This will now install all the basic plugins that you will need most.
![image](https://user-images.githubusercontent.com/51720295/209477634-8c33aa3a-e654-47ad-8203-8900a05b3370.png)

**14. **Now, Jenkins will ask us to **create the First Admin User**.
![image](https://user-images.githubusercontent.com/51720295/209477636-24ed8a4a-a5be-4738-aba3-d66289ede888.png)

**15. **Add the fields according to your username and Email.
![image](https://user-images.githubusercontent.com/51720295/209477640-990751dd-9797-4571-98bf-af854c7d1fd6.png)

**16. **The Jenkins homepage will look like this,
First of all, we **create a job**.
![image](https://user-images.githubusercontent.com/51720295/209477651-755d8698-ece4-437c-948a-b659591dcda5.png)

**17. **Now, we will create a **CI/CD pipeline**, which will **fetch the code from GitHub**.

**18. **From Jenkins Dashboard, Click on “**New Item**”.
![image](https://user-images.githubusercontent.com/51720295/209477656-46cbefa2-1f6d-472b-9a6d-63fb51460494.png)

**19. **Now, Add the name as.
• Name: **ToDo-App-Dev**
• Project: **Freestyle project**
• Click “**Ok**”.
![image](https://user-images.githubusercontent.com/51720295/209477662-f7608d55-291f-4a8d-a578-b006c7150713.png)


**20. **Here, we need to fill up the **description**.
![image](https://user-images.githubusercontent.com/51720295/209477669-7d2cb094-7bab-4ac0-8420-721df4c778c1.png)

**21. **In Source **Code Management, select Git and Add Repository URL and Credentials**. (If there is not any added credential, we need to add it). I have added them from configure so it will fetch it from there. You can also **add the branch**.
![image](https://user-images.githubusercontent.com/51720295/209477675-06dddbdb-12be-4fa2-a26f-982feb1397fe.png)

**22. ****Build Step**, select **Execute Shell** and write the following command to build a docker image and from the Docker image, we will create a container.
“**docker build -t todo-app-dev **”
“**docker run -d -p 8000:8000 todo-app-dev **“
![image](https://user-images.githubusercontent.com/51720295/209477679-e1d7251d-898f-4427-8223-f77706685550.png)

**23. **Now, Click on **build Now**. And the build will be started, in the build history.
![image](https://user-images.githubusercontent.com/51720295/209477685-3963a0f0-d4e6-4fe4-ae21-7990b4743997.png)

**24. **We can check the **Output Console** for any error, **SUCCESS**.
![image](https://user-images.githubusercontent.com/51720295/209477691-8876a92f-66ac-4551-a90f-32a70ce96dc2.png)

**25. **After getting success, In the browser, search for
“**public_ip_of_ec2:8000**”
![image](https://user-images.githubusercontent.com/51720295/209477695-e95314d4-9a15-4436-8afb-9102969f0f9d.png)

Now, our goal is,
·Whenever the developer commits their code in GitHub, after every commit, it should reflect in the live web app.
·For that, we will use “**GitScm polling**”.
·Every time, a developer made a commit, a trigger will run automatically, which will rebuild the image and run a container on your behalf as a part of automation that will run the pipeline automatically.

**26. **Now, configure the project again, and add
• **Build Trigger: GitHub hook trigger for GitScm polling**.
• Description: **GitHub webhook integration**.

**27. **We need to install the “**Git Integration**” plugin from Manage Jenkins, by following the
path,
(**Manage Jenkins > Manage Plugins > Git Integration**).
![image](https://user-images.githubusercontent.com/51720295/209477703-8b8c5a34-c4cf-4cae-a700-f41c3f47ecc9.png)

**28. **Now, we need to go to GitHub and create a **Webhook**.
**GitHub > Your Project Repository> Settings > Webhooks
**

**29. **Add the following details,
• Payload URL: **http://<public_ip_of_ec2>:8080/github-webhook/**
• Content Type: **application/json**
• Which event would you like to trigger this webhook?
• Just the **push event**.
• Active: **True**
• Click on “**Add Webhook**”.
![image](https://user-images.githubusercontent.com/51720295/209477709-86992268-3d20-46fb-a15e-937193cb4286.png)

**30. **Do some changes in the code and push it to GitHub, this will **automatically run a pipeline and the new code will be Live**.
After Webhook Deploying.
![image](https://user-images.githubusercontent.com/51720295/209477712-2a45502a-4f10-47fe-91ca-d64d09cb2b03.png)
