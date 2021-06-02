


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

In here the api: url  , this is the url of the proxy (where this was configured ? ) . 

Change the DNS and point it to DNS that we are managing with our hosted zone


________________

This helm chart will deploy  the ingress with all the end points that the application  load balance controller will require , for exposing API and proxy and our front end 

![image](https://user-images.githubusercontent.com/33585301/119612933-1ecb5000-be1a-11eb-9266-015ebbce80d8.png)


____________________


![image](https://user-images.githubusercontent.com/33585301/119612966-28ed4e80-be1a-11eb-9889-b4ec971be45c.png)



![image](https://user-images.githubusercontent.com/33585301/119613968-5be41200-be1b-11eb-9400-582c893ffa35.png)

in the values.development.yml and change all the hostnames according to the hosted zone that you have , for example instead of 'dev.mariomerco.com'  we need to put our own 


![image](https://user-images.githubusercontent.com/33585301/119613051-3d314b80-be1a-11eb-9b15-e2a021faa87f.png)

down here we have the proxy , this one should match  the one you set in the value.yml file for the front end helm chart 

____________________________________

Any of these will expose any load balancer all the servers are going to be node ports , so it means that after having everything installed 
we are going to create the central ingress that will point to each of these .

There are two more things we need to understand 

1) Docker images has been build and publish them into docker hub . 
2) we are going to be installing the development environment not the production environemnt yet , so for that  we are going to isolate this environment in its own namespace in k8s 


![image](https://user-images.githubusercontent.com/33585301/119614111-86ce6600-be1b-11eb-868b-32ac3b9c3836.png)

we have created our namespace where we are going to build all our resouces 

lets open one of the microserves to understand little bit , how it will be installed 

inside infra/helm folder we have 

-value.yml 
-templates 
-chart.yml
- create.sh  - which is basically the command which will install this 

inside the templates 
-deployments
- services 



![image](https://user-images.githubusercontent.com/33585301/119614278-b5e4d780-be1b-11eb-9eb9-ba49041f9a76.png)

the service type is node port
repoistory is mariomerico/resource-api and tag is latest 

just make sure to come here into values file and change the repository . This one is open 


![image](https://user-images.githubusercontent.com/33585301/119614305-c006d600-be1b-11eb-9aaa-890a5f941889.png)


in the create.sh file we can see that 'helm upgrade --install ' which means that if it doesnt exist it will install .
But if it exists it simply will update it with latest changes on the chart .
The namespace specify 'development' 
name of the chart is 'resource-api-development' 
like to put the end of the release name the namespace i'm working on 


![image](https://user-images.githubusercontent.com/33585301/119614588-1411ba80-be1c-11eb-825e-e52b98fda192.png)


we have it installed . 

____________________

Go to inventory API 

![image](https://user-images.githubusercontent.com/33585301/119615065-9b5f2e00-be1c-11eb-80ca-cc51676b827d.png)


we have it installed . 


_______________

Go to renting api, client api, front end api

![image](https://user-images.githubusercontent.com/33585301/119614675-36a3d380-be1c-11eb-8963-17701f7ebff4.png)

and run create.sh and install it . 


WE have all our 5 microserves installed on our application 
____________


Now we have to install the ingress that will be processed by the controller and create the loadbalancer we need to accessing these.
have created helm chart for that , thatz in the infra structure folder in the root of these repository 


![image](https://user-images.githubusercontent.com/33585301/119615234-ce092680-be1c-11eb-99f6-c9866b214604.png)


in central ingress we have 2 values files : 

1) for development 
2) for production 


Check what the value for development has :

The value file has an object called ingress , we have a sub object called internal . In the production one we are gonna find another object called external .
So it basically ask all the  anotation that the ingress needs to work with the ALB controller .


![image](https://user-images.githubusercontent.com/33585301/119615307-dfeac980-be1c-11eb-924c-1f974e807d09.png)


It has object which is an array of host . Which has the host name . We need to come here and change the host names , service name & port of the applicatin 


![image](https://user-images.githubusercontent.com/33585301/119616848-bc288300-be1e-11eb-80e2-6fe1d13c9ac3.png)

In the template folder ingress external and ingress internal 
open the ingress internal 

Here we are using range rule , for creating all the host on our ingress . The other file is external ingress , which is going to create an external loadbalancer whenever we want to deploy this to production. But as we know in our architecture and our developer environment needs to be protected and needs to leave only in the private subnets , so thatz why we need to create openvpn we can access those resources that are actually private . 


![image](https://user-images.githubusercontent.com/33585301/119616993-eed27b80-be1e-11eb-81d5-07801e0d04ee.png)

![image](https://user-images.githubusercontent.com/33585301/119617126-1aedfc80-be1f-11eb-9f2e-932ed2263f5b.png)


lets create this helm chart using the create.sh we also have in here . 
Now we have that ingress created on our side , we need to wait a little bit until 3 things happens 

1) Alg controller identify ingress & creating the load balancer 
2) provision of these load balancer 
3) creation of our A Records on our hosted zone of route 53 by exteranal dns 

_______________________

## Aws managment console 



![image](https://user-images.githubusercontent.com/33585301/119617192-2d683600-be1f-11eb-9961-8684b79ff4d8.png)

we have one now that is actually provisioning , will take couple of minutes . When it finishes we have to wait other 2 minutes or less 
until external dns identifies these one and creates records on the hosted zone 


![image](https://user-images.githubusercontent.com/33585301/119617345-61dbf200-be1f-11eb-8a7d-6eb70c251b59.png)


when the loadbalancer is active , take a look at listeners .

WE will find 2 listners port 80 for doing reddis ? & 443 


Lets take a look on the rules this one (443) has 

Given the host we have renting api, resource api , inventory api, client api , bookstore and the api itself which is the proxy have there own target groups .
Pointing to the nodegroup created by k8s to expose our applicatin 

![image](https://user-images.githubusercontent.com/33585301/119617227-3a852500-be1f-11eb-8746-063a629f23ba.png)
![image](https://user-images.githubusercontent.com/33585301/119617421-76b88580-be1f-11eb-9aae-9e8549430ab8.png)

_______

Now lets visit route53 -> hostedzones 


here we have all the records the external dns has created for us 




![image](https://user-images.githubusercontent.com/33585301/119617492-8a63ec00-be1f-11eb-90b8-a7c172db08c8.png)


lets navigate to the bookstore.dev.mariomirco.com that is the front end of our application . 


we have opened 'https://bookstore.dev.mariomerco.com/' redirects ad to 'https' ports 

its working because we can see reources, clients, inventory for each of the resources we had 


![image](https://user-images.githubusercontent.com/33585301/119617527-918afa00-be1f-11eb-95c8-a70adf6569b4.png)



If we open developer tool (browser) -> network -> clients

we will be able to see all the http call front end is doing to back end 

![image](https://user-images.githubusercontent.com/33585301/119617633-a8315100-be1f-11eb-9f2c-c4a1f42f620e.png)


as you can see the the end point here is using the api (https://api.dev.mariomeroc.com/clients-api)  for this example 


![image](https://user-images.githubusercontent.com/33585301/120441383-1d61d080-c3a2-11eb-8fda-ce85625b8fc2.png)


if we open resources the end point will be (https://api.dev.mariomeroc.com/resource-api)

which is what we have specified on the proxy 


![image](https://user-images.githubusercontent.com/33585301/120441419-294d9280-c3a2-11eb-941c-3be88ea9e947.png)


Now we have deployed everything we required for this solution to run  and to be exposed through a load balancer using a dns and using ssl terminated communication b/w you and the server .

keep in mind that this is also possible because connect to vpn that we have established . So all of these resources are private . 
If not in vpn it wont load simply because not in that private network , so from here we are going to start adding customization making this solution more reliable 
more secure and more automation oriented .

This is our base line although later in this course we are going to create an production environemnt we will continue walking through the whole solution from the conception of the code building it and deploying all the way up to the production . 


