

---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch1/what_you_will_learn.md)



# Chapter - 1
# Introduction

## 1.Welcome to the Course

![image](https://user-images.githubusercontent.com/33585301/119449130-dfd0c800-bd4f-11eb-9f03-53a12c91bea4.png)


Will be working around real micro service based project deployed on top of kubernetes using EKS and multiple other AWS services .
We will start for the scratch from an already developed project and easy to understand new advanced topics .





![image](https://user-images.githubusercontent.com/33585301/119449671-8b7a1800-bd50-11eb-8ce8-061fce882838.png)



______________________

K8s - We will need to know how k8S works and its different components 
EKS stands for Elastic Kubernetes services , it is one of the  container platform on AWS like ECS or ECR  . 

![image](https://user-images.githubusercontent.com/33585301/119450853-26bfbd00-bd52-11eb-88b5-6b6f4fa58253.png)



1) It is basically the control plain of the k8 cluster  we managed by AWS.

2) By default it is also highly available and performance oriented because it starts an minimum of 2 API servers and 3 etcd nodes in different availablityh zone avoiding a  single point of failure 

3) it also scales to reach performance requirments in term of serving the workers nodes 
4) EKS also has service level agreement or SLA which defines they try to be 99.95% uptime , they have a service credit policy 
5) Easy to integrate with other aws services 
6) It is simple to perform upgrades  


__________________



![image](https://user-images.githubusercontent.com/33585301/119451007-5cfd3c80-bd52-11eb-977f-20094f1deae4.png)
