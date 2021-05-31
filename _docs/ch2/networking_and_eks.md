

[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch1/Demo_Creation_of_the_physical_requisites.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/go_private%2Cgo_secure%2Cgo_open_vpn.md)



# Chapter - 2 
# Networking and EKS

## 6. Networking & EKS


![image](https://user-images.githubusercontent.com/33585301/119473206-93de4d00-bd68-11eb-898c-b0181899e3db.png)

1) For development and testing environment we need to make sure our resources are protected and not internet exposed so that the guide on how to access private resources without compromising security with the help of openVPN
2) We are going to be using our  DNS hosted zone (ROUTE53 )  , to prepare the identification of the resources with is 
3) connection from the end user to the app in the secure manner , this is going to be possible because of the ssl certificates we can install on our solution with the help of ACM (AWS Certificate Manager )  
4) Explain import concept of k8s and project it to EKS , its regarding how we set up the n/wing of the pods on the cluster . Will explaing container networking interface (CNI Plugin) 
5) Deploying appliation with the help of helm for the k8s resources , and cloudformation for creating all the aws resources that we need for it .
______________________________________

![image](https://user-images.githubusercontent.com/33585301/119473385-bd977400-bd68-11eb-8c07-04896eecad58.png)

______________________________________

![image](https://user-images.githubusercontent.com/33585301/119473498-d738bb80-bd68-11eb-8f64-3f0e612bac33.png)


______________________________________
