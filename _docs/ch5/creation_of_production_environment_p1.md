
# Creation of production environment part 1 


![image](https://user-images.githubusercontent.com/33585301/119318371-d7688680-bc96-11eb-915e-002b8c3406ad.png)

1 ) Prepare the DNS & create SSL certificate using ACM , all of this is for presenting and exposing application . In the developement environment we have the DNS that is specific for that environment . But in this one we want it to be differnet . Also the certificate managed by ACM will help us maintain secure connection between the end user and all infastructure .

2 ) We have to create production K8 namespace were all of our micro service is going to be lived 

3 ) We have the creation of our Dynamodb table for the production environment which are required for each micro service for which we are going to using cloudformation  

4 ) create iam role for service account to allow the micro service to consume dynamodb tabls of production environment , we are going to be using 'eksctl' for this one & it will be using cloudformation behind the scenes.

5 ) Then we are going to install micro services helm charts for the initial kickoff of the environments

6) create the production central ingress that is taking by the application load balancer controller and will create all the asset target groups load balancer itself also allowed the external DNS application to create the records on our hosted zone.


_______________________

## Lab -1 

![image](https://user-images.githubusercontent.com/33585301/119318482-01ba4400-bc97-11eb-816a-c81a6e1e6f5e.png)

## Lab 2 


__________________________

Step 1 

![image](https://user-images.githubusercontent.com/33585301/119318837-637aae00-bc97-11eb-8d88-c501f841ce75.png)

With our k8 cluster 1st step is for  assuring or creating hosted zone ( or route53) and the SSL certificate using ACM

____________________



Step 2

![image](https://user-images.githubusercontent.com/33585301/119318929-7d1bf580-bc97-11eb-9c06-e9a3e49e246f.png)

We are going to create prod namespace 


Step 3 

![image](https://user-images.githubusercontent.com/33585301/119319114-accafd80-bc97-11eb-8959-b77b90e3b51d.png)

we are going to execute the cloudformation that is going to create each of the dynamodb tables of each of api s  

Step 4 

![image](https://user-images.githubusercontent.com/33585301/119319239-cd935300-bc97-11eb-94b5-e0dd2054e7ef.png)

We are going to create IAM roles for service account assets which are all the iam assets and the servce account ready for start recieving our application on k8 
