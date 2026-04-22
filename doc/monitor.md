# Basic ROSA Observability
<!-- TOC -->

- [Basic ROSA Observability](#basic-rosa-observability)
  - [OpenShift Default Monitoring](#openshift-default-monitoring)
  - [Set Resource Request/Limit and How to Compare Usage vs Resource Config](#set-resource-requestlimit-and-how-to-compare-usage-vs-resource-config)
  - [OpenShift Default Logging](#openshift-default-logging)
  - [Monitor Container Log with OpenShift Console](#monitor-container-log-with-openshift-console)
  - [Back to Table of Content](#back-to-table-of-content)

<!-- /TOC -->

## OpenShift Default Monitoring

- Back to Project: `<username>-lab1`

  ![](../images/3/3-1.png)

- view defalt monitoring per deployment, click topology, click duke icon (backend deployment), 

- test call url 5-6 times, (or open url and refresh 5-6 times)

  ![](../images/3/3-2.png)  

- in backend deployment side panel,  select observe tab
  
  - view CPU usage,

  ![](../images/3/3-3.png)    

  - view Memory usage (scroll down to view)

  ![](../images/3/3-4.png)  

- Repeat to view observe with `nodejs` and `hello` deployment
  
  ![](../images/3/3-5.png)  

- view Project Monitoring, click Observe in left menu, select Dashboard, select `Kubernetes/Compute Resources/Namespace(Pods)` in Dashboard dropdown list 
  
- this page will show Monitoring Information of current project (all resources in this project)

  ![](../images/3/3-6.png)  

  ![](../images/3/3-7.png)  

- scroll down to view CPU Usage, Memory Usage and Other Monitoring Data

  ![](../images/3/3-8.png)  

- For view metrics by type, go to Observe and select Metrics in left menu, view performance/metrics information by type, 

  ![](../images/3/3-9.png)  

- click select query dropdown list to select default metrics information such as cpu usage, memory usage, filesystem usage, etc. 
  
  ![](../images/3/3-10.png)  
  
- select CPU usage, view cpu usage of all pod/deployment

  ![](../images/3/3-11.png)

- delete this query by click three dot icon and select `Delete query`

  ![](../images/3/3-12.png)

- change to another metrics such as memory usage., review memory usage of all pods.

  ![](../images/3/3-13.png)

- delete query again, try to manual input prometheus query by yourselft, type `pod` in query box and wait auto suggestion. select `pod:container_cpu_usage:sum`

  ![](../images/3/3-14.png)

- click `staked`

  ![](../images/3/3-15.png)

## Set Resource Request/Limit and How to Compare Usage vs Resource Config

- Back to Topology view, select backend deployment

  ![](../images/3/3-16.png)

- click actions menu, select edit resource limits

  ![](../images/3/3-17.png)  

- set cpu request : 50 millicores, limit : 100 millicores

- set memory request : 256 MiB, limit : 512 MiB, click save

  ![](../images/3/3-18.png)  

- Wait until `backend` deployment restart complete!

- Try to call URL of `backend` 5-6 times, 

  ![](../images/3/3-19.png) 

- back to Observe>Dashboards, select `Kubernetes/Compute Resources/Namespace(Pods)` again

  ![](../images/3/3-20.png) 

- scroll down to view cpu quota, view cpu usage, cpu request, cpu request%, cpu limit, cpu limit%

  ![](../images/3/3-21.png) 

- scroll down to view memory quota, view memory usage, memory request, memory request%, memory limit, memory limit%

  ![](../images/3/3-22.png) 

## OpenShift Default Logging

- Example code with logging (backend application)

  - Code URL: https://raw.githubusercontent.com/chatapazar/openshift-workshop/main/src/main/java/org/acme/getting/started/BackendResource.java

  - Example Code Logging

    ```java
    import org.jboss.logging.Logger;
    ...
    private static final Logger logger = Logger.getLogger(BackendResource.class);
    ...
     URL url;
            try {
                logger.info("Request to: " + backend);
    ...
    ```

- example log property (backend application)

  - Properties URL: https://raw.githubusercontent.com/chatapazar/openshift-workshop/main/src/main/resources/application.properties

  - Example properties:

  ```prop
  #Logging
  quarkus.log.level=INFO
  # quarkus.log.category."com.example.quarkus".level=INFO
  # quarkus.log.category."com.example.quarkus.health".level=DEBUG
  quarkus.log.console.enable=true
  quarkus.log.console.format=%d{HH:mm:ss} %-5p [%c{2.}] (%t) %s%e%n
  quarkus.log.console.color=false
  %dev.quarkus.log.console.color=true
  ```

## Monitor Container Log with OpenShift Console

- go to web terminal

- test call backend service

  ```bash
  BACKEND_URL=https://$(oc get route backend -o jsonpath='{.spec.host}')
  curl $BACKEND_URL/backend
  ```

  ![](../images/3/3-24.png) 

- view log in pod, go to Topology, click Duke icon (backend), in backend deployment select Resources Tab, click 'View logs' of Pod

  ![](../images/3/3-23.png) 

- in pod details, select Logs tab to view log of container 'backend'

  ![](../images/3/3-25.png) 

- re call backend service and check log in pod append (retry call 2-3 times for view logs append)

    ```bash
    BACKEND_URL=https://$(oc get route backend -o jsonpath='{.spec.host}')
    curl $BACKEND_URL/backend
    ```

    check log append at log terminal

  ![](../images/3/3-26.png) 

- click raw icon to view log in another browser tab

  ![](../images/3/3-27.png) 

  ![](../images/3/3-28.png) 

- click download to download currnet log

   ![](../images/3/3-29.png) 

## Back to Table of Content

- [Red Hat OpenShift Service for AWS (ROSA) Workshop](../README.md)





