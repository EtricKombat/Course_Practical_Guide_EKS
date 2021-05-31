


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
- VPCID we created with eksctl 
- vpc cidr 
- allowed ip range which is basically to who we are giving access to the machine as you can see by default simply letting everybody to connect to this one (we can be more strict depending on the company policy ) 
- preconfigured openvpn machine  with  username & a password are the initial one we are going to use for accesing this machine 

## Resources 

![image](https://user-images.githubusercontent.com/33585301/120168407-70118000-c21c-11eb-8d9e-28f550bb7624.png)
![image](https://user-images.githubusercontent.com/33585301/120168946-12316800-c21d-11eb-8f4e-e41edaa5e090.png)



- security group  allowing multiple TCP portS  443 ,943 ,945
- security group  allowing  UDP  port 1194 which is actually were the connection happens all of this port are describing open vpn documentation



Ref CF: https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/openvpn/openvpn.yaml
Ref OpenVPN: https://openvpn.net/vpn-server-resources/getting-started/


## GUI Parameters 

![image](https://user-images.githubusercontent.com/33585301/119475782-136d1b80-bd6b-11eb-8772-df9c99727fdc.png)




![image](https://user-images.githubusercontent.com/33585301/119475922-35669e00-bd6b-11eb-8978-35109662f191.png)


____________________________

worker node security group

![image](https://user-images.githubusercontent.com/33585301/119476164-7068d180-bd6b-11eb-9ade-c3121b543034.png)


____________________________


![image](https://user-images.githubusercontent.com/33585301/119476345-9aba8f00-bd6b-11eb-8e2b-f09be2b13100.png)


____________________________

![image](https://user-images.githubusercontent.com/33585301/119476389-a27a3380-bd6b-11eb-820f-697ef0299c4b.png)


https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/openvpn/openvpn-with-secondary-cidr.yaml



