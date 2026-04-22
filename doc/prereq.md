# Prerequisite for workshop (Instructor Only)

- OCP 4.20

## Install Operator

- Serverless (create knative-serving in project knative-serving)
- Web Terminal
- deploy test from https://github.com/chatapazar/openshift-workshop/tree/main/sample
- sclae 5
- openshift pipelines

## Scale OpenShift Console before start lab

- edit Console object, name: Cluster in openshift-console-operator namespace

```yaml
spec:
  managementState: Unmanaged
```

- in openshift-console namespace, edit console deployment, remove nodeselector, scale replica > 3, set cpu/memory