


[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/networking_and_eks.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/dns_do_not_suffer.md)





# Chapter - 2 
# Networking and EKS

## 7. Go Private , Go Secure , Go OpenVPN

![image](https://user-images.githubusercontent.com/33585301/119473607-f5062080-bd68-11eb-997a-2f52f54293c7.png)

It is an optional lecture , it the resource is hosted on public subnet this is not required 
_____________________________

![image](https://user-images.githubusercontent.com/33585301/119473643-fd5e5b80-bd68-11eb-83bc-c3f78df48662.png)

This software will allow us to use our trrafic internally in our 
_____________________________

![image](https://user-images.githubusercontent.com/33585301/119473791-1ff07480-bd69-11eb-8609-1c0a3623362d.png)

_____________________________

![image](https://user-images.githubusercontent.com/33585301/119473814-27178280-bd69-11eb-8c97-6c5dd846b5ab.png)
_____________________________

![image](https://user-images.githubusercontent.com/33585301/119473839-2c74cd00-bd69-11eb-9f18-55fe32328705.png)

_____________________________

![image](https://user-images.githubusercontent.com/33585301/119473899-40203380-bd69-11eb-9902-07af16f87f0f.png)

_____________________________

![image](https://user-images.githubusercontent.com/33585301/119474005-5a5a1180-bd69-11eb-9151-076a54e929a9.png)

_____________________________

![image](https://user-images.githubusercontent.com/33585301/119474046-647c1000-bd69-11eb-91e4-34b844886887.png)

_____________________________

![image](https://user-images.githubusercontent.com/33585301/119474122-7493ef80-bd69-11eb-8399-a777538cdf67.png)

_____________________________

![image](https://user-images.githubusercontent.com/33585301/119474679-0a2f7f00-bd6a-11eb-99bd-3c822bee2c3c.png)

____________________________

OpenVPN cloudformation


https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/openvpn/openvpn.yaml

![image](https://user-images.githubusercontent.com/33585301/119475782-136d1b80-bd6b-11eb-8772-df9c99727fdc.png)

![image](https://user-images.githubusercontent.com/33585301/119475922-35669e00-bd6b-11eb-8978-35109662f191.png)


worker node security group

![image](https://user-images.githubusercontent.com/33585301/119476164-7068d180-bd6b-11eb-9ade-c3121b543034.png)


![image](https://user-images.githubusercontent.com/33585301/119476345-9aba8f00-bd6b-11eb-8e2b-f09be2b13100.png)


![image](https://user-images.githubusercontent.com/33585301/119476389-a27a3380-bd6b-11eb-820f-697ef0299c4b.png)


https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/openvpn/openvpn-with-secondary-cidr.yaml
