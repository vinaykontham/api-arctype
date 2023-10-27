


Instructions for  ${ApiName}

[TOC levels=1-6]: # "# Table of Contents"

# Table of Contents
- [Reference API Proxy](#reference-api-proxy)
- [Description](#description)
- [Purpose](#purpose)
- [Pre-Requisites](#pre-requisites)
- [External configuration](#external-configuration)
    - [Access token generation](#access-token-generation)
    - [Maven cicd settings xml config](#maven-cicd-settings-xml-config)
- [Usage Instructions](#usage-instructions)

- [Lifecycle for API proxy deployment](#lifecycle-for-api-proxy-deployment)
    - [Lint Proxy](#lint-proxy)
    - [Unit Tests](#unit-tests)
    - [Bundle Package](#bundle-package)
    - [Upload Caches](#upload-caches)
    - [Upload Target Servers](#upload-target-servers)
    - [Upload KVM's](#upload-kvms)
    - [Deploy Proxy](#deploy-proxy)
    - [Upload Products](#upload-products)
    - [Upload Developers](#upload-developers)
    - [Upload Apps](#upload-apps)
    - [Export Keys](#export-keys)
    - [Integration Tests](#integration-tests)
    - [Upload to Artifactory](#upload-to-artifactory)



While deploying an API proxy to any targeted Apigee Edge env, it must go through several CI 
phases such as Linting, Code Coverage, Unit testing, Build and packaging, configuring environment
level dependencies (i.e. KVM's, Caches, Target Servers), Configuring org level entities (i.e. API products,
Developers, Apps) and integration testing. Also, artifacts must be deployed to the remote locations (i.e. Nexus).

    

The DevOps engineer should be able to execute all the above mentioned CI phases by manually running
the steps.

- JDK 8
- Apache Maven
- Linux/WSL
- Settings.xml file in local 
- SAML Tokens (Access & Refresh)


The external configuration references required for token generation and an external settings.xml


A template for the cicd settings xml (which must be configured into Jenkins as a global config file)


This Api Proxy project will be used to deploy hdfc-devops-hello-world to apigee edge. 

How to clone the hdfc-devops-hello-world and change the directory 

```bash
# clone hdfc-devops-hello-world repository
git clone https://bitbucket.org/hdfc/hdfc-devops-hello-world.git

# change directory into the repository
cd hdfc-devops-hello-world
```


To make local invocation easier, load the app token from an environment variable

Example of how to export a local environment variable:
```bash
export APIGEE_APITOKEN=abc123
```


```
# invoke apigeelint directly
HTML View
apigeelint -s edge/apiproxy -f html.js

OR, Table View
apigeelint -s edge/apiproxy -f table.js

# invoke linting through maven; run from /edge directory in project root
cd edge
mvn test -Pproxy-linting             
```
 
```
# run from /edge directory in project root
mvn test -Pproxy-unit-test
```
 

run this from `edge` directory:
```bash
mvn package \
    -Papigee \
    -Denv=dev \
    -Dorg=amer-mint-partner01 \
    -DvhostProtocol=https \
    -DvhostDomainName=amer-mint-partner01-prod.apigee.net \
    -DvhostDomainPort=443 \
    -DvhostEdgeName=secure \
    -Dapigee.config.dir=target/resources/edge \
    -Dapigee.config.options=create \
    -Dapigee.config.exportDir=target/test/integration \
    -Dtokenurl=xyz \
    -Dauthtype=oauth
```
 

```bash
mvn apigee-config:caches \
    -Papigee  \
    -Denv=dev \
    -Dorg=amer-mint-partner01 \
    -DvhostProtocol=https \
    -DvhostDomainName=amer-mint-partner01-dev.apigee.net \
    -DvhostDomainPort=443 \
    -DvhostEdgeName=secure \
    -Dapigee.config.dir=target/resources/edge \
    -Dapigee.config.options=create \
    -Dapigee.config.exportDir=target/test/integration \
    -Dapigee.api.port=8080 \
    -Dapigee.api.host=api.enterprise.apigee.com \
    -Dapigee.api.protocol=https \
    -Dtokenurl=xyz \
    -Dauthtype=oauth \
    -Dbearer= ************* \
    -Drefresh= ***************
```
 

```bash
mvn apigee-config:targetservers \
    -Papigee \
    -Denv=dev \
    -Dorg=amer-mint-partner01 \
    -DvhostProtocol=https \
    -DvhostDomainName=amer-mint-partner01-dev.apigee.net \
    -DvhostDomainPort=443 \
    -DvhostEdgeName=secure \
    -Dapigee.config.dir=target/resources/edge \
    -Dapigee.config.options=create \
    -Dapigee.config.exportDir=target/test/integration \
    -Dapigee.api.port=8080 \
    -Dapigee.api.host=api.enterprise.apigee.com \
    -Dapigee.api.protocol=https \
    -Dtokenurl=xyz \
    -Dauthtype=oauth \
    -Dbearer= ************* \
    -Drefresh= ***************
```
 

```bash
mvn apigee-config:keyvaluemaps \
    -Papigee \
    -Denv=dev \
    -Dorg=amer-mint-partner01 \
    -DvhostProtocol=https \
    -DvhostDomainName=amer-mint-partner01-dev.apigee.net \
    -DvhostDomainPort=443 \
    -DvhostEdgeName=secure \
    -Dapigee.config.dir=target/resources/edge \
    -Dapigee.config.options=create \
    -Dapigee.config.exportDir=target/test/integration \
    -Dapigee.api.port=8080 \
    -Dapigee.api.host=api.enterprise.apigee.com \
    -Dapigee.api.protocol=https \
    -Dtokenurl=xyz \
    -Dauthtype=oauth \
    -Dbearer= ************* \
    -Drefresh= ***************
```
 

```bash
mvn apigee-enterprise:deploy \
    -Papigee \
    -Denv=dev \
    -Dorg=amer-mint-partner01 \
    -DvhostProtocol=https \
    -DvhostDomainName=amer-mint-partner01-dev.apigee.net \
    -DvhostDomainPort=443 \
    -DvhostEdgeName=secure \
    -Dapigee.config.dir=target/resources/edge \
    -Dapigee.config.options=create \
    -Dapigee.config.exportDir=target/test/integration \
    -Dapigee.api.port=8080 \
    -Dapigee.api.host=api.enterprise.apigee.com \
    -Dapigee.api.protocol=https \
    -Dtokenurl=xyz \
    -Dauthtype=oauth \
    -Dbearer= ************* \
    -Drefresh= ***************
```
 

```bash
mvn apigee-config:apiproducts \
    -Papigee \
    -Denv=dev \
    -Dorg=amer-mint-partner01 \
    -DvhostProtocol=http \
    -DvhostDomainName=amer-mint-partner01-dev.apigee.net \
    -DvhostDomainPort=443 \
    -DvhostEdgeName=secure \
    -Dapigee.config.dir=target/resources/edge \
    -Dapigee.config.options=create \
    -Dapigee.config.exportDir=target/test/integration \
    -Dapigee.api.port=8080 \
    -Dapigee.api.host=api.enterprise.apigee.com \
    -Dapigee.api.protocol=https \
    -Dtokenurl=xyz \
    -Dauthtype=oauth \
    -Dbearer= ************* \
    -Drefresh= ***************
```
 

```bash
mvn apigee-config:developers \
    -Papigee \
    -Denv=dev \
    -Dorg=amer-mint-partner01 \
    -DvhostProtocol=https \
    -DvhostDomainName=amer-mint-partner01-dev.apigee.net \
    -DvhostDomainPort=443 \
    -DvhostEdgeName=secure \
    -Dapigee.config.dir=target/resources/edge \
    -Dapigee.config.options=create \
    -Dapigee.config.exportDir=target/test/integration \
    -Dapigee.api.port=8080 \
    -Dapigee.api.host=api.enterprise.apigee.com \
    -Dapigee.api.protocol=https \
    -Dtokenurl=xyz \
    -Dauthtype=oauth \
    -Dbearer= ************* \
    -Drefresh= ***************
```
 

```bash
mvn apigee-config:apps \
    -Papigee \
    -Denv=dev \
    -Dorg=amer-mint-partner01 \
    -DvhostProtocol=https \
    -DvhostDomainName=amer-mint-partner01-dev.apigee.net \
    -DvhostDomainPort=443 \
    -DvhostEdgeName=secure \
    -Dapigee.config.dir=target/resources/edge \
    -Dapigee.config.options=create \
    -Dapigee.config.exportDir=target/test/integration \
    -Dapigee.api.port=8080 \
    -Dapigee.api.host=api.enterprise.apigee.com \
    -Dapigee.api.protocol=https \
    -Dtokenurl=xyz \
    -Dauthtype=oauth \
    -Dbearer= ************* \
    -Drefresh= ***************
```
 

```bash
mvn apigee-config:exportAppKeys \
    -Papigee \
    -Denv=dev \
    -Dorg=amer-mint-partner01 \
    -DvhostProtocol=https \
    -DvhostDomainName=amer-mint-partner01-dev.apigee.net \
    -DvhostDomainPort=443 \
    -DvhostEdgeName=secure \
    -Dapigee.config.dir=target/resources/edge \
    -Dapigee.config.options=create \
    -Dapigee.config.exportDir=target/test/integration \
    -Dapigee.api.port=8080 \
    -Dapigee.api.host=api.enterprise.apigee.com \
    -Dapigee.api.protocol=https \
    -Dtokenurl=xyz \
    -Dauthtype=oauth \
    -Dbearer= ************* \
    -Drefresh= ***************
```
 
 

Invoke Maven and get the required configurations 

```bash
mvn clean test \
    -Dorg=amer-mint-partner01 \
    -Denv=dev \
    -Dkey=******** \
    -Dsecret=********* \
```
Execute node command to run integration test
```bash
node ./node_modules/cucumber/bin/cucumber-js target/test/integration/features --format json:target/reports.json
```


```bash
Disable for developers. Only CI Server can deploy to the remote location. 
```

