


[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/external_dns.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/installing_the_bookstore_p2.md)



# Chapter - 2 
# Networking and EKS

## 11. Installing the Bookstore - Part 1

We will be installing our bookstore app , first we have to set everything ready for recieving the app itself .
configure and install couple of things that we will be require for next lecture were we are going to be installing our docker app 


![image](https://user-images.githubusercontent.com/33585301/119609096-a1511100-be14-11eb-8d0b-8399e7e6f29c.png)

instance type is t2.micro which is pretty small machine . it wont allow us to deploy the no: of containers that we need .
so using eksctl we need to delete this node group and create a new one with larger instance type , since eksctl doesn't really manage node groups ,
inorder to a ___ ? lets say an ami or the instance type in our case , we need to delete old node group and crate new one .

But before that we need to de attach new policy that wasn't created with eksctl command .

In this case the external dns policy we attach in the previous lab .




![image](https://user-images.githubusercontent.com/33585301/119609179-c5aced80-be14-11eb-976c-b4d50176a895.png)


In policy we can see we have the 3 main policy that we attach to this roles in the very beginning of the creation ,
this wont prevent us to delete a node group but  top one will (externldnspolicy) the custom one we created .




![image](https://user-images.githubusercontent.com/33585301/119609187-ce9dbf00-be14-11eb-8c95-4f4c4195fd5b.png)


lets de attach it .

![image](https://user-images.githubusercontent.com/33585301/119609198-d52c3680-be14-11eb-8278-3e72d58d2357.png)

Now it is ready to go and ready to be deleted .

![image](https://user-images.githubusercontent.com/33585301/119609253-ed9c5100-be14-11eb-8ec6-4d44760ed149.png)

___________________

## terminal 

using the eksctl get node group command will return the node group of this cluster .
then with the eksctl delete node group we will delte the node 

![image](https://user-images.githubusercontent.com/33585301/119609362-14f31e00-be15-11eb-84d4-b4a530cbe383.png)




![image](https://user-images.githubusercontent.com/33585301/119610250-8da6aa00-be16-11eb-96be-643812b9ba3b.png)

we are creating another node group with the subfix of v2 .(t2.medium ) . which is enough for maintaining all the pods we need in place .
Now creating the node group running eksctl .

![image](https://user-images.githubusercontent.com/33585301/119610315-a6af5b00-be16-11eb-8d45-81437c7613fa.png)


in the aws management attach external dns policy 

![image](https://user-images.githubusercontent.com/33585301/119610441-deb69e00-be16-11eb-9251-b82de4396f5e.png)


to attach another policy to this role which is related to dynamo db . because our application is using dynamodb as the data source .
So we need to make sure it has permissoin to consume dynamo db . Give dynamo db full access .
In the next chaper we are going to look at this more deeply and assigning the special permissions to each pod 



![image](https://user-images.githubusercontent.com/33585301/119610487-f130d780-be16-11eb-8b75-685ee613a5e9.png)


we are going to expose our application thourgh application load balancer , for that we need our application load balancer installed in our k8s cluster .
github link is given for understing little bit more of  what this controller is about .

so due to external dns application load balancer controller requires iam permission so we have cloudformation template with all the permission it needs to run correctly .

________________

Reviewing CF template 

![image](https://user-images.githubusercontent.com/33585301/119610548-09a0f200-be17-11eb-99f9-e1634dbf41cb.png)




![image](https://user-images.githubusercontent.com/33585301/119610619-250bfd00-be17-11eb-8890-402cf5a02524.png)





the only resource this thing is crating is "Albcontrolleriampolicy" , which the policy document is taking directly from  documentation of the alb controller in the gihub side 

of the project . we just have to put all those permission inside in here . And at the end put the name of the policy .


---




In cloud formation to create that stack 

use this template 


https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/iam/alb-controller.json


![image](https://user-images.githubusercontent.com/33585301/119610772-5dabd680-be17-11eb-825d-9d08f1f9fefa.png)


![image](https://user-images.githubusercontent.com/33585301/119610788-66041180-be17-11eb-8111-7085a14ee184.png)


![image](https://user-images.githubusercontent.com/33585301/120307768-fdbe9f80-c2f0-11eb-951c-18caed23ced1.png)




now it is created . 


_________

back to iam -> roles -> attach policies -> AlbControllerPolicy (attach) , now all the permission albcontroller needs are installed on our cluster 


![image](https://user-images.githubusercontent.com/33585301/119610883-8a5fee00-be17-11eb-87d7-1865ae5be070.png)



____

Let go and install that controller on our terminal 



![image](https://user-images.githubusercontent.com/33585301/119611007-ba0ef600-be17-11eb-8770-436a3ef8df62.png)


The alb-ingress-controller.yaml which is the deployment of all the rest of the things and all  the rest of the k8s  objects required for these application .
Quick look of it : we have the deployment with all the configuration possible again on the references (https://github.com/kubernetes-sigs/aws-load-balancer-controller)
you can find all the documentation about it and multiple types of configurations apply to it .


important tip : 
-ingress class flaging we need to keep this in mind , all the ingress flag are going to processed by the alb controller 
-cluster-name flag needs to match your k8s cluster name , 

in the local copy we need to change these naming here , otherwise these wont work . 


rest of the config of the repo of these project you are going to find if you are going to adjust there accourding to there needs . 




![image](https://user-images.githubusercontent.com/33585301/119611106-d874f180-be17-11eb-8037-916ca14de982.png)


![image](https://user-images.githubusercontent.com/33585301/119611119-de6ad280-be17-11eb-9120-358f44f10712.png)


lets create the resource by below listed command 



![image](https://user-images.githubusercontent.com/33585301/119611187-fc383780-be17-11eb-8771-be9d7ece435e.png)



______________________


in this lecture we have created our cluster  using eksctl for supporting the man of pods that we are going to have with our application  .

We have also attached the dynamodb policy to the worker nod iam role so that it is alble to manage all the table to our applicaation 

we have installed the alb ingress controller with its permission into the cluster 

![image](https://user-images.githubusercontent.com/33585301/119611219-078b6300-be18-11eb-98a5-745e5adf7242.png)

