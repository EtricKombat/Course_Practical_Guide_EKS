

[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/installing_the_bookstore_p2.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/demo_cni.md)



# Chapter - 2 
# Networking and EKS

## CNI and EKS - Integration with VPC

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



Lets 
