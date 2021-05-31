


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
__________


Last one is not recommended , going to use this one so that have quick understanding how this will work but eventually in the 3rd chapter we are going to correct this and we are actually going to expand this towards the rest of our application 

![image](https://user-images.githubusercontent.com/33585301/119618609-b469de00-be20-11eb-971e-81edb0583044.png)




________________-

![image](https://user-images.githubusercontent.com/33585301/119618630-bc298280-be20-11eb-8ac7-a9207927010e.png)


https://github.com/kubernetes-sigs/external-dns/blob/master/docs/tutorials/aws.md





https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/k8s-tooling/1-external-dns/01-initial-external-dns.yaml

https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/Infrastructure/k8s-tooling/1-external-dns/02-testing-external-dns.yaml





![image](https://user-images.githubusercontent.com/33585301/119618773-e5e2a980-be20-11eb-92bc-54a52c1ffce1.png)


![image](https://user-images.githubusercontent.com/33585301/119618789-ec712100-be20-11eb-939b-3dd047fba80e.png)


![image](https://user-images.githubusercontent.com/33585301/119618864-00b51e00-be21-11eb-8db0-3d06450aa45a.png)


![image](https://user-images.githubusercontent.com/33585301/119618993-2a6e4500-be21-11eb-802c-3f1579d06819.png)



![image](https://user-images.githubusercontent.com/33585301/119618899-0e6aa380-be21-11eb-8e71-e1412139decf.png)

![image](https://user-images.githubusercontent.com/33585301/119619074-3f4ad880-be21-11eb-8f98-749b1e565336.png)




_________


![image](https://user-images.githubusercontent.com/33585301/119619246-6a352c80-be21-11eb-930f-52b421955d6c.png)


![image](https://user-images.githubusercontent.com/33585301/119619267-702b0d80-be21-11eb-9b8b-5b0c8395ce10.png)


![image](https://user-images.githubusercontent.com/33585301/119619323-7de09300-be21-11eb-885a-e04aa9995229.png)

![image](https://user-images.githubusercontent.com/33585301/119619347-83d67400-be21-11eb-959f-dc3d7fcbadd6.png)



