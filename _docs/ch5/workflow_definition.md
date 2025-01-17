


[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch5/cicd.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch5/source_code_%26_build_part_1.md)



# Chapter - 5
# CICD

## 27 . Workflow Definition


![image](https://user-images.githubusercontent.com/33585301/119655381-26551e00-be47-11eb-93dc-786ef5d3ab0b.png)


We are going to be defining CICD workflow that we are going to be creating during this chapter .


![image](https://user-images.githubusercontent.com/33585301/119655454-3a991b00-be47-11eb-82b8-63d8aa68e8ec.png)


Initial source , code . Each microservices will have git repo in our code commit .


![image](https://user-images.githubusercontent.com/33585301/119655476-41279280-be47-11eb-90a5-be768e689c2f.png)

Each of them will start with a master branch , for this workflow no push into master branch are allowed . 



![image](https://user-images.githubusercontent.com/33585301/119655663-78963f00-be47-11eb-8e3c-07cb5a869b37.png)


So Developer will have to create copy of master where he will start coding the task they were asked to do . 

![image](https://user-images.githubusercontent.com/33585301/119655679-7d5af300-be47-11eb-8695-07998a6f78a2.png)



This is a process that could involve multiple commit so this same branch as he works to it . 

![image](https://user-images.githubusercontent.com/33585301/119655725-877cf180-be47-11eb-9d92-08667ed89833.png)


When the developer is done we will request merging the code into the branch via the pull request .

![image](https://user-images.githubusercontent.com/33585301/119655757-8e0b6900-be47-11eb-8186-bd0183b1194b.png)

 Once merged we will trigger the automated build job that will create docker image and pull it to ecr 

![image](https://user-images.githubusercontent.com/33585301/119655894-b3987280-be47-11eb-93da-1cb76438f10f.png)

Its important to notice that we have one ECR per micro service . Then after push the image we will automatically trigger another job to deploy directly into the development environment , after the feature is tested in this environment and if everything looks good then we will need a manual approval to push the same package to the production environment ready for being consumed by the customers . 

![image](https://user-images.githubusercontent.com/33585301/119655934-bf843480-be47-11eb-8c4e-b42110174806.png)




![image](https://user-images.githubusercontent.com/33585301/119656001-d034aa80-be47-11eb-834f-bd2fa8879d8d.png)

Ths process is iterative allowing the development team to continue creating feature branhes of master and following the same process 

________________________________

Lets review more indepth  of the workflow now from the aws services used for making this real .

We start from the source code which is stored in code commit repo 


![image](https://user-images.githubusercontent.com/33585301/119656308-2ace0680-be48-11eb-9ece-3f7c4e89af39.png)


when a new commit comes into the master branch it will trigger cloudwatch event that is listening this kind of events 

![image](https://user-images.githubusercontent.com/33585301/119656049-de82c680-be47-11eb-8006-a0bf90e84eda.png)

then code pipeline appears to be the orchastrator of the stages of the workflow 


![image](https://user-images.githubusercontent.com/33585301/119656066-e3e01100-be47-11eb-8303-d3634cab16e6.png)

the first stage is source where the code is retrived by code build 


![image](https://user-images.githubusercontent.com/33585301/119656080-e8a4c500-be47-11eb-98bf-688a5ec5b628.png)

next we build docker and push it to ecr this is again code build


![image](https://user-images.githubusercontent.com/33585301/119656108-efcbd300-be47-11eb-9f77-1566a234ce92.png)


next phase is to grab the image that was recently pushed into ecr and deployed to the development environemt on k8s using helm 

![image](https://user-images.githubusercontent.com/33585301/119656413-4afdc580-be48-11eb-9594-e8c3ecc36638.png)

After this step is done code pipeline will be waiting for a manual approval until lets say QA team validates the environment and give the greenlight to push to production 


![image](https://user-images.githubusercontent.com/33585301/119656126-f65a4a80-be47-11eb-8065-8e6faf4dab36.png)

Now when this step happens code build will push the version of the docker image into the prodcution that is the end of the workflow . 


![image](https://user-images.githubusercontent.com/33585301/119656145-fd815880-be47-11eb-9a6a-405141ce713e.png)




_________________________




![image](https://user-images.githubusercontent.com/33585301/119656589-7d0f2780-be48-11eb-8585-b732fa51f951.png)


In a CICD work flow it is also important to define the versioning format that will be used .
In our case this is how it is going to work .

Each repostiory has version file which has 2 environment variables . 


MARJOR & MINOR these 2 are number that we are going to use in the version tag 

In the other hand we have code commit id or the git commit sha which is unique in whole repo and identify specific commit in the history.

We are going to take the first 8 charater .

So we take 2 components and build the version tags that will be used as tag for the docker image that will help us to identify version of the application and also to identify
which specific commit was it created from . 


