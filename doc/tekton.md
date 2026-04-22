# Basic DevOps with Tekton
<!-- TOC -->

- [Basic DevOps with Tekton](#basic-devops-with-tekton)
  - [Tekton](#tekton)
  - [Create Project](#create-project)
  - [Install Task](#install-task)
  - [Create Pipeline](#create-pipeline)
  - [Start Pipeline](#start-pipeline)
  - [Test Application](#test-application)
  - [Back to Table of Content](#back-to-table-of-content)

<!-- /TOC -->

## Tekton

![](../images/6/tekton.png)

Tekton is a powerful and flexible open-source framework for creating CI/CD systems, allowing developers to build, test, and deploy across cloud providers and on-premise systems. [Get started with Tekton](https://tekton.dev/docs/getting-started/).

Tekton Information

- https://tekton.dev/

- https://github.com/openshift/pipelines-tutorial

## Create Project

- Create a project for the sample application that you will be using in this tutorial:

- set Name : `<username>-tekton`, click create 
  
  ![](../images/6/6-3.png) 

  ![](../images/6/6-4.png) 

- Open Web Terminal and change current project to `<username>-tekton` project.

  ```ssh
  oc project <username>-tekton
  ```

  ![](../images/6/6-5.png) 

- OpenShift Pipelines automatically adds and configures a ServiceAccount named pipeline that has sufficient permissions to build and push an image. This service account will be used later in the tutorial.

  Run the following command to see the pipeline service account:

  ```ssh
  oc get serviceaccount pipeline
  ```

- You will use the simple application during this tutorial, which has a [frontend](https://github.com/openshift/pipelines-vote-ui) and [backend](https://github.com/openshift/pipelines-vote-api)

  ![](../images/6/6-20.png) 

  ![](../images/6/6-19.png) 

## Install Task

Tasks consist of a number of steps that are executed sequentially. Tasks are executed/run by creating TaskRuns. A TaskRun will schedule a Pod. Each step is executed in a separate container within the same pod. They can also have inputs and outputs in order to interact with other tasks in the pipeline.

Here is an example of a Maven task for building a Maven-based Java application:

  ```yaml
    apiVersion: tekton.dev/v1
    kind: Task
    metadata:
    name: maven-build
    spec:
    workspaces:
    - name: filedrop
    steps:
    - name: build
        image: maven:3.6.0-jdk-8-slim
        command:
        - /usr/bin/mvn
        args:
        - install
  ```

When a task starts running, it starts a pod and runs each step sequentially in a separate container on the same pod. This task happens to have a single step, but tasks can have multiple steps, and, since they run within the same pod, they have access to the same volumes in order to cache files, access configmaps, secrets, etc. You can specify volume using workspace. It is recommended that Tasks uses at most one writeable Workspace. Workspace can be secret, pvc, config or emptyDir.

Note that only the requirement for a git repository is declared on the task and not a specific git repository to be used. That allows tasks to be reusable for multiple pipelines and purposes. You can find more examples of reusable tasks in the Tekton Catalog and OpenShift Catalog repositories.

Install the apply-manifests and update-deployment tasks from the repository using oc or kubectl, which you will need for creating a pipeline in the next section:

- Review apply-manifests task at https://github.com/openshift/pipelines-tutorial/blob/master/01_pipeline/01_apply_manifest_task.yaml

  ![](../images/6/6-1.png) 

- Review update-deployment task at https://github.com/openshift/pipelines-tutorial/blob/master/01_pipeline/02_update_deployment_task.yaml

  ![](../images/6/6-2.png) 

- run command to create task in project

  ```ssh
  oc create -f https://raw.githubusercontent.com/openshift/pipelines-tutorial/master/01_pipeline/01_apply_manifest_task.yaml
  oc create -f https://raw.githubusercontent.com/openshift/pipelines-tutorial/master/01_pipeline/02_update_deployment_task.yaml
  ```
  
  ![](../images/6/6-6.png) 

- View task in project, go to Home>Search in left menu , in Resources filter, type `task` and select `(T) Task`, to show all Task in this project.

  ![](../images/6/6-8.png) 

## Create Pipeline

A pipeline defines a number of tasks that should be executed and how they interact with each other via their inputs and outputs.

In this tutorial, you will create a pipeline that takes the source code of the application from GitHub and then builds and deploys it on OpenShift.

- Review pipeline at https://github.com/openshift/pipelines-tutorial/blob/master/01_pipeline/04_pipeline.yaml

  ![](../images/6/6-7.png) 

This pipeline helps you to build and deploy backend/frontend, by configuring right resources to pipeline.

Pipeline Steps:

1. Clones the source code of the application from a git repository by referring (git-url and git-revision param)

2. Builds the container image of application using the buildah task that uses Buildah to build the image

3. The application image is pushed to an image registry by refering (image param)

4. The new application image is deployed on OpenShift using the apply-manifests and update-deployment tasks.

You might have noticed that there are no references to the git repository or the image registry it will be pushed to in pipeline. That's because pipeline in Tekton are designed to be generic and re-usable across environments and stages through the application's lifecycle. Pipelines abstract away the specifics of the git source repository and image to be produced as PipelineResources or Params. When triggering a pipeline, you can provide different git repositories and image registries to be used during pipeline execution. Be patient! You will do that in a little bit in the next section.

The execution order of tasks is determined by dependencies that are defined between the tasks via inputs and outputs as well as explicit orders that are defined via runAfter.

workspaces field allows you to specify one or more volumes that each Task in the Pipeline requires during execution. You specify one or more Workspaces in the workspaces field.

- Go to the top right of Console, click plus icon, select Import YAML 
  
  ![](../images/6/6-9.png) 

- Create pipeline by copy yaml from https://github.com/openshift/pipelines-tutorial/blob/master/01_pipeline/04_pipeline.yaml and paste to Import YAML, click create.

  ![](../images/6/6-10.png) 

- review pipline `build-and-deploy`
  
  ![](../images/6/6-11.png)

- Review persistent volume claim for pipeline at 
https://github.com/openshift/pipelines-tutorial/blob/master/01_pipeline/03_persistent_volume_claim.yaml

  ![](../images/6/6-12.png)

- copy yaml and import to project (same with import pipeline), click create

  ![](../images/6/6-13.png)

- review persistent volume claim
  
  ![](../images/6/6-14.png)

## Start Pipeline

- Go to Pipelines>Pipelines Left Menu, in pipelines tab, 

  ![](../images/6/6-15.png)

- at `build-and-run` pipeline, select action menu (3 dots), and select start

  ![](../images/6/6-23.png)

- In start pipeline dialog, we will start build and deploy api, 

- set deployment-name : `pipelines-vote-api `
  
- set git-url : `https://github.com/openshift/pipelines-vote-api.git`

- set git-revision : `master`

- set IMAGE : `image-registry.openshift-image-registry.svc:5000/<username>>-tekton/pipelines-vote-api` `!!! Change username before paste to input box !!!`

- leave default timeouts,

- set shared-workspace : PersistentVolumeClaim and select `source-pvc`

- click start
  
  ![](../images/6/6-16.png)

- Wait tekton start pipeline and run

  ![](../images/6/6-17.png)

- change to Logs tab to view output in each step. wait until status change to `Succeeded`

  ![](../images/6/6-18.png)

- Back to Topology view, check `pipelines-vote-api` deployment complete!
 
  ![](../images/6/6-21.png)

- Back to `build-and-deploy` pipeline, start it again

  ![](../images/6/6-22.png)

- In start pipeline,

- set deployment-name : `pipelines-vote-ui `
  
- set git-url : `https://github.com/openshift/pipelines-vote-ui.git`

- set git-revision : `master`

- set IMAGE : `image-registry.openshift-image-registry.svc:5000/<username>>-tekton/pipelines-vote-ui` `!!! Change username before paste to input box !!!`

- leave default timeouts,

- set shared-workspace : PersistentVolumeClaim and select `source-pvc`

- click start

  ![](../images/6/6-24.png)                

- View Pipeline run.

  ![](../images/6/6-25.png)   

- View Logs. wait until status change to `Succeeded`

  ![](../images/6/6-26.png) 

- View pipeline run history, go to Pipelines>Pipelines in left menu, select PipelineRuns tab,      

  ![](../images/6/6-28.png)  

- Back to Topology view, view `pipelines-vote-ui` deployment

  ![](../images/6/6-27.png)     

## Test Application

- Back to Topology view, 

- Click url from `pipelines-vote-ui`    

  ![](../images/6/6-29.png)     

  ![](../images/6/6-30.png)     

## Back to Table of Content

- [Red Hat OpenShift Service for AWS (ROSA) Workshop](../README.md)






