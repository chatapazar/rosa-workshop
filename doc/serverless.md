# OpenShift Serverless, auto scale up & down by event
<!-- TOC -->

- [OpenShift Serverless, auto scale up \& down by event](#openshift-serverless-auto-scale-up--down-by-event)
  - [Serverless Deployment](#serverless-deployment)
  - [Back to Table of Content](#back-to-table-of-content)

<!-- /TOC -->

## Serverless Deployment

- From Topology view, go to top right, click plus icon, select Container images

  ![](../images/5/5-1.png)

- In Deploy Image, select Image stream tag from internal registry

- select Project : `user1-lab1`

- select Image Stream : `hello`
  
- select Tag : `latest`   

- select project : `<username>-lab1`

- set Runtime icon : `openshift` 

  ![](../images/5/5-2.png)

- In General section, set Create application

- set Application Name : `hello-serverless`

- set Name : `hello-serverless`
  
- In Deploy, set Resource type : `Serverless Deployment`   

  ![](../images/5/5-3.png)

- Leave default in other section. click Create.

  ![](../images/5/5-4.png)

- wait until deploy complete, click KSVC `hello-serverless`

  ![](../images/5/5-5.png)

- test hello-serverless application click route icon to open application in new tab
  
  ![](../images/5/5-6.png)

- wait until application auto scale down (1 minute)

  ![](../images/5/5-7.png)

- in topology view, no pod start and deployment show 0 pod 

  ![](../images/5/5-8.png)

- test call application again by click route, serverless will automatic start pod.

  ![](../images/5/5-9.png)

  ![](../images/5/5-10.png)

## Back to Table of Content

- [Red Hat OpenShift Service for AWS (ROSA) Workshop](../README.md)






