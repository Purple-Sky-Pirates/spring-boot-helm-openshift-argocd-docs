# Continuous Deployment with ArgoCD

A set of documentation to explain how to setup GitOps with ArgoCD on Openshift.

You will need:

- Container image(s) for your application
- A helm chart for your application (in order to deploy standalone without ArgoCD)
- A helm repository in order to store the helm application helm chart
- A helm chart for your argocd managed application deployment (in order to deploy your application into different environments)


### Application helm chart

This helm chart would contain everything required in order to deploy your application (i.e. your application solution topology as described by kubernetes manifests) into a cluster.  It is expected that this helm chart is under source control (git).

The expectation would be that this would contain:

- At least one deployment
- Service objects
- Persistent Volume Claims (if required)
- Routes or Ingress Objects
- A default values.yaml file

This helm chart should be tested as working irrespective of ArgoCD/GitOps.  This helm chart should ideally be available in a Helm Chart repository.

For the purposes of a demonstration the helm chart can be found at:
https://github.com/Purple-Sky-Pirates/spring-boot-helm-openshift-argocd-charts

### Helm chart repository

It is required that a helm chart repository is available and that the desired helm chart is packaged and available in the repository.

#### Nexus Setup

Initially using nexus (as per Red Hat Labs): https://github.com/redhat-cop/helm-charts/tree/master/charts/sonatype-nexus

https://nexus-nexus.apps.ocp1.purplesky.cloud/repository/purplesky-charts/

### ArgoCD Application helm chart

This helm chart would contain:

- An ArgoCD AppProject definition - a parent level of your application across namespaces
- An ArgoCD Application definition - one application definition per deployment
- A folder of environments, within each environment it is expected that a different values.yaml is defined for that environment configuration.

Given that this git repository contains secret information it is expected that this git repository would be locked down and only accessible by people with appropriate permissions (ssh-private key access).

Permissions need to be granted to the argocd-application-controller on order for it to seed the objects into that namespace, in this instance we are using `sb-staging` and `sb-test`, therefore we need to run the following:

```
oc policy add-role-to-user admin system:serviceaccount:stu-argocd:argocd-application-controller -n sb-staging
oc policy add-role-to-user admin system:serviceaccount:stu-argocd:argocd-application-controller -n sb-test
```


## TODOs:

- Installing ArgoCD into your own namespace.