


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
  







