


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

OpenVPN cloudformation


https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/openvpn/openvpn.yaml

![image](https://user-images.githubusercontent.com/33585301/119475782-136d1b80-bd6b-11eb-8772-df9c99727fdc.png)


____________________________

![image](https://user-images.githubusercontent.com/33585301/119475922-35669e00-bd6b-11eb-8978-35109662f191.png)


____________________________

worker node security group

![image](https://user-images.githubusercontent.com/33585301/119476164-7068d180-bd6b-11eb-9ade-c3121b543034.png)


____________________________


![image](https://user-images.githubusercontent.com/33585301/119476345-9aba8f00-bd6b-11eb-8e2b-f09be2b13100.png)


____________________________

![image](https://user-images.githubusercontent.com/33585301/119476389-a27a3380-bd6b-11eb-820f-697ef0299c4b.png)


https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/openvpn/openvpn-with-secondary-cidr.yaml



