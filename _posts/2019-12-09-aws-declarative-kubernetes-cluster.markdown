---
layout: post
title:  "“Kubeforming”: going full declarative with Kubernetes and AWS"
date:   2019-12-09 02:14:02 -0300
categories: automation declarative iac aws
---

> **Notice:** this is a reproduction of my post originally published on [In Loco Tech Blog](https://medium.com/inlocotech/kubeforming-going-full-declarative-with-kubernetes-and-aws-2507a90f683b)

![Photo by Micheile Henderson]({{ "/assets/2019-12-09/funny_ship.jpg" | absolute_url }})

**Declarative infrastructures** are a dream come true for any infra team. Having your cloud environment bootstrapped by an **IaC** (Infra as Code) tool, such as *Terraform*, means even more than gains in speed or cost reduction: it ensures that the knowledge required to get your systems up and running is crystallized in a functional blueprint, rather than sitting around on pages of perhaps-some-step-is-missing documentations.

Those tools have already reached an astonishing level of maturity. We encourage them to be the default approach to building cloud infrastructure in your organization.

Among IaC tools, the most adopted ones are the so-called **declarative**: you specify the desired state, and the system itself defines and executes the needed steps to achieve it.

In this post, we’ll show how we leveraged three declarative tools (*Terraform*, *eksctl*, and *Argo CD*) to deliver fully working and standardized Kubernetes clusters to our product teams.

### Step 0: preparing the cloud for your cluster

AWS provides a managed service for Kubernetes clusters called Amazon EKS.

However, EKS does not live in isolation. It interacts with some other cloud services that we need to prepare prior to declaring your EKS Control Plane.

![]({{ "/assets/2019-12-09/terraform_manages.png" | absolute_url }})

*Terraform* receives a handful of parameters and provisions for us:

* **Amazon VPC** private and public subnets on our VPC, across multiple AZs (availability zones), and utility security groups;
* **Amazon Route53** hosted zones to be managed by the cluster’s _external DNS_;
* A **CodeCommit** repo to store cluster backups of our Kubernetes resources.

With a blueprint, no more than a few parameters are needed to have these services properly configured: the name of the cluster itself, subnet positioning in the private IP range, and desired domains to be managed by Route53.

*Terraform* outputs the state, including IDs, of the resources it just provisioned. They become parameters for the next step.

### Step 1: bringing that shiny new k8s cluster up

Amazon has _recently released_ managed EKS node groups. However, they still can’t give us all the features that we may achieve while managing our own workers.

When these node groups reach a higher level of maturity, we will be able to bootstrap the entire cluster with Terraform, without having an extra layer of complexity. While that day hasn’t come, we are more than happy with our tool of choice, _eksctl_.

![how adorable :3]({{ "/assets/2019-12-09/eksctl.png" | absolute_url }})

*eksctl* is capable of interacting (via AWS CloudFormation) with EKS, so it bootstraps the control plane for you, and with AWS EC2, so it also makes sure your worker nodes are running with your desired configuration.

With _eksctl_, we have been able to configure a surprising range of workers flavors — including elastic spot instance groups to run Spark jobs, and database clusters leveraging i3’s local storage.

### Step 2: delivering add-ons to all clusters

A brand new Kubernetes cluster is capable of many things, but it would be a mistake to consider it production-ready.

Production applications need a **supporting infra**: Prometheus should be up scraping metrics, Filebeat harvesting logs, ingress controllers making sure services are being exposed, and much more.

We call these supporting infra **add-ons**. They are delivered like any regular Kubernetes app: by uploading manifests to the Kubernetes API. Sounds declarative enough, huh?

However, what if we had a repo of what our ideal “In Loco cluster” looks like, and have this target configuration continuously delivered to all of our clusters?
In such a manner, we would eliminate the toil of ensuring our add-ons are on the same version across clusters. Also, we could use this very process to deliver the same level of configuration to a newly created cluster.

_*Argo CD*_ can help us to accomplish this vision.

![]({{ "/assets/2019-12-09/argocd_deploys.png" | absolute_url }})

*Argo CD* offers a **pull-based** CD model, where an agent watches for changes in an artifacts repository and delivers them to the target system.

Add-ons may also be customizable from cluster to cluster, by writing _Kustomize_ patches, such that we can achieve standardization without sacrificing flexibility completely.

### Conclusion

Delivering and managing multiple clusters don’t have to feel like dying. With this pipeline, we could deploy and maintain our Kubernetes infra as code.

At the same time, we could maintain a uniform configuration across all clusters, which makes maintainability far easier.

Of course, there are some hidden details which glue all these steps together, so feel free to email me if you got stuck on implementing this :)

Happy provisioning!