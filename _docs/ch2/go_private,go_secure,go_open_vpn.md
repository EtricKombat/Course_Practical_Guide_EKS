


[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/networking_and_eks.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/dns_do_not_suffer.md)





# Chapter - 2 
# Networking and EKS

## 7. Go Private , Go Secure , Go OpenVPN

![image](https://user-images.githubusercontent.com/33585301/119473607-f5062080-bd68-11eb-997a-2f52f54293c7.png)

It is an optional lecture , it the resource is hosted on public subnet this is not required 
_____________________________

![image](https://user-images.githubusercontent.com/33585301/119473643-fd5e5b80-bd68-11eb-83bc-c3f78df48662.png)

This software will allow us to use our traffic as we are  internally in the  VPC , using the private CIDR we have established for  VPC
_____________________________

![image](https://user-images.githubusercontent.com/33585301/119473791-1ff07480-bd69-11eb-8609-1c0a3623362d.png)

Lets explain how VPN will help us in our VPC 
_____________________________

![image](https://user-images.githubusercontent.com/33585301/119473814-27178280-bd69-11eb-8c97-6c5dd846b5ab.png)

If we try to hit one of EC2 we wont be able to , simply because we are not in the same network 
_____________________________

![image](https://user-images.githubusercontent.com/33585301/119473839-2c74cd00-bd69-11eb-9f18-55fe32328705.png)

Thatz when openvpn jumps into the game , because it will give us ablity to behave as we are inside it and happly access our resources.
With open vpn in pic we dont have to worry about putting mostly anything in the public subnet 
_____________________________

![image](https://user-images.githubusercontent.com/33585301/119473899-40203380-bd69-11eb-9902-07af16f87f0f.png)

if a hacker try to access ec2 in private subnet to perform any malicious activity that traffic wont be possible, because thatz the nature of the private subnet .
It promots the good pratice of protecting everything from being accessed from internet and disable any hacker attack 

_____________________________

![image](https://user-images.githubusercontent.com/33585301/119474005-5a5a1180-bd69-11eb-9151-076a54e929a9.png)


We are going to create openvpn setup using cloudformation , before that we need to do couple of things 

1) subscribe to the open vpn product on the aws market place 
2) Then we will take the AMI that open vpn is providing throught the subscription , this ami will be a parameter on our CF stack
_____________________________

![image](https://user-images.githubusercontent.com/33585301/119474046-647c1000-bd69-11eb-91e4-34b844886887.png)

got to aws marketplace subscription - > list of all the subscription in our account (openvpn subscrition above ) 

_____________________________

![image](https://user-images.githubusercontent.com/33585301/119474122-7493ef80-bd69-11eb-8399-a777538cdf67.png)

Discover product - > OpenVPN inc - > select most aligned option 

If we select any of them we need to continue to subscribe and select .

Start create ec2 instances with the pre-configured ami with openvpn inc ,

_____________________________

![image](https://user-images.githubusercontent.com/33585301/119474679-0a2f7f00-bd6a-11eb-99bd-3c822bee2c3c.png)


Multiple configuration we can set in 

Note the AMI id from here


____________________________

## Review OpenVPN cloudformation template 


![image](https://user-images.githubusercontent.com/33585301/120166424-2e7fd580-c21a-11eb-99db-59f43d89cb10.png)

## Parameter

![image](https://user-images.githubusercontent.com/33585301/120166482-3e97b500-c21a-11eb-9dec-984ff9fdb8cf.png)

![image](https://user-images.githubusercontent.com/33585301/120167923-ee215700-c21b-11eb-9c60-cf9374ccd78e.png)



-we are going to need the AMI id 
-the node volume size which is disk size in GB of the EBS we are attaching to the machine 
-the pem file we are going to be using to sh to the machine if required 
- the node instance type ec2 instance type 
- 2 parameter of the autoscaling group which are the minimum and maximum size of it (in this case 1 ) 
- subnet id were this is going to be located , in the architecture openvpn machine need to be sit in the public subnet (any of them  ) , in the CF stack we need to select the public subnet in here , autoscaling group can decide were to put it .
- VPCID we created with eksctl in previous lab 
- vpc cidr 
- allowed ip range which is basically to who we are giving access to the machine as you can see by default simply letting everybody to connect to this one (we can be more strict depending on the company policy ) 
- preconfigured openvpn machine  with  username & a password are the initial one we are going to use for accesing this machine 

## Resources 

![image](https://user-images.githubusercontent.com/33585301/120168407-70118000-c21c-11eb-8d9e-28f550bb7624.png)
![image](https://user-images.githubusercontent.com/33585301/120168946-12316800-c21d-11eb-8f4e-e41edaa5e090.png)
![image](https://user-images.githubusercontent.com/33585301/120169829-f9758200-c21d-11eb-9763-559a599b1b70.png)

![image](https://user-images.githubusercontent.com/33585301/120170499-a51ed200-c21e-11eb-9a05-bc8855c18d42.png)


![image](https://user-images.githubusercontent.com/33585301/120170323-799be780-c21e-11eb-84ec-17f68029567d.png)



- security group ingress allowing multiple TCP portS  443 ,943 ,945
- security group ingress allowing  UDP  port 1194 which is actually were the connection happens all of this port are describing open vpn documentation
- security group Egress allowing everything to go out from the security 
- IAM role (NodeRole) using the policy down (NodeRolePolicies) which is actually denying everything , we are creating this to not create an ec2 instance without an IAM attached to it which is not a good pratice 
- the instance profile which is at the end which is attached to the ec2 instance 
- Autoscaling group (nodegroup ), with the node launch configuration , b/w these 2 we are actually configuring the autoscaling group to locate our machines into the subnet,vpcs and all the configuration required for the it . 
- userdata specified on the launch configuration 
- openvpn has a program called sacli which allow to set configuration of software itself , setting a key which is parivate network , for specifying the open vpn service that we , parameter we are sending to for the cloudformation template . Used as our target 
- creating the first user with parameters as well
- then we start this .


Ref CF: https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/openvpn/openvpn.yaml
Ref OpenVPN: https://openvpn.net/vpn-server-resources/getting-started/


## GUI Parameters 

![image](https://user-images.githubusercontent.com/33585301/119475782-136d1b80-bd6b-11eb-8772-df9c99727fdc.png)




![image](https://user-images.githubusercontent.com/33585301/119475922-35669e00-bd6b-11eb-8978-35109662f191.png)

## Stack created 

![image](https://user-images.githubusercontent.com/33585301/120177107-abfd1300-c225-11eb-903b-4727b55a3ac0.png)

![image](https://user-images.githubusercontent.com/33585301/120177140-b7e8d500-c225-11eb-8c95-75a1060d7196.png)


____________________________

# OpenVPN along with the security group created

![image](https://user-images.githubusercontent.com/33585301/120177342-f8485300-c225-11eb-97d5-e365e6b59dff.png)

Allow the security group of the open vpn machine for accessing the worker node of our eks cluster 

OpenVPN Security group -> copy the id 

![image](https://user-images.githubusercontent.com/33585301/120172316-86b9d600-c220-11eb-9165-8b29e2f469d8.png)

# worker node security group

open one of the worker nodes 

![image](https://user-images.githubusercontent.com/33585301/120172391-9d602d00-c220-11eb-8f88-b41a3ff462fc.png)




Worker node security group - > edit it -> allow all the triaffic of security grup of openvpn - SAVE

![image](https://user-images.githubusercontent.com/33585301/120177462-1615b800-c226-11eb-9704-d5568c343689.png)





![image](https://user-images.githubusercontent.com/33585301/119476164-7068d180-bd6b-11eb-9ade-c3121b543034.png)



![image](https://user-images.githubusercontent.com/33585301/120177984-9b996800-c226-11eb-92a7-69be6ba0aeef.png)

Now the worker node can accept traffic from the openvpn server which by proxy we are going to add heating thos inputs 

## Client configuration file from openvpn 

![image](https://user-images.githubusercontent.com/33585301/120178215-e1eec700-c226-11eb-9469-8773cb946300.png)


![image](https://user-images.githubusercontent.com/33585301/120178254-efa44c80-c226-11eb-82b9-fd9071f28112.png)

![image](https://user-images.githubusercontent.com/33585301/120178313-03e84980-c227-11eb-89ec-9beabd03298b.png)





![image](https://user-images.githubusercontent.com/33585301/119476345-9aba8f00-bd6b-11eb-8e2b-f09be2b13100.png)




![image](https://user-images.githubusercontent.com/33585301/119476389-a27a3380-bd6b-11eb-820f-697ef0299c4b.png)


![image](https://user-images.githubusercontent.com/33585301/120178822-85d87280-c227-11eb-8c85-1b1f3dd96042.png)



There are multiple ways to connect ,download client file . To connect to openvpn we need a client 

Connect using the configurtion file we got and the credetials 

_______

Create a pod that simply serves http request using apache httpd image in our eks cluster 

![image](https://user-images.githubusercontent.com/33585301/120179077-c637f080-c227-11eb-92c0-a8e9de5f4149.png)


![image](https://user-images.githubusercontent.com/33585301/120179135-d5b73980-c227-11eb-8d9d-722c29813db6.png)

![image](https://user-images.githubusercontent.com/33585301/120179886-c5ec2500-c228-11eb-98ff-545f3b1ff661.png)


HOw this ip is actually in same cidr as our vpc 

No output , as expected we are not able to get it because this is a private ip and we are not in the network 

so let try to Connect up openvpn  client configuration

![image](https://user-images.githubusercontent.com/33585301/120179175-de0f7480-c227-11eb-8d18-01fc86c493d2.png)


![image](https://user-images.githubusercontent.com/33585301/120179264-f4b5cb80-c227-11eb-82ec-0e16a77743b4.png)

![image](https://user-images.githubusercontent.com/33585301/120179325-0ac38c00-c228-11eb-9ee2-db6e7427a219.png)


Now the openvpn setup has worked , and now we can access from internet all our private resource in a secured manner

___________________________

# Accessing resources in a  secondary CIDR 

The idea is to led openvpn completely setup so we dont have to comeback and do that when we introduce thos changes in the CNI plugins .

change is pretty simple : we just have to configure openvpn to route our traffice to second cidr 

Open the ssh port inthe security group of this machine so that we can connect to it 

![image](https://user-images.githubusercontent.com/33585301/120180793-d51fa280-c229-11eb-8232-3b421a56a353.png)

edit inbound rule , another rule with ssh open it anywhere (not recommended) just to make it easy 

![image](https://user-images.githubusercontent.com/33585301/120180936-0304e700-c22a-11eb-93ac-892d1a5fc851.png)

![image](https://user-images.githubusercontent.com/33585301/120181943-59bef080-c22b-11eb-8bb7-5dd676bf63e3.png)

save the rules 

_______________

Come back to instances , openvpn server - connect - copy the example 


![image](https://user-images.githubusercontent.com/33585301/120181033-229c0f80-c22a-11eb-9f70-627483f5e737.png)


In the folder where have keypar which is used to create openvpn server . Copy the instruction to ssh into the machine (change username to openvpnas ) 

![image](https://user-images.githubusercontent.com/33585301/120181257-6f7fe600-c22a-11eb-9292-e913d66356a5.png)


--key already set up in CF template 

--value secondary CIDR 


Very similar to the userdata in the CF template , differnce is that we are setting up differnt CIDR in the second position of the private network we are going to routing our traffic to 

Now we need to restart our servers 

![image](https://user-images.githubusercontent.com/33585301/120181355-8f170e80-c22a-11eb-99c5-6e73d73b0df4.png)


![image](https://user-images.githubusercontent.com/33585301/120181481-c08fda00-c22a-11eb-942e-f6feb4b609f8.png)

![image](https://user-images.githubusercontent.com/33585301/120181567-dbfae500-c22a-11eb-98e2-e757df139bb8.png)


while we are doing this process of restarting openvpn connect will be lost . So probably has to disconnect and  we need to connect it again

With all of these we already are prepared when those CNI changes come to our stack , and then after running all of those command inside openvpn server dont forgot to drop all ssh rules from the security gruop . then save the rule .

![image](https://user-images.githubusercontent.com/33585301/120181620-f339d280-c22a-11eb-93e2-88fd3f3652d2.png)

Drop the pod 

![image](https://user-images.githubusercontent.com/33585301/120183281-1ebdbc80-c22d-11eb-819a-8346bc21a905.png)

Clean up of what we have to created 

go to cloudformation -> openvpn setup -> select the stack -> Delete 


![image](https://user-images.githubusercontent.com/33585301/120183459-575d9600-c22d-11eb-9665-beaab8ceb448.png)



https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/openvpn/openvpn-with-secondary-cidr.yaml



