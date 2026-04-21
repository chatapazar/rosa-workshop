# First Container (Pod) on ROSA
<!-- TOC -->

- [First Container (Pod) on ROSA](#first-container-pod-on-rosa)
  - [Red Hat OpenShift Service on AWS](#red-hat-openshift-service-on-aws)
  - [Start using OpenShift Console](#start-using-openshift-console)
  - [OpenShift Project](#openshift-project)
  - [Deploy First Application from source code with OpenShift Builds S2I](#deploy-first-application-from-source-code-with-openshift-builds-s2i)
  - [Deploy Second Application from ContainerFile with OpenShift Builds S2I](#deploy-second-application-from-containerfile-with-openshift-builds-s2i)
  - [Deploy Third Application from Container Image with OpenShift Builds S2I](#deploy-third-application-from-container-image-with-openshift-builds-s2i)
  - [Using OpenShift Command Line with Web Console](#using-openshift-command-line-with-web-console)
  - [Back to Table of Content](#back-to-table-of-content)

<!-- /TOC -->

## Red Hat OpenShift Service on AWS

A fully managed turnkey application platform that allows organizations to increase operational efficiency, refocus on innovation, and quickly build, deploy, and scale applications in a native AWS environment.

  ![](../images/rosa.png)

`Start learning` : Explore learning paths designed to help you use Red Hat OpenShift Service on AWS

- [Interactive product overview](https://www.redhat.com/en/products/interactive-demo/install-rosa)

- [Red Hat OpenShift Service on AWS explained](https://cloud.redhat.com/learn/red-hat-openshift-service-aws-rosa-explained)

- [Foundations of ROSA](https://cloud.redhat.com/learn/foundations-red-hat-openshift-service-aws-rosa)

- [Getting started with Red Hat OpenShift Service on AWS](https://cloud.redhat.com/learn/getting-started-red-hat-openshift-service-aws-rosa)

## Start using OpenShift Console
- open browser to URL from Register Page

  - confirm URL from instructor

- login to openshift with your username/password
  
  - username: `xxx` --> get from register page
  
  - password: `xxx` --> get from register page
  
  ![](../images/1/1-1.png)

- if login first time, OpenShift console will launch Launch tour, click `Launch tour`, click Next until `Launch tour` complete!
  
  ![](../images/1/1-2.png)

  ![](../images/1/1-3.png)

  ![](../images/1/1-4.png)

  ![](../images/1/1-5.png)

  ![](../images/1/1-6.png)

  ![](../images/1/1-7.png)

- Config User Preferences, go to top right, clieck your username, select `User Preferences`

  ![](../images/1/1-8.png)

- In General Tab, Choose the theme you want (Light or Dark).

  ![](../images/1/1-9.png)

- In Application Tab, Set Resource type to `Deployment`

  ![](../images/1/1-10.png)

## OpenShift Project

- From left menu, select Home>Projects

  ![](../images/1/1-11.png)

- Click create project
- set Project Name: `<username>-lab1`, change <username> to your username such as user1, click `Create`

  ![](../images/1/1-12.png)

- View OpenShift Project in Overview Tab, Project Overview serves as a central dashboard for monitoring the health and status of a specific project (namespace).

  ![](../images/1/1-13.png)

- change to Details Tab, it provides a comprehensive overview of project resources, utilization, and configuration. It serves as a central hub for managing project-level metadata, status, and associated Kubernetes resources. 

  ![](../images/1/1-14.png)

- change to YAML Tab, it provides a direct interface to view and edit the live configuration of cluster resources in YAML format. It is a critical tool for both developers and administrators to manage resource manifests without leaving the browser. 

  ![](../images/1/1-15.png)

- change to Workloads Tab, it provides a centralized view of all running applications and their underlying resources.

  ![](../images/1/1-16.png)

- You can access to Topology view by select Workloads>Topology in left menu. (same view with project>workload)

  ![](../images/1/1-17.png)

## Deploy First Application from source code with OpenShift Builds S2I

- Review Java Source Code from Git --> `https://gitlab.com/chatapazar/openshift-workshop.git` (source in src folder)
  
    ![](../images/1/1-30.png)

- Review Java Source Code in src folder
  
  ![](../images/1/1-31.png)

- From Topology view, click `Add page` Link.

  ![](../images/1/1-17.png)

- In Add page, click star icon for add to Favorites in left menu.

  ![](../images/1/1-18.png)

  ![](../images/1/1-19.png)

- In Add page, click `Git Repository>Import from Git`

  ![](../images/1/1-20.png)

- OpenShift Source-to-Image (S2I) is a framework that automates the creation of container images by injecting application source code into a pre-configured builder image. It eliminates the need for developers to write or maintain Dockerfiles, ensuring a consistent and secure build process across teams. 

- In Import from Git, input git url to `https://gitlab.com/chatapazar/openshift-workshop.git`
- set project to `<username>-lab1`
- OpenShift S2I will automatic select Builder Image from your source code, in case s2i can't detect base image. you can manual select.

  ![](../images/1/1-22.png)

- click Edit Import Strategy, if you want to change builder image. you can select builder image from dropdown menu. `Do not change the builder image because it will cause the build to fail.`

  ![](../images/1/1-21.png)

- scroll down to General, 
  - set Application name to `backend` 

  - set Name to `backend`

  - leave default Build option to `BuildConfig`

  ![](../images/1/1-23.png)

- leave default Resource type, Target port and Create a route
- Click Create

  ![](../images/1/1-24.png)

- OpenShift will call OpenShift Builds to automatic build image from your source code. click View logs link in `Build #1` to view build log

  ![](../images/1/1-25.png)

- In Build page, Logs tab, wait until status build `backend-1` change to complete.

  ![](../images/1/1-26.png)

- Back to Topology view, check Workloads>Topology in left menu,

  ![](../images/1/1-27.png)

- In Topology view, click `(D) Backend` or `Duke` icon (Duke is the official mascot of the Java programming language). Wait until the circle around the duke icon turns dark blue. click route icon on top right of `Duke` icon or click location of `(RT) backend` in Routes section of Backend Deployment panel. 

  ![](../images/1/1-28.png)

- new tab will be open and browse to your applicaiton

  ![](../images/1/1-29.png)

- test call rest api of this application by add `/backend` at the end of current url.

  ![](../images/1/1-32.png)

## Deploy Second Application from ContainerFile with OpenShift Builds S2I

- Reveiw source code from `https://gitlab.com/chatapazar/node-example.git`
  
  ![](../images/1/1-34.png)

- Review Dockerfile 

  ![](../images/1/1-35.png)  


- In top right of OpenShift Console, click plus icon and select Import from Git

  ![](../images/1/1-33.png)

- In Import from Git, set git url to `https://gitlab.com/chatapazar/node-example.git`
  
  ![](../images/1/1-36.png)

- OpenShift will automatic select Dockerfile , click Edit Import Strategy to view Dockerfile path

  ![](../images/1/1-37.png)  

- In General, in Application, select Create Application

- set Application name to `nodejs`

- set Name to 'nodejs`

- leave default Build option

  ![](../images/1/1-38.png) 

- leave default resource type, target port and create a route, click create

  ![](../images/1/1-39.png)  

- view build log by click `View logs` in Build #1

  ![](../images/1/1-40.png) 

- wait until build `nodejs-1` change status to complete.

  ![](../images/1/1-41.png) 

- Back to Topology view, wait until deployment complete!

  ![](../images/1/1-42.png)  

- click Route to access applicaiton

  ![](../images/1/1-43.png)   

## Deploy Third Application from Container Image with OpenShift Builds S2I

- Review container image at dockerhub --> `https://hub.docker.com/r/openshift/hello-openshift/` 
  
  ![](../images/1/1-56.png)      

- In top right, click plus icon, select Container images

  ![](../images/1/1-44.png) 

- In Deploy Image page, select Image name from external registry

- Input `openshift/hello-openshift:latest`, wait until show `Validated` status.

  ![](../images/1/1-45.png)  

- In General section, select create application, set Application Name to `hello`
- set Name to `hello`

  ![](../images/1/1-46.png) 

- leave all default. click Create.

  ![](../images/1/1-47.png) 

- wait until deploy complete!

  ![](../images/1/1-48.png)  

- click route to open new tab.

  ![](../images/1/1-49.png)    

- View all application in Topology View

  ![](../images/1/1-50.png) 

## Using OpenShift Command Line with Web Console

- Go to top right of Console, click OpenShift command line icon to open 

  ![](../images/1/1-51.png)  

- In Initialize terminal, select Project `user1-lab1`, click Start.

  ![](../images/1/1-52.png) 

- Wait until web terminal start complete.  
  
  ![](../images/1/1-53.png) 

- In command line terminal, check current project by below command
  
  ```bash
  oc project
  ```
  
  example of output
  
  ```bash
  Using project "user1-lab1" from context named "user1-context" on server "https://172.30.0.1:443".
  ```
     
  ![](../images/1/1-54.png)  
   
- if current project is not your project (such as result is not 'Using project "user1-lab1"'), use below command to set current project to command line context
  
  ```bash
  oc project <your project>
  ```

- Test call backend service api (REST)
  
  ```bash
  curl https://$(oc get route backend -o jsonpath='{.spec.host}')/backend
  ```
  
  example of result
  
  ```bash
  Backend version:v1, Response:200, Host:backend-7b5c56fc8c-t57wl, Status:200, Message: Hello, World
  ```
  
  Remark: Host name in result of this api is name of Pod, please check and verify it!
  
  ![](../images/1/1-55.png)    
  
- If all done!, You are ready for the next step.

## Back to Table of Content

- [Red Hat OpenShift Service for AWS (ROSA) Workshop](../README.md)