


[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/CNI_and_eks_integration_with_VPC.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch3/achieve_least_privileged_principle_containers_with_there_own_permission.md)



# Chapter - 2 
# Networking and EKS

## 14 . Demo : CNI

![image](https://user-images.githubusercontent.com/33585301/119605817-0c97e480-be0f-11eb-933d-1b9c2f74f089.png)


In this lab we are going to start creating and make it real all the customization were talked about in the previousl lecture on the CNI plugin 


![image](https://user-images.githubusercontent.com/33585301/119606000-6698aa00-be0f-11eb-9e88-8eb40c9824be.png)


Lets check the container ip just to see what for example  what the inventory API has : 

the ip is 10.0.97.55 , in the previous lecture we been using cidr of the vpc (10.0.0./16) , the possiblity the CNI gives us is to expand this range is to secondary
cidr on our vpc 



![image](https://user-images.githubusercontent.com/33585301/119605967-541e7080-be0f-11eb-86f3-d49b326c3165.png)

for doing that we need to customize our vpc

1) for adding our cidr
2) we need to create some new subnet using the new cidr 
3) we need to attach them to some route tables and point them to NAT table because we want these subnet to be private 

all of these can be done in the management console . 




![image](https://user-images.githubusercontent.com/33585301/119606141-a1024700-be0f-11eb-8c95-2740649ca922.png)






===

but inorder to maintain a repeatble patter in our architecture we are going to be doing all of these through our bash script 



![image](https://user-images.githubusercontent.com/33585301/120460262-979c5000-c3b6-11eb-974e-926f3d5d2d05.png)

inside infrastructure/k8s-tooling/3-cni/

- script
- CF template (subnet.json)
- 3 yaml files 

Resources 

bash script 
https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/k8s-tooling/3-cni/setup_secondary_cidr.sh




---

cloudformation template 
https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/k8s-tooling/3-cni/subnets.json


---

https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/k8s-tooling/3-cni/us-east-2a.yaml

https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/k8s-tooling/3-cni/us-east-2b.yaml

https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/k8s-tooling/3-cni/us-east-2c.yaml


_________________________

## in the code editor : 



![image](https://user-images.githubusercontent.com/33585301/119606486-4f0df100-be10-11eb-9616-127055e07470.png)


![image](https://user-images.githubusercontent.com/33585301/120461374-94ee2a80-c3b7-11eb-9e64-1c0a697aa4b1.png)


part-1
we have a series of variables 

- with the CF stackname of the cluster
- with the region which we are going to be deployed into
- cluster name 
- the secondary cidr 
- cidr of 3 new subnets we are going to be creating , keep in mind that these cidr need to be part of main one . 

part-2 

we are going to get tehe vpc id using the aws cli cloudformation command describing the stack resources and getting the vpc id 

Then we are going to use again the aws cli but this time for associating the secondary cidr block to vpc 

Then we are gonna retrive the nat gateway id again doing a describe natgatway command from the ec2 section of the aws cli & retrieving the id 


part-3 

we are gonna deploy our cloudformation stack 

which template is subnet.json 

overriding all the parameters that we have in this script



Quick note in here is that , everything that in this script is creating could be done throught the aws management console . through a manual process 

Refer link (help setup secondary ) : https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#VPC_Sizing

doing this way to avoid error from manual process 

____________________________

Check our CF template 


![image](https://user-images.githubusercontent.com/33585301/120462881-f9f65000-c3b8-11eb-9c4e-dd946424536e.png)


![image](https://user-images.githubusercontent.com/33585301/120464032-1c3c9d80-c3ba-11eb-92e6-586571b7a5dc.png)

![image](https://user-images.githubusercontent.com/33585301/120464064-25c60580-c3ba-11eb-9229-31eba3e23587.png)

![image](https://user-images.githubusercontent.com/33585301/120464094-2fe80400-c3ba-11eb-8550-bdf4e7859e60.png)



Specially we will be able to see  : subnetA, subnetB, subnetc 


![image](https://user-images.githubusercontent.com/33585301/120464181-468e5b00-c3ba-11eb-918b-2a9670a02409.png)


down route table for the subnet a , route table for the subnet b , route table for the subnet c 

down route specific to the nat gatway A attaching it to subnet A ,route specific to the nat gatway B attaching it to subnet B ,  route specific to the nat gatway c attaching it to subnet c


![image](https://user-images.githubusercontent.com/33585301/120463923-03cc8300-c3ba-11eb-9f35-3731c08f1886.png)


so what CF is actually  doing is creating those extra subnet we need in that secondary cidr on our vpc and creating the route tables and routes for allowing the traffic to go to  the nat gateway and by default aws wants to create secondary cidr will modify the route table so we dont have todo anything else . 


![image](https://user-images.githubusercontent.com/33585301/120464753-e815ac80-c3ba-11eb-81f8-e2632e9bce14.png)


outputing all the subnet ids


___________________




![image](https://user-images.githubusercontent.com/33585301/119606739-b62ba580-be10-11eb-9bfb-2a6c540caa2c.png)


It already associate the secondary cidr , itz going to retrive the nat gateway and CF stack is created . 


IN the resources of CF , we are going to find all the route tables , the routes & the subnets . 

![image](https://user-images.githubusercontent.com/33585301/119606823-d8bdbe80-be10-11eb-8a3a-233cf80b6e80.png)




![image](https://user-images.githubusercontent.com/33585301/119606949-1884a600-be11-11eb-9c37-226b61bd37aa.png)

we will find 3 new subnets all of them with secondary on there name , if we scroll to the right we will see the IPv4 cidr start with 100.64. and so on 

so it means that we have vpc specified for start customizing our CNI pLUGIN 



![image](https://user-images.githubusercontent.com/33585301/119606966-21757780-be11-11eb-8b80-5eb2d407ec1a.png)


______________

CNI is already running in here  . Deamonset running in kube-system namespace , 'aws-node' has CNI plugin on it 

The method used for customizing this is throught : 

1) env variable
2) ENI config resource , which is a custom resource definitin on k8S


Ref : link to list of env variable that can be used for customizing CNI 

https://docs.aws.amazon.com/eks/latest/userguide/pod-networking.html

https://docs.aws.amazon.com/eks/latest/userguide/cni-custom-network.html






![image](https://user-images.githubusercontent.com/33585301/119607181-77e2b600-be11-11eb-8c87-1307bcb2a507.png)



Make sure that version of the guide is higher than 1.6 . If we dont have this version (command :  kubectl apply -f https://raw.githubusercontent.com/aws/amazon-vpc-cni-k8s/master/config/v1.6/aws-k8s-cni.yaml)


![image](https://user-images.githubusercontent.com/33585301/119607440-f6d7ee80-be11-11eb-9772-6f2de7249e55.png)



set 2 env variable to the demonset of the CNI 

1) boolean that is to be set as true in order to customize the CNI behaviour 
2) label of the worker node which value needs to match with the name of the any config the custom resource defintion we are about to create and CNI will use that one for 
   assigning the configuration to this specific worker node 
   
   
   ![image](https://user-images.githubusercontent.com/33585301/120596710-b1df3800-c461-11eb-8824-644dff596dcc.png)

Lets take a look at one of the nodes and see what that label has as values 

in the labels section we are going to find that , this specific node has the label that we specified in the environment varialble with the availablity zone name as value 
this is very important , because now we have to create the ENI config using these values so as you know we are using a multi az deployment for cluster 

![image](https://user-images.githubusercontent.com/33585301/119607515-1c64f800-be12-11eb-9d9f-95ba0d752a60.png)

And if we have 3 nodes 3 subnets and using the 3 availablity zone in these region each node should have us-east-2a,us-east-2b,us-east-2c

This is important because the second configuration we set into deamonset we ll use these value and match it with the name of the custom resource definitions we are about to create .  
   

![image](https://user-images.githubusercontent.com/33585301/119607805-939a8c00-be12-11eb-80ed-976531b2b910.png)


Let look at how those custom resource should look like , its a very simple one.
These resource works as a configuration for the CNI cluster to create pods in the specified subnet and with the specified security groups .
These is were all the customization start happening . 


keep something in mind , the name of the CRG(az)  is exactly the same name of the values that is appear in the nodes so CNI given in the second environment variable we said 
is going to match that name with example this configuration . so all of them look exactly the same . What we have to do is to replace the subnet and the security group we want ot attach to each of them . 

so for example if this is the us-east-2c it means it will reflect the configuration of these availablity zone we need to specify the subnet id of brand new subnet that lives in the secondary cidr of our vpc . So let me get those value of both subnet & security group that i want to attach . and put them in here . i invite to do exactly the same (use the needed subnet & security group you want in this files  ).


![image](https://user-images.githubusercontent.com/33585301/119607820-9d23f400-be12-11eb-8625-57e8a3079fa7.png)



Once all the subnets , SG we want to use and put it in the corresponding AZ  configuration file and also the security group .
For making it simple using the same 2 SG as worker but we can create any other specified in here  . 

Lets create 3 files into ur cluster . 

_____________________



![image](https://user-images.githubusercontent.com/33585301/119608045-effdab80-be12-11eb-826d-51bd7bc28df2.png)



- we checked the version of our CNI plugin , if it is less than v.16 we need to upgrade it 
- we set 2 environment variable to CNI plugin
- and we installed 2 custom resources definitions that will specifiy what subnets and what secuity groups that we are going to start deploying our pods into . 


![image](https://user-images.githubusercontent.com/33585301/119608074-fee45e00-be12-11eb-8b9c-5d1b3b098c8d.png)


so inorder to get all this configuration functinal we need to drop the existing worker nodes and the autoscaling group will create new ones with the new configuration nodes in it . And we will recreate all the  pods into the new subnet we specified . 


Go to the AWS console and drop the existing woker nodes (terminate)


![image](https://user-images.githubusercontent.com/33585301/119608271-4e2a8e80-be13-11eb-85db-c721e1e54b5b.png)



![image](https://user-images.githubusercontent.com/33585301/120619587-bca5c700-c479-11eb-8280-2d72bbabd274.png)



select all the worker nodes , action-> terminate . Make you if you already in production you want to do this process at least you have some worker nodes for backing up 

because this will drop all our work load . It will create a while to drop every thing and create new worker node and they get connected to our k8s cluster . 
Wait until new 3 worker nodes come online . 


Now we have 3 terminalte woeker nodes & 3 new ones . That replace them . 





![image](https://user-images.githubusercontent.com/33585301/119608342-70bca780-be13-11eb-9e99-890937ad3409.png)


So lets load our application . Here it is working correctly just as always which means that the configuration was 
completely transparent 

![image](https://user-images.githubusercontent.com/33585301/119608388-8762fe80-be13-11eb-8675-863b07acda88.png)

Lets take a look at real change . IPs . We should be able to see that the pods now have an IP of the secondary CIDR we attach to the vpc . 

Get the pods of the namespace 'development'  


![image](https://user-images.githubusercontent.com/33585301/119608487-a95c8100-be13-11eb-8928-af1637c596f5.png)


Grep by ip , this is the ip of the range 100.64.0.0/16 which is that secondary IP range . 


Now we have broader knowledge of now the networking and configuration of CNI works. 


__________________

Let review what we have done in this lab .



![image](https://user-images.githubusercontent.com/33585301/119608741-196b0700-be14-11eb-9afb-be97c956d603.png)


We start with our architecture , first thing we did was assign a secondary CIDR for this VPC 

![image](https://user-images.githubusercontent.com/33585301/119608854-44edf180-be14-11eb-9e62-1151a21f5255.png)

Now inorder to use it we created 3 new subnets , under the new IP range . Let point the private subnet in the diagram as the new ones . 


![image](https://user-images.githubusercontent.com/33585301/119608916-5d5e0c00-be14-11eb-8ec8-0d24838b7a12.png)

From here we started doing our CNI customizations , so we could deploy our application into these new secondary IP range of our VPC and our application continue working correctly without any problems 


![image](https://user-images.githubusercontent.com/33585301/119608577-d14be480-be13-11eb-8f62-424044d6676b.png)


so thtz all we needed to know about CNI configuration , you can do all the customization you want . 


![image](https://user-images.githubusercontent.com/33585301/119608619-e1fc5a80-be13-11eb-85d0-0774aab9460f.png)


And for learning more about it (Ref link : ? ) letting official eks cli plugin documentation from the aws side ,

During this chapter . We were assigning multiple policies directly to the worker nodes of our k8s cluster and the bac new is that this is not a good pratice but thatz fine because we will address that in the next chapter . 

