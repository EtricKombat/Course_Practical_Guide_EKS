


[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/dns_do_not_suffer.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/external_dns.md)



# Chapter - 2 
# Networking and EKS

## 9.Demo : ACM


https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/dns/dns.json




![image](https://user-images.githubusercontent.com/33585301/120185955-7a3d7980-c230-11eb-9ae8-75a473a71bf4.png)

![image](https://user-images.githubusercontent.com/33585301/120186335-fe8ffc80-c230-11eb-8715-64e1a708ba0b.png)


![image](https://user-images.githubusercontent.com/33585301/120186158-bf61ab80-c230-11eb-901d-e89f1e6ad1a1.png)


This is the cloudformation template that we are going to using for our ACM certificate 

The tutor has purchased DNS domain for route 53 , which by default created a hosted zone for him . 
so he has created a parameter for "deployedhostedzone"  , in his case he wont use it since he already have a hosted zone 

if We dont have a hosted zone and actually have dns domain in another provider ,different route 53 (refer below link to know how to achieve this )
Highly recommend to select 'yes' when creating the stack 

- recieve the domain name we are going to be using 
- and environment where dev or prod 
- condtion - if deployed hosted zone is 'yes' , then simply deploy hosted zone resource or else don't do it 
- certificate - which domain name is (*.${DomainName} ) we have specified on the parameter 
- also for supporting the root we have 'subjectalternativename' 
- we set some tags 
- important is 'validationMethod" , could be email or dns . We use DNS here . It is important to do that clarificatin because this will require an extra step for validating it 


____________________




 ## Creating stack 

https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/cloudformation/dns/dns.json

![image](https://user-images.githubusercontent.com/33585301/120186437-22ebd900-c231-11eb-99b3-925e7d0fbbd5.png)




![image](https://user-images.githubusercontent.com/33585301/120191713-e8396f00-c237-11eb-9b2b-3c6b66fc35ea.png)

Keep inmind something , in the resources section we are going to create this certificate



![image](https://user-images.githubusercontent.com/33585301/120191851-1323c300-c238-11eb-8bdc-02e64193835a.png)


A C-Name we need to add into our hosted zone , Need to put all of the "Content of DNS Record "  dns table in the hosted zone 

Go to route 53 on the hosted zone create a new CName with all these values 


________________

Easest way is to go to Certificate manager service and we are going to find certificate with pending validateion  

![image](https://user-images.githubusercontent.com/33585301/120186914-c1783a00-c231-11eb-813b-452075d50a22.png)

Lets open this app , as you can see validateion not complete . We had set up on our CF Template validation method as DNS 

![image](https://user-images.githubusercontent.com/33585301/120187020-e8367080-c231-11eb-80f3-fd5c806c602b.png)


This is the extra step , go to domain (one of these 2 , doesn't matter ) , it can show you the Cname and the value it is going to add on route 53 as a record on our hosted zone . If click on the "create record in route 53 " button and then create it is going to add the record for us on the hosted zone

![image](https://user-images.githubusercontent.com/33585301/120193075-998cd480-c239-11eb-98cd-ae52317d3cd7.png)




![image](https://user-images.githubusercontent.com/33585301/120187105-000df480-c232-11eb-8717-a389acadbcb7.png)



So from here we need to wait until ACM validates that actaully that domain belong to us 

___________________

Back to cloudformation -> stacks -> development dns 

![image](https://user-images.githubusercontent.com/33585301/120193859-87f7fc80-c23a-11eb-928e-667dfd370a22.png)



Lets waits until this one comes complete 


![image](https://user-images.githubusercontent.com/33585301/120187137-0c924d00-c232-11eb-8a1c-583a3f9e3be5.png)

Completed ! 

__________

Come back to certificate manager service again , certificate is already issued 

![image](https://user-images.githubusercontent.com/33585301/120194000-b1b12380-c23a-11eb-8e78-bbedd4d2a716.png)


If we open this up , we will find an ARN . This is actually what  k8s is going to be using when we deploy our application 


IN next session we are going to deploy external dns in our k8s cluster 


![image](https://user-images.githubusercontent.com/33585301/120187264-364b7400-c232-11eb-994a-294d3f505f06.png)





Ref : 

https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/CreatingNewSubdomain.html

https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html
