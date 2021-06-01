


[Previous Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/installing_the_bookstore_p1.md)---------------------------------------------------- [Next Page](https://github.com/EtricKombat/Course_Practical_Guide_EKS/blob/master/_docs/ch2/CNI_and_eks_integration_with_VPC.md)



# Chapter - 2 
# Networking and EKS

## 12 .Installing the Bookstore - Part 2


![image](https://user-images.githubusercontent.com/33585301/119611334-2c7fd600-be18-11eb-9932-283b93fc0977.png)


In the previous lecture we configure and instaled everything we required for this solution .

In this one we are going to install the applicatin itself .



![image](https://user-images.githubusercontent.com/33585301/119611519-68b33680-be18-11eb-8be8-01ed382310c5.png)


Pre-requesite for successfully complete for this lab . 


1) eks control plane up and running 
2) worker nodes attached to this eks control  plane which is pod for atlead 6 new pods , in previous lecture we increase instance type of the worker node to support more pods 
3) we use open vpn for this in our stack . 
4) with CF template are in the github repo and we created in our prev chapeter . 
5) external dns installed in our cluster and all the iam permission it need to attach in the worker nodes 
6) same thing for the applicatin load balancer controller . 
7) full dynamodb permissions in our woker nodes , so tha apps can use it to access date in the table 

all of these pre-requestite has been covered  
___________

Helm for installaing micro services  (resource api , client, inventory ).
Below links contains what is helm how it works , how can you install it depending on you OS 

https://helm.sh/

https://helm.sh/docs/


![image](https://user-images.githubusercontent.com/33585301/119611531-71a40800-be18-11eb-9aef-249b9ea025b5.png)


we have 5 folders that match microservices , each of them have an infra folder and inside it helm folder 


![image](https://user-images.githubusercontent.com/33585301/119611629-94362100-be18-11eb-84de-73dab6314ac0.png)


_________

we will be creating 2 things :
 1) deployment itself which is a k8s deployment , which container will be the image of the renting api in this case 



![image](https://user-images.githubusercontent.com/33585301/119611905-e70fd880-be18-11eb-9460-70fcee2daeb3.png)

 2) service for exposing that 

![image](https://user-images.githubusercontent.com/33585301/119611950-f2630400-be18-11eb-9acc-7ad4e0e7d40a.png)

3) if we go to values we are going to find the service type is node port , which is fine for what we are going to be doing next 


![image](https://user-images.githubusercontent.com/33585301/119611980-f98a1200-be18-11eb-90cf-5f03490b0182.png)

this is simple helm chart , we will be exactly the same for resource api , inventory api, and clients api 

front end helm chart is little bit differnt 
_____________

Helm for installaing micro services  (front end )

![image](https://user-images.githubusercontent.com/33585301/119612277-5259aa80-be19-11eb-953d-e07050a4efbd.png)

deployment with the continer of the front end 

![image](https://user-images.githubusercontent.com/33585301/119612296-584f8b80-be19-11eb-8854-11b2414a5859.png)

and its service 



![image](https://user-images.githubusercontent.com/33585301/119612083-1cb4c180-be19-11eb-9f92-3192e9cdec55.png)


we have deployment of the proxy that we connect the front end with all the APIs 

![image](https://user-images.githubusercontent.com/33585301/119612328-61d8f380-be19-11eb-933a-c9f4df9da0d4.png)

and its services , 



![image](https://user-images.githubusercontent.com/33585301/119612370-70270f80-be19-11eb-958f-ab86f9923282.png)

since thise proxy ngix running behind the scenes we are customizing itz configuration , we put all oof that in a config map . this is an nginx configuration for allowing the proxy to redirect all the request to the api in the back end from the front end 


![image](https://user-images.githubusercontent.com/33585301/119612514-9a78cd00-be19-11eb-99f8-b103d4990593.png)


and also we have secret.yml which presents one value in the data  , this config the json file which is actually get & replaced using the gold template functionality that helm  provides . Is basically  json file actaully front end applicatin requires for getting all the endpoint of our application .
Lets take a look at this file because it is important .




![image](https://user-images.githubusercontent.com/33585301/119612540-a49acb80-be19-11eb-84aa-dc3be6efc45e.png)


Json file with some gold template tags in here . we are going to look at this on in a second .

As you can see this is a config file that has the url of the resource api , inventory, client, renting api that are exposed through the proxy .

So it means that the front end will request this file and from it get the location of where the apis should be .

so given this background you ll need to go to the values.yaml file in the front end helm chart 

![image](https://user-images.githubusercontent.com/33585301/119612852-052a0880-be1a-11eb-8c92-12f3444465e7.png)

In here the api: url  , this is the url of the proxy 


________________


![image](https://user-images.githubusercontent.com/33585301/119612933-1ecb5000-be1a-11eb-9266-015ebbce80d8.png)


![image](https://user-images.githubusercontent.com/33585301/119612966-28ed4e80-be1a-11eb-9889-b4ec971be45c.png)



![image](https://user-images.githubusercontent.com/33585301/119613968-5be41200-be1b-11eb-9400-582c893ffa35.png)


![image](https://user-images.githubusercontent.com/33585301/119613051-3d314b80-be1a-11eb-9b15-e2a021faa87f.png)

____________________________________


![image](https://user-images.githubusercontent.com/33585301/119614111-86ce6600-be1b-11eb-868b-32ac3b9c3836.png)



![image](https://user-images.githubusercontent.com/33585301/119614278-b5e4d780-be1b-11eb-9eb9-ba49041f9a76.png)


![image](https://user-images.githubusercontent.com/33585301/119614305-c006d600-be1b-11eb-9aaa-890a5f941889.png)



![image](https://user-images.githubusercontent.com/33585301/119614588-1411ba80-be1c-11eb-825e-e52b98fda192.png)


![image](https://user-images.githubusercontent.com/33585301/119615065-9b5f2e00-be1c-11eb-80ca-cc51676b827d.png)


![image](https://user-images.githubusercontent.com/33585301/119614675-36a3d380-be1c-11eb-8963-17701f7ebff4.png)



![image](https://user-images.githubusercontent.com/33585301/119615234-ce092680-be1c-11eb-99f6-c9866b214604.png)


![image](https://user-images.githubusercontent.com/33585301/119615307-dfeac980-be1c-11eb-924c-1f974e807d09.png)


![image](https://user-images.githubusercontent.com/33585301/119616848-bc288300-be1e-11eb-80e2-6fe1d13c9ac3.png)


![image](https://user-images.githubusercontent.com/33585301/119616993-eed27b80-be1e-11eb-81d5-07801e0d04ee.png)

![image](https://user-images.githubusercontent.com/33585301/119617126-1aedfc80-be1f-11eb-9f2e-932ed2263f5b.png)




![image](https://user-images.githubusercontent.com/33585301/119617192-2d683600-be1f-11eb-9961-8684b79ff4d8.png)
![image](https://user-images.githubusercontent.com/33585301/119617345-61dbf200-be1f-11eb-8a7d-6eb70c251b59.png)


![image](https://user-images.githubusercontent.com/33585301/119617227-3a852500-be1f-11eb-8746-063a629f23ba.png)
![image](https://user-images.githubusercontent.com/33585301/119617421-76b88580-be1f-11eb-9aae-9e8549430ab8.png)



![image](https://user-images.githubusercontent.com/33585301/119617492-8a63ec00-be1f-11eb-90b8-a7c172db08c8.png)



![image](https://user-images.githubusercontent.com/33585301/119617527-918afa00-be1f-11eb-95c8-a70adf6569b4.png)



![image](https://user-images.githubusercontent.com/33585301/119617633-a8315100-be1f-11eb-9f2c-c4a1f42f620e.png)


![image](https://user-images.githubusercontent.com/33585301/119617682-b2ebe600-be1f-11eb-9384-bbbcf53057c7.png)

