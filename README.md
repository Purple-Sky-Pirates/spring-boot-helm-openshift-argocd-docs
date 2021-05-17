# Continuous Deployment with ArgoCD

A set of documentation to explain how to setup GitOps with ArgoCD on Openshift.

## Continuous Deployment with ArgoCD

You will need:

- Container image(s) for your application
- A helm chart for your application (in order to deploy standalone without ArgoCD)
- A helm chart for your argocd application deployment (in order to deploy your application into different environments)


### Application helm chart

This helm chart would contain everything required in order to deploy your application (i.e. your application solution topology as described by kubernetes manifests) into a cluster.  It is expected that this helm chart is under source control (git).

The expectation would be that this would contain:

- At least one deployment
- Service objects
- Persistent Volume Claims (if required)
- Routes or Ingress Objects
- A default values.yaml file

This helm chart should be tested as working irrespective of ArgoCD/GitOps.  This helm chart should ideally be available in a Helm Chart repository.

## Continuous Deployment with ArgoCD

This helm chart would contain:

- An ArgoCD AppProject definition - a parent level of your application across namespaces
- An ArgoCD Application definition - 
- A folder of environments, within each environment it is expected that a different values.yaml is defined for that environment configuration.

Given that this git repository contains secret information it is expected that this git repository would be locked down and only accessible by people with appropriate permissions (ssh-private key access).


## TODOs:

- Installing ArgoCD into your own namespace.
- Granting ArgoCD permissions to your project namespaces.

https://github.com/redhat-cop/helm-charts/tree/master/charts/argocd-operator

