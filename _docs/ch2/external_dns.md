


[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/demo_acm.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/installing_the_bookstore_p1.md)



# Chapter - 2 
# Networking and EKS

## 10. External DNS

We are going to be reviewing  the architecture that will make this tool shine in our solution .




Lets start with our k8s cluster which is already up and running 

![image](https://user-images.githubusercontent.com/33585301/119494640-27bb1380-bd7f-11eb-8d3a-916e274b22e3.png)

____________________

The way external dns works is  , we have an application running on a pod and with a service in front 

![image](https://user-images.githubusercontent.com/33585301/119494680-34d80280-bd7f-11eb-9ab6-e0069f2b24ec.png)

____________________

In route53 we have a hosted zone that we showed in the previous lab and that will manage all the DNS entries for the solution 

![image](https://user-images.githubusercontent.com/33585301/119494770-4b7e5980-bd7f-11eb-8e2c-b8c8f42022ff.png)

____________________

Then we install external DNS in our cluster , 

![image](https://user-images.githubusercontent.com/33585301/119494796-50dba400-bd7f-11eb-8430-d9ab5c81e449.png)

____________________

which is going to be watching at services with a specific annotation that has the domain name we want to use for the app

![image](https://user-images.githubusercontent.com/33585301/119494845-605aed00-bd7f-11eb-9569-7c457658946f.png)




____________________

![image](https://user-images.githubusercontent.com/33585301/119618172-3574a580-be20-11eb-9d90-306283a57aa9.png)

External DNS will find the service with the annotation and create an A record to it , if it is a load balancer type service it will create an alias , pointing towards the app  can found .
____________________

![image](https://user-images.githubusercontent.com/33585301/119494877-694bbe80-bd7f-11eb-90df-77eb6dfbb769.png)

This don't only work with 'services' but also with 'ingress' , so you can deploy your fav ingress controller like nginx controller or ALB controller and use them .

____________________

![image](https://user-images.githubusercontent.com/33585301/119494893-6ea90900-bd7f-11eb-9fe1-1720bfd2c89e.png)

For these type of scenerios we will have to use our ingress to locate the service and the external dns annotation needs to leave in the ingress instead .

____________________

![image](https://user-images.githubusercontent.com/33585301/119494919-75d01700-bd7f-11eb-823a-6694670d4c16.png)

Remember for allowing external dns to perform all the AWS api methods to manage route 53 is important to assign correct permissions where it is running .
This is important in the next chapter we will check the one of the ways to securely do this with out giving the whole cluster permissions to it .
For this one we will approach it in that way.

_________________________


![image](https://user-images.githubusercontent.com/33585301/119618273-563cfb00-be20-11eb-94a0-0a11b7e12028.png)




Lets get pratical and start installing everything into our cluster . This is the github site of external dns . It is widely used in the cloud & k8s environments .



![image](https://user-images.githubusercontent.com/33585301/119618406-7967aa80-be20-11eb-944d-6776415afc64.png)

So it is highly recommeded to use this especially for the services that are listed (stable )

![image](https://user-images.githubusercontent.com/33585301/120197532-acee6e80-c23e-11eb-9642-9a8dba580a05.png)

LInk : https://github.com/kubernetes-sigs/external-dns

________________________

## Index of how to install and point them to  all the different DNS services that are external DNS users 



![image](https://user-images.githubusercontent.com/33585301/119618445-88e6f380-be20-11eb-9e7f-81385316f4ef.png)


Under AWS we have 

![image](https://user-images.githubusercontent.com/33585301/119618500-98663c80-be20-11eb-998d-3c1c05953162.png)

1) Lets focus on Route53 first , (from prev lecture) we need these set of persmission so that the container that is running external DNS can actually perform AWS api calls to modify or  host a zone 

____________

![image](https://user-images.githubusercontent.com/33585301/119618557-a6b45880-be20-11eb-814d-c18f33e8e9eb.png)

2) They suggested us to create an iam role using the policy and there are multiple ways of doing this using  kiam,kube2iam , 




![image](https://user-images.githubusercontent.com/33585301/119618609-b469de00-be20-11eb-971e-81edb0583044.png)




________________

Last one is not recommended , going to use this one so that have quick understanding how this will work but eventually in the 3rd chapter we are going to correct this and we are actually going to expand this towards the rest of our application 

![image](https://user-images.githubusercontent.com/33585301/119618630-bc298280-be20-11eb-8ac7-a9207927010e.png)




The 3rd solution they provide simply gives permission for managing that hosted zone to the instance role that is running worker nodes 





______________

## Management console 

### CloudFormation create stack



![image](https://user-images.githubusercontent.com/33585301/119618773-e5e2a980-be20-11eb-92bc-54a52c1ffce1.png)


![image](https://user-images.githubusercontent.com/33585301/119618789-ec712100-be20-11eb-939b-3dd047fba80e.png)


![image](https://user-images.githubusercontent.com/33585301/119618864-00b51e00-be21-11eb-8db0-3d06450aa45a.png)


____________________

## Running worker nodes



![image](https://user-images.githubusercontent.com/33585301/119618993-2a6e4500-be21-11eb-802c-3f1579d06819.png)


Attach policy -> look for external DNS (CLOUDFORMATION has created ) 

![image](https://user-images.githubusercontent.com/33585301/119618899-0e6aa380-be21-11eb-8e71-e1412139decf.png)

![image](https://user-images.githubusercontent.com/33585301/119619074-3f4ad880-be21-11eb-8f98-749b1e565336.png)


The policy that external dns is suggesting for us to create 


_________


Back at external dns doc -> Manifest file ( for cluster with RBAC enabled ) to install this application into our cluster 



![image](https://user-images.githubusercontent.com/33585301/119619246-6a352c80-be21-11eb-930f-52b421955d6c.png)

It will create the following 

![image](https://user-images.githubusercontent.com/33585301/119619267-702b0d80-be21-11eb-9b8b-5b0c8395ce10.png)

and also with latest image of external dns application


![image](https://user-images.githubusercontent.com/33585301/119619323-7de09300-be21-11eb-885a-e04aa9995229.png)

comes with these arguments 

![image](https://user-images.githubusercontent.com/33585301/119619347-83d67400-be21-11eb-959f-dc3d7fcbadd6.png)


-if the source is a service and or ingress 
-apply domain filter like everything on this subdomain 'external-dns-test.my-org.com' will be taken by external dns as a subject 
- aws provider
- upsert-only ,  it wont delete whenever remove a  service 
- zone type public or private 
- 

Something important to take a look in here  is this annotation is in 'iam.amazonaws.com/role' this is  only in the case of kiam or k2iam 
in our case we are not using any of them and we wont (we are going to get rid of that annotation for now)  

![image](https://user-images.githubusercontent.com/33585301/120268995-8f63e800-c2c4-11eb-81bd-67b56637d204.png)

________________


![image](https://user-images.githubusercontent.com/33585301/120269285-2335b400-c2c5-11eb-8779-73a74fddffcc.png)


We are gonna find a yaml file with a service type load balancer 

![image](https://user-images.githubusercontent.com/33585301/120269242-0ef1b700-c2c5-11eb-9f3b-694171529dcf.png)

and external-dns.alpha.kubernetes.io/hostname which is holding the dns of the subdomain of our application so it is important and we have change that from our side 


![image](https://user-images.githubusercontent.com/33585301/120269222-04cfb880-c2c5-11eb-9eaf-bbd75f8645ad.png)



and a deployment of a simple nginx docker that ofcourse will display the splash screen off , we are going to use this one off for testing our installation 

![image](https://user-images.githubusercontent.com/33585301/120269556-ace58180-c2c5-11eb-8cee-aee0e767d3a4.png)



______

# in terminal 

![image](https://user-images.githubusercontent.com/33585301/120269675-e28a6a80-c2c5-11eb-9116-0dcd1d205359.png)

1) installing all the components  , cluster role , service account , cluster role binding and the deployment of the application itself 
2) is the deployment of the simple nginx site & its service with load balancer type 

![image](https://user-images.githubusercontent.com/33585301/120270297-ff736d80-c2c6-11eb-8733-7cf8317af782.png)



so before continue we need to change the dns that the testing external dns file has on it so you will match the hosted zone on our account 
Go to the code 


![image](https://user-images.githubusercontent.com/33585301/120270331-10bc7a00-c2c7-11eb-851e-2d2b063e99c2.png)

under the metadata section we have the annotation for external-dns.alpha.kuberntes.io/hostname

so we need to put subdomain that falls into the hosted zone that is in our account 

![image](https://user-images.githubusercontent.com/33585301/120270477-511bf800-c2c7-11eb-87b5-3055723b1077.png)


the 'dev.mariomerco.com' in the authorz case is been managed by the hosted zone in his account .

make sure what is the subdomain or domain that you have in your hosted zone , and make sure it annotations curresponds to it .


## back to terminal 

Lets create the first one , lets look at what we have in our pods . Here there is external DNS already running .
Lets try to see what the logs say about this one . The latest logs is watching towards our events 

![image](https://user-images.githubusercontent.com/33585301/120273145-bc67c900-c2cb-11eb-8cf1-1ec534b4435f.png)



![image](https://user-images.githubusercontent.com/33585301/120273342-005ace00-c2cc-11eb-8a55-1d4e0279eb45.png)

## aws console load balancer 

![image](https://user-images.githubusercontent.com/33585301/120273509-37c97a80-c2cc-11eb-89eb-6508decfe54f.png)


classic load balancer internet facing , this is setup in this way because the type of load balancer specified is the default one
so everything is going to be default in here. 

![image](https://user-images.githubusercontent.com/33585301/120273536-41eb7900-c2cc-11eb-8958-7390b14ad85a.png)

In instances section we can see that everthing now is in service (if not in service that mean the instances are still registering to load balancers )


____

in terminal 

lets check again the loads of external dns pods ( created the A record & TXT record 


![image](https://user-images.githubusercontent.com/33585301/120273587-5596df80-c2cc-11eb-98c0-b766bf3b4cbe.png)

_____


Review the 2nd yaml file created . In the annotation (external-dns-alpha.kubernetes.io/hostname) specified the dns we wanted (testing-external-dns.dev.mariomerco.com) 
as we could already see in the logs it is already created .


![image](https://user-images.githubusercontent.com/33585301/120273615-60517480-c2cc-11eb-917f-de9eb2df3ae8.png)



here it is ... it means it is working . We dont even have to look at the route53 table 

![image](https://user-images.githubusercontent.com/33585301/120273634-65aebf00-c2cc-11eb-93a8-f0bfda4e710e.png)



___________________

Only missing one thing ... the ACM certificated in the previous lab for accessing these page through the https protocol because as you can see 
above we are not using secure connection 

![image](https://user-images.githubusercontent.com/33585301/120273773-90991300-c2cc-11eb-997e-8ac9d97c0dbb.png)


certificate we created in the previous lectures and grab the ARN , we are gonna need it because we have to specify 3 more annotations and another port for the deployments
, lets go to the code again add those

1) arn for the cert
2) http , because our application is running on http
3) name of the ports for ssl certificating and end points , down in loadbalancer have to create another port (443 ) which name is https which should align , and the target port is same as the container down

![image](https://user-images.githubusercontent.com/33585301/120273944-cb02b000-c2cc-11eb-9c08-7a7ef1e7a165.png)

![image](https://user-images.githubusercontent.com/33585301/120297124-6b190300-c2e6-11eb-8bfe-2fd5173eae71.png)


__________

The deployment hasn't changed but the service did

![image](https://user-images.githubusercontent.com/33585301/120274030-e79ee800-c2cc-11eb-984e-e531f84cf494.png)

____________

aws management console - > ec2-> load balancer 

![image](https://user-images.githubusercontent.com/33585301/120274166-14eb9600-c2cd-11eb-9ce4-bc79a9fbb42b.png)

if we go to listners , we finds that there is another one called https listening to the port 443 and it has an ssl certificate 

_________

which means that if we go to dns once again , and put https in front of it . Now we are navigating through our application using ssl 

![image](https://user-images.githubusercontent.com/33585301/120274247-2f257400-c2cd-11eb-90b4-74bf4081c2d5.png)


so all of these are pointing that our external dns is working correctly and we could verify that our certificate we install previously also is covering the dns that we want
again without even have to open route53 for doing anytrhing it is no longer required unless you need to do something else for a very specific scenerio 

![image](https://user-images.githubusercontent.com/33585301/120274263-364c8200-c2cd-11eb-8b4c-0dae747a04c6.png)
 

Now we have everything set up before we start installing bookstore applilcation into our k8 cluster 

______________

before moving to next cleanup 

![image](https://user-images.githubusercontent.com/33585301/120298633-e4fdbc00-c2e7-11eb-9c4c-a9d9a01de23d.png)


_____________________

Reference link



https://github.com/kubernetes-sigs/external-dns/blob/master/docs/tutorials/aws.md



https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/k8s-tooling/1-external-dns/01-initial-external-dns.yaml

https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/k8s-tooling/1-external-dns/02-testing-external-dns.yaml





