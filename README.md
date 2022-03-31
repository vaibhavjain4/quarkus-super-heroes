# quarkus-super-heroes on openshift

This repo focuses on deploying quarkus super heroes workshop into an openshift cluster. A step not included in original guide.

Make sure to refer to original quarkus super heroes workshop : https://quarkus.io/quarkus-workshops/super-heroes 

# Steps to follow for deploying super heroes microservice app into OpenShift

git clone https://github.com/vaibhavjain4/quarkus-super-heroes.git -b master

cd quarkus-super-heroes

mvn clean install -f extension-version

oc login -u \<username\> 

oc new-project super-heroes-app

oc new-app --name villainsdb -e POSTGRESQL_USER=superbad -e POSTGRESQL_PASSWORD=superbad -e POSTGRESQL_DATABASE=villains_database postgresql:10-el8

