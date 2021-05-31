


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
- for supporting the root we have 'subjectalternativename' 
- we set some tags 
- important is 'validationMethod"


____________________




![image](https://user-images.githubusercontent.com/33585301/120186437-22ebd900-c231-11eb-99b3-925e7d0fbbd5.png)

![image](https://user-images.githubusercontent.com/33585301/120186479-2ed79b00-c231-11eb-8597-7074d34acb22.png)




![image](https://user-images.githubusercontent.com/33585301/120186914-c1783a00-c231-11eb-813b-452075d50a22.png)


![image](https://user-images.githubusercontent.com/33585301/120187020-e8367080-c231-11eb-80f3-fd5c806c602b.png)


![image](https://user-images.githubusercontent.com/33585301/120187105-000df480-c232-11eb-8717-a389acadbcb7.png)


![image](https://user-images.githubusercontent.com/33585301/120187137-0c924d00-c232-11eb-8a1c-583a3f9e3be5.png)


![image](https://user-images.githubusercontent.com/33585301/120187264-364b7400-c232-11eb-994a-294d3f505f06.png)


Ref : 

https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/CreatingNewSubdomain.html

https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html
