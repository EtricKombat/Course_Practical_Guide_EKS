


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

![image](https://user-images.githubusercontent.com/33585301/119494770-4b7e5980-bd7f-11eb-8e2c-b8c8f42022ff.png)


![image](https://user-images.githubusercontent.com/33585301/119494796-50dba400-bd7f-11eb-8430-d9ab5c81e449.png)


![image](https://user-images.githubusercontent.com/33585301/119494845-605aed00-bd7f-11eb-9569-7c457658946f.png)

![image](https://user-images.githubusercontent.com/33585301/119618172-3574a580-be20-11eb-9d90-306283a57aa9.png)


![image](https://user-images.githubusercontent.com/33585301/119494877-694bbe80-bd7f-11eb-90df-77eb6dfbb769.png)


![image](https://user-images.githubusercontent.com/33585301/119494893-6ea90900-bd7f-11eb-9fe1-1720bfd2c89e.png)


![image](https://user-images.githubusercontent.com/33585301/119494919-75d01700-bd7f-11eb-823a-6694670d4c16.png)


_________________________


![image](https://user-images.githubusercontent.com/33585301/119618273-563cfb00-be20-11eb-94a0-0a11b7e12028.png)


![image](https://user-images.githubusercontent.com/33585301/119618406-7967aa80-be20-11eb-944d-6776415afc64.png)


https://github.com/kubernetes-sigs/external-dns

![image](https://user-images.githubusercontent.com/33585301/119618500-98663c80-be20-11eb-998d-3c1c05953162.png)

![image](https://user-images.githubusercontent.com/33585301/119618445-88e6f380-be20-11eb-9e7f-81385316f4ef.png)


![image](https://user-images.githubusercontent.com/33585301/119618557-a6b45880-be20-11eb-814d-c18f33e8e9eb.png)


![image](https://user-images.githubusercontent.com/33585301/119618609-b469de00-be20-11eb-971e-81edb0583044.png)


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



