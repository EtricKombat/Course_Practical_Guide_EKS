


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


And finally we have the definition of the pipelie

![image](https://user-images.githubusercontent.com/33585301/120753587-04375c00-c529-11eb-8569-ec1e0aa3c980.png)


![image](https://user-images.githubusercontent.com/33585301/120753608-0dc0c400-c529-11eb-866f-aed621bff248.png)


Thos are the new changes for this template. 


https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/cicd/cicd-3-automatic-build.json




___________________________________________


![image](https://user-images.githubusercontent.com/33585301/119766187-2fd79800-bed2-11eb-8af7-58470358507c.png)


![image](https://user-images.githubusercontent.com/33585301/119766216-3c5bf080-bed2-11eb-98d6-f8ca4b618305.png)

![image](https://user-images.githubusercontent.com/33585301/119766244-50075700-bed2-11eb-80d8-0ad6b77857b6.png)


![image](https://user-images.githubusercontent.com/33585301/119766261-5a295580-bed2-11eb-964a-347f0b813ac4.png)




![image](https://user-images.githubusercontent.com/33585301/119766549-d7ed6100-bed2-11eb-90f5-f3579c741032.png)


![image](https://user-images.githubusercontent.com/33585301/119766594-efc4e500-bed2-11eb-9c03-9544143961a7.png)


![image](https://user-images.githubusercontent.com/33585301/119766630-fce1d400-bed2-11eb-9908-e6f5105db06f.png)


![image](https://user-images.githubusercontent.com/33585301/119766678-1551ee80-bed3-11eb-96b1-3df5d057a3a4.png)

![image](https://user-images.githubusercontent.com/33585301/119766713-2569ce00-bed3-11eb-8948-f5a64383d355.png)



![image](https://user-images.githubusercontent.com/33585301/119766844-5e09a780-bed3-11eb-96f4-62d5e9d6ac3e.png)


![image](https://user-images.githubusercontent.com/33585301/119766773-40d4d900-bed3-11eb-8332-34583d3f87a1.png)




![image](https://user-images.githubusercontent.com/33585301/119766969-a32dd980-bed3-11eb-866f-59a8202576df.png)

