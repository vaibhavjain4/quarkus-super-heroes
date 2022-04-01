# Deploy quarkus-super-heroes on Red Hat OpenShift

This repo focuses on deploying quarkus super heroes workshop into an openshift cluster. A step not included in original guide.

Make sure to refer to original quarkus super heroes workshop : https://quarkus.io/quarkus-workshops/super-heroes 

![image](https://user-images.githubusercontent.com/26201808/161063761-b2b2f21e-9b51-4ce6-8a5a-84209bd8189a.png)

## Demo Recording 

<img width="1153" alt="image" src="https://user-images.githubusercontent.com/26201808/161100693-53f90968-515e-4818-b276-309a92242f67.png">


<h3><b><centre> https://youtu.be/421h2h9OUFY </centre></b></h3> 

## Pre-requisities

 Make sure pre-requisites are completed as mentioned here : https://quarkus.io/quarkus-workshops/super-heroes/#_software_requirements 
 
 Try running one of the services like rest-heroes $./mvnw quarkus:dev 

# Steps to follow for deploying super heroes microservice app into OpenShift

Step 1 : git clone https://github.com/vaibhavjain4/quarkus-super-heroes.git -b master

Step 2 : cd quarkus-super-heroes

Step 3 : mvn clean install -f extension-version

Step 4 : oc login -u \<username\> 

Step 5 : oc new-project super-heroes-app

Step 6 : oc new-app --name villainsdb -e POSTGRESQL_USER=superbad -e POSTGRESQL_PASSWORD=superbad -e POSTGRESQL_DATABASE=villains_database postgresql:10-el8

Step 7 : cd rest-villains

Step 8 : ./mvnw clean package -Dquarkus.kubernetes.deploy=true -Dmaven.test.skip -Dquarkus.openshift.ports."ports".container-port=8084
 
Step 9 : oc expose svc/rest-villains --port=8084
 
Step 10 : oc get route -> access application on this route, remember to use http://\<route\>

Step 11 : oc new-app --name heroesdb -e POSTGRESQL_USER=superman -e POSTGRESQL_PASSWORD=superman -e POSTGRESQL_DATABASE=heroes_database postgresql:10-el8

Step 12 : cd rest-heroes

Step 13 : ./mvnw clean package -Dquarkus.kubernetes.deploy=true -Dmaven.test.skip -Dquarkus.openshift.ports."ports".container-port=8083

Step 14 : oc expose svc/rest-heroes --port=8083
 
Step 15 : oc get route -> access application on this route, remember to use http://\<route\>
 
Step 16 : oc new-app --name fightsdb -e POSTGRESQL_USER=superfight -e POSTGRESQL_PASSWORD=superfight -e POSTGRESQL_DATABASE=fights_database postgresql:10-el8
 
Step 17 : cd rest-fights
 
Step 18 : ./mvnw clean package -Dquarkus.container-image.build=true -Dmaven.test.skip
 
Step 19 : Login to console -> Administrator => Operatorhub => Deploy AMQ Streams Operator in current namespace only

Step 20 : Installed Operators => AMQ Streams -> Kafka -> Create Kafka -> Name = super-heroes-kafka -> create. Wait till Condition is Ready

Step 21 : Installed Operators => AMQ Streams -> Kafka Topics -> Create Kafka Topic -> YAML view -> name: fights, strimzi.io/cluster: super-heroes-kafka, partitions: 1, replicas: 1

Step 22 : Login to console -> Developer => Click on add container images -> Image stream tag from internal registry -> Project : super-heroes-app , Image Stream : rest-fights , Tag : 1.0.0-SNAPSHOT , Runtime icon : Quarkus ; Application : No Application group ; name : rest-fights ; Target Port : 8082 ; Show Advance Routing options -> Insecure traffic : Allow ; Create

Step 23 : Check Route & access rest-fights application ; validate http://\<route\>/api/fights/randomfighters

Step 24 : Also see : https://\<route\>/q/health & https://\<route\>/q/metrics

## Backend components deployment complete. Let's deploy front end components - event statistics & angular ui

Step 25 : cd event-statistics

Step 26 : ./mvnw clean package -Dquarkus.kubernetes.deploy=true -Dmaven.test.skip -Dquarkus.openshift.ports."ports".container-port=8085

Step 27 : oc expose svc/event-statistics --port=8085

Step 28 : oc get route -> access application on this route, remember to use http://\<route\> only.

### Deploy Angular UI

Step 29 : Login to Developer Console -> Click on +Add -> Import from Git 
    Git Repo URL : https://github.com/vaibhavjain4/quarkus-super-heroes.git
    Git Reference : master
    Context dir : ui-super-heroes
    Source Secret : leave it blank
    Import strategy : Dockerfile
    Application : No Application group
    name: ui-super-heroes
    Resources : Deployment
    Target port : 8080 (type)
    Create a route : selected
    Advanced Routing Options :
        Hostname : blank
        Path: /
        Secure Route : selected
        TLS termination : Edge
        Insecure traffic : Allow

Click on Create and wait for build to complete 

If you access the ui application, expect it to not working as the backend api are not yet exposed to browser. Follow the steps :

Login to Developer Console -> Click on Search -> Under Resources drop down -> Search for Route.

Notice ui-super-heroes route url and copy the url.

Step 30 : Create Route ->
    Name : ui-fights-api
    Hostname : \<paste the ui-super-heroes route url here\> \(Remove any http:// or https:// from value\)
    Path : /api/
    Service : Select rest-fights from dropdown
    Target port : 8082
    Secure Route : Check
    TLS termination : Edge
    Insecure traffic : Allow
    
Click on Create.

Access the ui-super-heroes route application from topology or route sections. Refresh it few times if not getting the images.

![image](https://user-images.githubusercontent.com/26201808/161077044-4b1e4df8-c193-4910-9bae-c41b15f388ab.png)

Go back to event-statistics application page and you should see the data being updated there. If opening a new tab, you might not see any data. Try making few clicks on ui app and leave this open. It shall update as you make few click there. Alternatively also can check log.

<img width="945" alt="image" src="https://user-images.githubusercontent.com/26201808/161077924-0f1b7814-0ed3-42a2-90e7-e748c01fcc17.png">


    
    


    
    
