# quarkus-super-heroes on openshift

This repo focuses on deploying quarkus super heroes workshop into an openshift cluster. A step not included in original guide.

Make sure to refer to original quarkus super heroes workshop : https://quarkus.io/quarkus-workshops/super-heroes 

![image](https://user-images.githubusercontent.com/26201808/161063761-b2b2f21e-9b51-4ce6-8a5a-84209bd8189a.png)

# Steps to follow for deploying super heroes microservice app into OpenShift

git clone https://github.com/vaibhavjain4/quarkus-super-heroes.git -b master

cd quarkus-super-heroes

mvn clean install -f extension-version

oc login -u \<username\> 

oc new-project super-heroes-app

oc new-app --name villainsdb -e POSTGRESQL_USER=superbad -e POSTGRESQL_PASSWORD=superbad -e POSTGRESQL_DATABASE=villains_database postgresql:10-el8

cd rest-villains

 ./mvnw clean package -Dquarkus.kubernetes.deploy=true -Dmaven.test.skip -Dquarkus.openshift.ports."ports".container-port=8084
 
 oc expose svc/rest-villains --port=8084
 
 oc get route -> access application on this route, remember to use http://\<route\>

oc new-app --name heroesdb -e POSTGRESQL_USER=superman -e POSTGRESQL_PASSWORD=superman -e POSTGRESQL_DATABASE=heroes_database postgresql:10-el8

cd rest-heroes

./mvnw clean package -Dquarkus.kubernetes.deploy=true -Dmaven.test.skip -Dquarkus.openshift.ports."ports".container-port=8083

 oc expose svc/rest-villains --port=8083
 
 oc get route -> access application on this route, remember to use http://\<route\>
 
 oc new-app --name fightsdb -e POSTGRESQL_USER=superfight -e POSTGRESQL_PASSWORD=superfight -e POSTGRESQL_DATABASE=fights_database postgresql:10-el8
 
 cd rest-fights
 
 ./mvnw clean package -Dquarkus.container-image.build=true -Dmaven.test.skip

Login to console -> Developer => Click on add container images 
