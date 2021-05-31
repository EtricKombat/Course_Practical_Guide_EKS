

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

 

It is very import to find a guilde that defines good solution - <a href="https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf">Well Architected framework </a>is best for it 

### Security piller 

1) Introduction of OpenVPN is crucial in here. All pods are in private subnet and we will be able to access them from internal n/w 
2) Given the role of the user log into openvpn they can have access or not to the private resources 
3) ACM will provide the SSL certificate to securily terminate communiccation with the application and the ed user 


______________________________________

![image](https://user-images.githubusercontent.com/33585301/119473498-d738bb80-bd68-11eb-8f64-3f0e612bac33.png)


### Reliability piller 

1) with the CNI  to EKS the idea is to keep the solution as simple as possible , since the pods are going to be in the same layer of the resources on the VPC 
2)  The maximum number of pods that we can have on eks is depends on the type of the ec2 instance so taking more advantange of CNI will help us keep the limit under control 

______________________________________
