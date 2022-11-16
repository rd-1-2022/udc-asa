# Spring Azure App Commands

This repository contains three commands to help you deploy a Spring application to Spring Azure Apps.

* spring asa provision
* spring asa deploy
* spring asa delete-app

## Prerequisites

* The Spring CLI is installed
* The Azure CLI is installed

You should be logged into Azure and be able to execute the following command

```
az group list
```

The ASA commands will use the first resource group returned from the `az group list` command to create a Spring Service Instance and a Spring Application on Azure.
Refer to the overview section for the definiton of these terms.

## Installation

To install these commands into the Spring CLI execute the following command

```
spring command add --from https://github.com/rd-1-2022/udc-asa 
```

## Azure Spring Apps Overview

An Azure resource group is a container that holds related resources for an Azure solution. A resource group might contain storage, virtual machines, apps, dashboards, services, or almost anything you deal with in Azure.  
Refer to the guide [How to manage Azure resource groups with the Azure CLI](https://learn.microsoft.com/en-us/cli/azure/manage-azure-groups-azure-cli) for more information.

In our case, the Azure resource group will contain Azure Spring Apps Service Instances, or simply *service instance* resource, as well an Azure Spring application, or simply *App*. 

App and Deployment are the two key concepts in the resource model of Azure Spring Apps.
In Azure Spring Apps, an App is an abstraction of one business app. One version of code or binary deployed as the App runs in a Deployment.
Apps run in an Azure Spring Apps Service Instance, or simply service instance, as shown next.

![Concepts](https://learn.microsoft.com/en-us/azure/spring-apps/media/spring-cloud-app-and-deployment/app-deployment-rev.png)

Refer to the [Concept guide on App and deployment in Azure Spring Apps](https://learn.microsoft.com/en-us/azure/spring-apps/concept-understand-app-and-deployment) for more information

## Command Overview

### `spring asa provision`

This command will use the first resource group that is returned from the command `az group list`.

Using that resource group, an Azure Spring Service instance will be defined using a command such as

```
az spring create --resource-group scdf-demo --name si-mark
```
The `--name` option is the `service-instance-name` and is templated as the variable `si-{{user-name}}` so that it will be unique to you.

The command will then create a new Azure Spring Application in the Azure Spring Service instance using a command such as

```
az spring app create --resource-group scdf-demo --service si-mark --name demo --assign-endpoint true
```

The `--name` option is set to Maven model variable `project-name`.
In this case the name is `demo`

### `spring asa deploy`

This command will deploy the .jar file to the Spring Application that was previously provisioned using a command such as

``` 
az spring app deploy --resource-group scdf-demo --service si-mark --name demo --artifact-path /home/mark/testing/demo/target/demo-0.0.1-SNAPSHOT.jar
```

The `--name` option is set to Maven model variable `project-name`.
In this case the name is `demo`
The `--artifact-path` is constructed from knowledge of the Maven model for your Maven project.

### `spring asa delete-app`

This command will delete your application.  The other resources, such as the Azure Spring Service instance created with `az spring create` and the Azure Spring Application created with `az spring app create` will not be deleted.

## Customization

Take a look at the files in the directories under `.spring/commands/asa` and modify them for what makes sense to have a simple getting started experience in your environment.
This specific sequence of steps was taken assuming that after activating your Azure subscription, you will have already had a Azure Resource Group created for you by your administrator.

## Example usage

```
spring boot new
cd demo
./mvnw clean package
spring command add --from https://github.com/rd-1-2022/udc-asa 
spring asa provision
spring asa deploy
```

