

[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/installing_the_bookstore_p2.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/demo_cni.md)



# Chapter - 2 
# Networking and EKS

## 13 . CNI and EKS - Integration with VPC

![image](https://user-images.githubusercontent.com/33585301/119602045-f89cb480-be07-11eb-8243-9dbd77d9b7a9.png)

How to customize n/wing around EKS with the help of CNI plugin

___


![image](https://user-images.githubusercontent.com/33585301/119602981-f3406980-be09-11eb-82ba-acfccc626aa0.png)




1) CNI stands for container network interface , which is instructions or guidelines that a container environment needs to have related to the administration of the n/wing .
Maintaining containers in terms of n/w itss one of most important part on its success . so CNI is an standard that defines how a container engines manage there connections.

2) CNI can be implemented in the form of plugins and the standard can be extended according to the needs the environment and the runtime were this is installed .

3) The most famous  container orchestration tooling like Openshift,ECS,rks,k8.....etc  implement CNI in there own way but still being CNI . which means that all of them follows the same strategy the general way and prevent redoing work 


___

![image](https://user-images.githubusercontent.com/33585301/119603299-88436280-be0a-11eb-88c7-e3bcf32c8f92.png)

EKS is k8s , in the k8s world multiple implementation are plugins of CNI are out there .  so eks also has its own one that takes more advantage of the AWS eco system , espically from VPC . 

![image](https://user-images.githubusercontent.com/33585301/119604406-a0b47c80-be0c-11eb-9ed2-0e27b131164f.png)



1) This plugin was developed and is maintaned by AWs . But it is opensource . Contributions from community 

2) CNI plugin used in AWS natively uses vpc, allowing the pods to use ip from the network and be at the same level as other aws resources  that can be allocated in vpc . Gain all the security group benefits all the connections established and so on .

3) The CNI plugin doesnt need to installed into the EKS cluster only upgrade it if new versions come up . This is the out of the feature that can be used right away .


---

Since this is already installed we are now getting benefits from it , and we saw how smooth the n/wing went with the k8S  cluster and other aws services like loadbalancer

But given this CNI knowledge we can now expand & customize our n/w 

---

![image](https://user-images.githubusercontent.com/33585301/119604985-9fd01a80-be0d-11eb-84ec-79fdd1dc60ad.png)


Lets start from VPC that we have created , in here we have multiple subnet 

![image](https://user-images.githubusercontent.com/33585301/119605050-c1c99d00-be0d-11eb-80d9-da1a1c93862d.png)


![image](https://user-images.githubusercontent.com/33585301/119605106-d7d75d80-be0d-11eb-9929-14230cad02e5.png)



![image](https://user-images.githubusercontent.com/33585301/119605137-e291f280-be0d-11eb-8fa3-9bf3d2be8faf.png)


![image](https://user-images.githubusercontent.com/33585301/119605170-efaee180-be0d-11eb-9067-3dd98507b5b9.png)


![image](https://user-images.githubusercontent.com/33585301/119605266-166d1800-be0e-11eb-9f19-34345e4edb0b.png)



![image](https://user-images.githubusercontent.com/33585301/119605199-fb020d00-be0d-11eb-8cb7-bd256c1151ca.png)



![image](https://user-images.githubusercontent.com/33585301/119605342-3e5c7b80-be0e-11eb-980c-688594d16aac.png)



![image](https://user-images.githubusercontent.com/33585301/119605368-4fa58800-be0e-11eb-84a7-809c0dc344f3.png)


![image](https://user-images.githubusercontent.com/33585301/119605405-60ee9480-be0e-11eb-9dd6-d94456965e53.png)



![image](https://user-images.githubusercontent.com/33585301/119605513-8e3b4280-be0e-11eb-98e8-c34e29ce482c.png)

![image](https://user-images.githubusercontent.com/33585301/119605438-6f3cb080-be0e-11eb-93b2-24d9a59d9e5f.png)







![image](https://user-images.githubusercontent.com/33585301/119605467-795eaf00-be0e-11eb-949b-969ff48f5975.png)


