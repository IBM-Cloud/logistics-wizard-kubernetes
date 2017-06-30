# Logistics Wizard with Kubernetes

This repository is part of the larger [Logistics Wizard](https://github.com/IBM-Bluemix/logistics-wizard) project.

:warning: WORK IN PROGRESS

## Overview

In this variation of the Logistics Wizard deployment, the ERP and Controller services are deployed as containers within a Kubernetes cluster. The web user interface stays deployed as a Cloud Foundry application.

## Prepare your Kubernetes cluster

1. Have a Kubernetes cluster

1. Install Istio from https://istio.io/docs/tasks/installing-istio.html

   ```
   kubectl apply -f install/kubernetes/istio-rbac-alpha.yaml
   kubectl apply -f install/kubernetes/istio.yaml
   kubectl apply -f install/kubernetes/addons/prometheus.yaml
   kubectl apply -f install/kubernetes/addons/grafana.yaml
   kubectl apply -f install/kubernetes/addons/servicegraph.yaml
   ```

## Deploy the toolchain in IBM Bluemix

1. Ensure you have a [GitHub](https://github.com/) account.

1. Ensure you have a [Bluemix](https://ibm.com/bluemix) account.

1. Ensure you have 2GB of free memory and space for 4 additional services in your Bluemix organization.

1. It is recommended to create a new space named `logistics-wizard-kube` in your Bluemix organization. This helps grouping the apps and services together in the console.

1. **To get started, click this button:**

   [![Deploy To Bluemix](https://console.ng.bluemix.net/devops/graphics/create_toolchain_button.png)](https://console.ng.bluemix.net/devops/setup/deploy/?repository=https://github.com/IBM-Bluemix/logistics-wizard-kubernetes&branch=master)

1. The toolchain configuration page opens.

1. Give a name to your toolchain. This name is used as the prefix for the deployed applications. The name must be unique within your organization.

  > A simple trick is to use your name or initials or to leave the default generated name.

1. Select the GitHub box.

1. Optionally change the name of the GitHub repositories that will be created in your GitHub account.

  > You can choose to fork the main repositories so that you can import changes as they become available or to clone the repositories.

1. Select the Delivery Pipeline box.

1. Give a unique name to the Logistics Wizard web application

1. Select the region, organization and space where you want to deploy the application.

   :warning: Make sure the organization and the space have no space in their names.

   :warning: Only the US South region is currently supported.

1. Select the region, organization and space where quota for IBM Containers have been specified. If you never used the IBM Containers before, you may need to initialize one space with IBM Container quotas.

1. Specify a Bluemix API key. You can obtain one using `bx iam api-key-create my-new-key` or from the Bluemix console under Manage / Security / Bluemix API Keys. The API key is used to interact with the Bluemix Container Service.

1. Specify the name of an existing Kubernetes cluster where you have installed Istio.

1. Click Create

1. Once the toolchain is created, you will end up with several delivery pipelines for all applications in the sample. Wait for the pipelines to deploy all the apps. You can open all pipelines in new browser tabs to follow the deployments or you can wait for all apps to be ready in the Bluemix dashboard.

1. Your apps are deployed. Head over the [walkthrough](https://github.com/IBM-Bluemix/logistics-wizard/blob/master/WALKTHROUGH.md) for a tour of the app.

## Using the Istio addons

### Grafana

The Grafana addon provides an Istio dashboard visualization of the metrics (request rates, success/failure rates) in the cluster.

   ```
   kubectl port-forward $(kubectl get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000
   ```

   http://localhost:3000/dashboard/db/istio-dashboard

### dotviz

   ```
   kubectl port-forward $(kubectl get pod -l app=servicegraph -o jsonpath='{.items[0].metadata.name}') 8088:8088
   ```

   http://localhost:8088/dotviz


### Uninstall Istio

   ```
   kubectl delete -f install/kubernetes/istio.yaml
   kubectl delete -f install/kubernetes/istio-rbac-beta.yaml
   kubectl delete -f install/kubernetes/addons/
   kubectl delete istioconfigs --all
   kubectl delete thirdpartyresource istio-config.istio.io
   ```
