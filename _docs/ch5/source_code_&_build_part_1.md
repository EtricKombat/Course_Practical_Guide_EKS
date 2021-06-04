


[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch5/workflow_definition.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch5/source_code_%26_build_part_2.md)



# Chapter - 5
# CICD

## 28.Source Code and Building - Part 1


![image](https://user-images.githubusercontent.com/33585301/119656701-9a43f600-be48-11eb-8f72-9ab325990933.png)

In this lecture we are going to start creating code commit repository  or ECR repo and use code build to create image and push it 




Let first divide it in 3 stages 

1) create the code commit repo for the micro service and then upload the code into the master branch as the initial commit . 
2) we will create the code build job that will take the code from code commit build the docker image and push it into ECR 
3) And finally we will make all this automatic with the help of code pipeline so whenever a new commit appear to code commit master branch all of these will happen automatically. 


![image](https://user-images.githubusercontent.com/33585301/119765178-5563a200-bed0-11eb-9dcc-e8c50a362992.png)





Since it a  very techinical lab only step 1 & 2 in this chapter for ease of understanding .


![image](https://user-images.githubusercontent.com/33585301/120651379-57af9880-c49c-11eb-8ec1-5a67fcee4ea8.png)


_____________________________





![image](https://user-images.githubusercontent.com/33585301/119656847-ccedee80-be48-11eb-929a-89a20eebc2d1.png)





https://github.com/EtricKombat/Course_Practical_Guide_EKS/tree/master/Infrastructure/cloudformation/cicd




In here we will find multiple template of cloudformation that we are going to be using while we proceed in this chapter . 
Each of the cloudformation updates the previous one adding more resources and more configuration . This is setup in this way so that we can analysis 
step by step while we are going to be achieveing the whole cicd workflow . 


___________

Let start with 1st template which is called code commit , in here the only resource we are creating is the code commit repository , which recieve the app name which is the microservice we are going to be putting into this one. 

We are also outputing the clone url via http of the codecommit repo we are going to be created . This is a very simple one.



https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/cicd/cicd-1-codecommit.json

Lets go to CF and create stack 

CF-> new resource-> upload template -> choose file (above listed ) -> open 

![image](https://user-images.githubusercontent.com/33585301/120652251-38653b00-c49d-11eb-9000-9097c2b50141.png)


The way we want to name the stack is adding the word cicd then the name of the micro service , 

![image](https://user-images.githubusercontent.com/33585301/120653292-2fc13480-c49e-11eb-92a3-3c49747b3344.png)

in this case let start doing everything for the resources api (once filled in next->next-> create the stack ) 



![image](https://user-images.githubusercontent.com/33585301/119657095-1d654c00-be49-11eb-80da-c435decb7f1e.png)



![image](https://user-images.githubusercontent.com/33585301/120653779-a78f5f00-c49e-11eb-9c02-e5347010e054.png)

It is already created . 


![image](https://user-images.githubusercontent.com/33585301/119657154-2a823b00-be49-11eb-98a9-6c873fe01fd3.png)

The only resource it created is was the code commit repo 


![image](https://user-images.githubusercontent.com/33585301/119657172-34a43980-be49-11eb-87d4-ac48e6509b14.png)


If we go to output we are going to find the url of the code commit repository we just created . so lets grab it .


![image](https://user-images.githubusercontent.com/33585301/120654312-33a18680-c49f-11eb-9ae4-89acfde2bf7a.png)


The next step is to upload the resources api code to this repository , since this is code commit we need to have code commit credentials .


![image](https://user-images.githubusercontent.com/33585301/120654623-7fecc680-c49f-11eb-8cb0-dfed86354190.png)

so let go to iam->users->security credentials -> SSH keys for AWS CodeCommit , so as we are outputting the https url its easier to get https git credentials which already have generated 

![image](https://user-images.githubusercontent.com/33585301/120654898-bfb3ae00-c49f-11eb-997a-b76170dde12e.png)

![image](https://user-images.githubusercontent.com/33585301/120654987-d4904180-c49f-11eb-8398-52cfb6aa4b49.png)



If not and if prefer ssh keys we can also do that . <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_ssh-keys.html">Link</a>


So for now going to using already created git credentials 

_________________________

In our terminal we will got to the route of our resource api / , and lets start pushing our code into our master branch ,  so inorder to do that we need to  <a href="https://git-scm.com/book/en/v2/Getting-Started-Installing-Git">git installed<a>  in our machine
  
  
git init initialize our git repository 
  
  need to add git remote origin and the url we just got out of the cloud formation or if you are using the ssh we can go to the code commit and get ssh  url 
  

  ![image](https://user-images.githubusercontent.com/33585301/120745216-49a05d00-c51a-11eb-9a4f-6851f145f273.png)

  ![image](https://user-images.githubusercontent.com/33585301/120745228-5329c500-c51a-11eb-88eb-6389df2b7058.png)

  
  
  Now add everything to the folder with the git add . command it will add all resource api source code 
  
  and then git commit with initital commit message . It has commited all the files in here 

Then push it to remote master branch 
  
  ![image](https://user-images.githubusercontent.com/33585301/120745286-75234780-c51a-11eb-836d-0559930a1ccf.png)

  
 If we check on the management console we can confirm that the code is available in isolated repo . 
  
  
  
______________
  
## Second flow of our workflow 
  
  https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/cicd/cicd-2-ecr-and-build.json
  
  
  ![image](https://user-images.githubusercontent.com/33585301/120746245-6dfd3900-c51c-11eb-85c6-7e24d82e3f31.png)

what can we do with our code , the second step is to introduce now to code build & ECR so that the docker image of the resources api can be pushed to ECR and then being used 


, so lets go back to code and re-visit 2nd CF template , that will generate the code build job and the ECR .
  
  Now will update the stack with the second template .
  
  
  ![image](https://user-images.githubusercontent.com/33585301/120746982-e284a780-c51d-11eb-84ab-df2ae57f752f.png)

  
  We are going to be specifiying 2 more parameters : 
  
  ![image](https://user-images.githubusercontent.com/33585301/120747145-28da0680-c51e-11eb-8357-7f039ac7e6b0.png)

  
  - Buildspeclocatoion : which is the buildspec.yml file which is the code build uses as the instructions 
  - CICDbranch : in our case it will be master , can specify some other type of branches based on requirment 
  
  
  Downbelow on the resources section we now can see have not only the code commit repo but multiple other new services 
  
![image](https://user-images.githubusercontent.com/33585301/120747387-8d956100-c51e-11eb-9b48-aa628bde8e1d.png)
  
  
  ![image](https://user-images.githubusercontent.com/33585301/120747743-35ab2a00-c51f-11eb-9624-dd34b8487e80.png)

  
  - ecr repo with the name on it , its going to be ready for start recieving all the pushes of the new docker images 
  - iam service role - and policy will be attaching to that role . this is for using this role coeebuild and allow it to push docker images into ECR 
  - Codebuildproject - this is one in charge for taking the source code for codecommit building the docker image and pushing that directly into that ecr repo 
  
  HOw the process actually happens , how codebuild is instructed to do the build process , each microservice in here has its on buildspec file 
  
  
  ![image](https://user-images.githubusercontent.com/33585301/120747984-a2262900-c51f-11eb-995d-4e203b954598.png)
  
  this file has all the instruction that code build will follow inorder to our docker image , there are something that you will have to change in here. 
  
  - App:   this one should be same as the one we put in the cloudformation template
  - ECR_URL : this is just the initial URL of the ecr repostiory , we are about to see that in a second , when we create the CF stack we need to update this . 
  - ECR_REPO_NAME : we need to change the last part , with the value we put on the app name 
  
![image](https://user-images.githubusercontent.com/33585301/120748435-650e6680-c520-11eb-89fa-65695c4f8fdd.png)

 ________________________________
  
  
  

  Lets go back to CF and update the stack . 
  
  ![image](https://user-images.githubusercontent.com/33585301/120748581-a3a42100-c520-11eb-9569-fcab6673408d.png)

  cicd-resource -> update 
  
  
  ![image](https://user-images.githubusercontent.com/33585301/120748652-c46c7680-c520-11eb-81ca-afd5112f068d.png)

  
  ![image](https://user-images.githubusercontent.com/33585301/120748783-f5e54200-c520-11eb-9432-17b093a26747.png)

  
  we already have values pre-loaded , this is the location of the build spec file and branch we want to use for the CICD 
  
  check ack -> 
  
  ![image](https://user-images.githubusercontent.com/33585301/120748978-51afcb00-c521-11eb-875d-b15451cc622f.png)

  on the change set preview we can see the new resource CF will add for us 
  -cocebulid
  - ecr
  - iam asset that will be attached to the codebuild project to allow it to push directly into the ecr repo 
  
  
  NOw CF stack is update 
  
  ![image](https://user-images.githubusercontent.com/33585301/120749167-a7847300-c521-11eb-9b88-5e493fbf395e.png)
  
  
  IN resources , we will able to see which all assets created for us . 
  
  ![image](https://user-images.githubusercontent.com/33585301/120749189-b9661600-c521-11eb-837c-eace6bc15cc5.png)

  

  
  _________
  
  ## ECR repo 
  
  ![image](https://user-images.githubusercontent.com/33585301/120749251-dc90c580-c521-11eb-83b3-a1b6b84a8174.png)
  
  
  Take the URI as base for  build spec file so it will work fromor side 
  
  first part of URI is ECR url & last part is ECR name 
  
  ![image](https://user-images.githubusercontent.com/33585301/120749388-25487e80-c522-11eb-9550-a92718abc064.png)

  Once this part is updated we can push the code into the repo again 
  
  This is all update and pushed again to code commit let go back to aws management console and look for the job in codebuild 
  
  

  
  
