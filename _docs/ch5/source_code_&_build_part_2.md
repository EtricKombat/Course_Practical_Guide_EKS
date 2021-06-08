


[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch5/source_code_&_build_part_1.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch5/automatic_build_%26_deployment.md)



# Chapter - 5
# CICD

## 29. Source Code & Build - Part 2


This is the continuation of the previous lab , in the prev lab we did the 1st and second step of the architecture . 
Which was creating the code commit repo upload the code to it , create the codebuild project and ecr for building the docker from the source code . 




![image](https://user-images.githubusercontent.com/33585301/119656761-adef5c80-be48-11eb-810a-c3c355c11326.png)

In this lab we are going to continue the rest of the architeture adding the automation required for triggering code build when we push changes to master branch . 

This is going to be possible using code pipeline 


![image](https://user-images.githubusercontent.com/33585301/119765482-f94d4d80-bed0-11eb-987c-4baa5afe91bf.png)




![image](https://user-images.githubusercontent.com/33585301/119765901-ac1dab80-bed1-11eb-88b4-003e30bd49ca.png)

_______________________________

For start doing that we will have to update the CF stack with the 3rd CF template we have on our code , so let go and visit that one .

![image](https://user-images.githubusercontent.com/33585301/119765929-bcce2180-bed1-11eb-959e-8baf1bf513f3.png)


IN the very first part we can serve the same 3 parameters , we have then the 5 resources we already have . And the new stuff after that 

![image](https://user-images.githubusercontent.com/33585301/120753015-31cfd580-c528-11eb-88e8-141f61e25a97.png)

IAM policies we are going to be attach to the codepipelines with all the permissions  we requires . Then we create the bucket and its bucket policies that will work as the artifact of the pipeline itself 

![image](https://user-images.githubusercontent.com/33585301/120753091-457b3c00-c528-11eb-9efd-0068b6c60271.png)


Then down in here we have cloudwatch event rule

![image](https://user-images.githubusercontent.com/33585301/120753310-9be87a80-c528-11eb-9bb2-dff8a53c3ac8.png)

which source is code commit and repository is the one we created in this stack . 

![image](https://user-images.githubusercontent.com/33585301/120753400-c1758400-c528-11eb-86ba-c3d7fa90e252.png)

It will be listening to the all the reference created or updated to the branch on th one we passed . Which in our case is the master branch .


![image](https://user-images.githubusercontent.com/33585301/120753449-ccc8af80-c528-11eb-845c-6dc968fd6c71.png)

when this event rule triggers the target will be the codepipeline for this specific micro services . 

![image](https://user-images.githubusercontent.com/33585301/120753482-db16cb80-c528-11eb-8d5a-daf76983fd14.png)


If we continue scrolling down we find the IAM asset to attach to the cloud watch to specifically allow to start the pipeline execution of the pipeline proejct of this micro service 

![image](https://user-images.githubusercontent.com/33585301/120753558-f97cc700-c528-11eb-94a2-5513ba13eeb1.png)


And finally we have the definition of the pipelie . Lets look at the stages of this .

![image](https://user-images.githubusercontent.com/33585301/120753587-04375c00-c529-11eb-8569-ec1e0aa3c980.png)

stage 1) source : which is getting all the code from the code commit repository and from the master branch or the branch we specified in the stack 


![image](https://user-images.githubusercontent.com/33585301/120753608-0dc0c400-c529-11eb-866f-aed621bff248.png)

stage 2) build : this stage is pointing to the code build project we just saw running and building our code and pushing the docker image directly to ECR  


Thos are the new changes for this template. 


https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/cicd/cicd-3-automatic-build.json




___________________________________________

Lets got to CF and lets update this . 

![image](https://user-images.githubusercontent.com/33585301/119766187-2fd79800-bed2-11eb-8af7-58470358507c.png)


Change it with the 3rd file . same parameter . 


![image](https://user-images.githubusercontent.com/33585301/119766216-3c5bf080-bed2-11eb-98d6-f8ca4b618305.png)


It has finished the update of the stack . 

![image](https://user-images.githubusercontent.com/33585301/119766244-50075700-bed2-11eb-80d8-0ad6b77857b6.png)

Lets now go to most resource this one should have created which is code pipeline . This is the pipeline we have created as you can see 'most recent exectution' is failed . 
because of the lack of permission on IAM the CF was still creating . So dont worry about that specifically .  Now with this pipeline in place we sure already have the ability to push new changes and immediately have this pipeline to execute automatically . 

So lets go to the terminal to commit new change to repository , and push it to the master branch to test it to the automation 


![image](https://user-images.githubusercontent.com/33585301/119766261-5a295580-bed2-11eb-964a-347f0b813ac4.png)



______________

## in the terminal 

A simple change just for triggering and testing automation with code pipeline 



![image](https://user-images.githubusercontent.com/33585301/119766549-d7ed6100-bed2-11eb-90f5-f3579c741032.png)


Once we change MINOR=0 to MINOR=1 just for testing 



![image](https://user-images.githubusercontent.com/33585301/119766594-efc4e500-bed2-11eb-9c03-9544143961a7.png)


![image](https://user-images.githubusercontent.com/33585301/119766630-fce1d400-bed2-11eb-9908-e6f5105db06f.png)



It pushed our file . Now let go back to our AWS management console . And then see the code pipeline started immedialty 


![image](https://user-images.githubusercontent.com/33585301/119766678-1551ee80-bed3-11eb-96b1-3df5d057a3a4.png)

It already finished the firet state , which was getting the src code from code commit . Now it is building it . After build and then we can go to ECR and verify that the new image was creating in there . 


ecr - > bookstore.resources.api 


as you can see we have a second image which starts with '1.1.*'  , we updated this in the versions file . We change the MINOR number to 1 and now we  get this upgraded version of the code . everthing went automatically just with the simple push of that commit . 


Lets review what we have achieved in this process


![image](https://user-images.githubusercontent.com/33585301/119766713-2569ce00-bed3-11eb-8948-f5a64383d355.png)


____________________________________________________

This diagram show what we have already achieved . 

We did this in 3 stages 


1) Creating with CF codecommit repo  & push the code into it using git . 
2) Then with CF again we created the code build project that we could manually trigger and then push the docker image just  build in to the ECR 
3) Another CF template updated everything creating the automation required for the code pipeline from the cloudwatch event , the s3 bucket for the artifacts & connecting everything so now every simgle time we push changes to the master branch we are going to have all this flow automatically . 

From here we will be continuing to upgrading &  expand our workflow 


![image](https://user-images.githubusercontent.com/33585301/119766844-5e09a780-bed3-11eb-96f4-62d5e9d6ac3e.png)


![image](https://user-images.githubusercontent.com/33585301/119766773-40d4d900-bed3-11eb-8332-34583d3f87a1.png)




![image](https://user-images.githubusercontent.com/33585301/119766969-a32dd980-bed3-11eb-866f-59a8202576df.png)

