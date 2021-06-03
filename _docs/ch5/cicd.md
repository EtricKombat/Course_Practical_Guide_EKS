

[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch4/comparision.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch5/workflow_definition.md)



# Chapter - 5
# CICD

## 26. CICD




![image](https://user-images.githubusercontent.com/33585301/119654770-6c5db200-be46-11eb-9461-2248ec051ad4.png)


In this chapter we will define an simple automated worflow that will take tht code from it conception all the way up to the production .

We will be using come of the services of the code suite or developer suites on aws like code build , code commit , code pipeline which are going to be integrated with  eks.


And ofcourse all of this will be driven by infrastructure as a code . with cloud formation

![image](https://user-images.githubusercontent.com/33585301/119654930-98793300-be46-11eb-90fb-464ed50acd3b.png)

1) Explain the workflow that will dirve the code across environment into different phases. 
2) We will get pratical creating the initial set of assets we required which are the code commit repo & ECR , We will also use code build to create docker image of the repositroy 
3) once we have that in place we will use code pipeline to automate the buiding of the app from the source code and then deploy it to our k8 cluster . At this point we have complete automated workflow for the development environment 
4) we will proceed and create production environment 
5) Finally we will also automate the workflow that will already past development directly to production 



![image](https://user-images.githubusercontent.com/33585301/119654997-af1f8a00-be46-11eb-8011-778a548f201c.png)

As mentioned we will be using some of the code suite of aws which are :

-code commit for source coe management offer microservices in isolated repository
-code build for building our docker images pushing them to ecr and also running deployment instruction which in our case are based on helm . 
- code pipeline to connecting everything to gather applying automation to workflow . 


![image](https://user-images.githubusercontent.com/33585301/119655095-ccecef00-be46-11eb-9fbd-54c0dc2f59cf.png)

As we have been doing in all the chapters , let direct all we are doing to the well architecture framework in this case we are going to be 
specific in 2 of the question of the operation excellence piller . 


