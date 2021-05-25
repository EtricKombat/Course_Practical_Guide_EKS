



[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch1/The_BookStore_project.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch1/Demo_Creation_of_the_physical_requisites.md)



# Chapter - 1
# Introduction

## The Bookstore Project : Prerequisites 


![image](https://user-images.githubusercontent.com/33585301/119455071-dc8d0a80-bd56-11eb-9851-67fb45e3624c.png)

Explaination of how the whole architecture is located in AWS 
__________________________



![image](https://user-images.githubusercontent.com/33585301/119455142-f0d10780-bd56-11eb-87bf-be0be63f29e2.png)

A VPC is created in northern Virginia ,
__________________________

![image](https://user-images.githubusercontent.com/33585301/119455174-f75f7f00-bd56-11eb-84ed-7c51c2d28022.png)


 for HA we will be using 3 AZ , not all AZ support EKS (in 2020) 
__________________________

![image](https://user-images.githubusercontent.com/33585301/119455211-021a1400-bd57-11eb-8397-eb7aac8ab262.png)


Each AZ we will have one public subnet , where we want to locate the resources that we will be accessable from the internet 
__________________________

![image](https://user-images.githubusercontent.com/33585301/119455289-1231f380-bd57-11eb-904f-036e6e166737.png)

set of private subnet is required , this will contain most of our resources like worker nodes , putting this kind of resouces in our private subnet 
help our architect to be more secure since this are not accable from internet 

__________________________

![image](https://user-images.githubusercontent.com/33585301/119455312-19f19800-bd57-11eb-97bd-454d5183fb71.png)

in terms of worker nodes we will have an auto scaling group with ec2 instances that will work as the worker node for our 
k8 cluster later we will see that this change whether we want to use on demand or spot instances  or even go o serverless side with fargate

__________________________

![image](https://user-images.githubusercontent.com/33585301/119455351-25dd5a00-bd57-11eb-84ba-23f76e37db95.png)

on top of public subnet we will create nat gatways  so that the resources from the public subnet can access interneet , 
this is for download for software required , updates,security patches  and so on . Also this can be used to acess resource such as s3 , dynamodb .
Although most secure and effective  way to access this resource is via VPC endpoint will keep this simple to maintain the  cost of the solution low as possible
After the creation of NAT gateway we will have to address the route table so that private subnet whenever they want to got to the internet simple point the traffic to NAT gateway , 
__________________________

![image](https://user-images.githubusercontent.com/33585301/119455468-41486500-bd57-11eb-8a62-ac43b12f06d9.png)

ofcouse all of the worker nodes are going to be managed by control plane provided by EKS 
__________________________

![image](https://user-images.githubusercontent.com/33585301/119455532-4f968100-bd57-11eb-9265-87c8b3d0c5c2.png)

When we start deploying  our applications some of them are going to have private application load balancer so that we can access from the network such as development environment which we dont want to internet accessible 
__________________________

![image](https://user-images.githubusercontent.com/33585301/119455573-59b87f80-bd57-11eb-9909-4a942e2b4203.png)

And the front end will be expose through the public load balancer 
__________________________

![image](https://user-images.githubusercontent.com/33585301/119455854-a8feb000-bd57-11eb-8251-28a2c304425b.png)

so that it can be reached from internet , which is the intented 
__________________________

![image](https://user-images.githubusercontent.com/33585301/119455822-9c7a5780-bd57-11eb-82fb-f208607e3960.png)

DynamoDB tables will be created as well . these are not really tied to a VPC ( It was put this in here to represent the worker nodes will access them 

__________________________

![image](https://user-images.githubusercontent.com/33585301/119455909-b9168f80-bd57-11eb-80fc-09c333880434.png)

__________________________
