---
layout: post
autore: "Evangelista Tragni"
title: Install Tanzu User Managed Packages 
comments: false
category: blog
---

# Tanzu User Managed Packages Prerequisites

## Install yq tool

Install the following tool that you will need in the following steps.
* yq - yq is a command-line tool designed to transform YAML.



```bash
sudo snap install yq 
```



## Install Tanzu CLI

To download and unpack Tanzu CLI:

Go to https://my.vmware.com and log in with your My VMware credentials.

Visit the [Tanzu Kubernetes Grid downloads page](https://customerconnect.vmware.com/en/downloads/info/slug/infrastructure_operations_management/vmware_tanzu_kubernetes_grid/1_x)

In the VMware Tanzu Kubernetes Grid row, click `Go to Downloads`.

In the Select Version drop-down, select `1.5.1`.

Under Product Downloads, scroll to the section labeled VMware Tanzu CLI 1.5.1 CLI:
For This example , I will download the `VMware Tanzu CLI for Linux` and click Download Now.

In the Download folder, unpack the Tanzu CLI bundle file for your operating system. To unpack the bundle file, use the extraction tool of your choice. For example, the `tar -xvf` command.
```bash
tar -xvf tanzu-cli-bundle-linux-amd64.tar
```
Navigate to the cli subfolder under the tanzu folder that you unpacked in the previous section:
```bash
cd cli/
```
Confirm that the binary is executable by running the ls command.
```bash
ls
```

> ```bash
> cluster                                 management-cluster
> core                                    manifest.yaml
> imgpkg-darwin-amd64-v0.xx.0+vmware.1.gz package
> kapp-darwin-amd64-v0.xx.0+vmware.1.gz   pinniped-auth
> kbld-darwin-amd64-v0.xx.0+vmware.1.gz   vendir-darwin-amd64-v0.21.1+vmware.1.gz
> kubernetes-release                      ytt-darwin-amd64-v0.34.0+vmware.1.gz
> login
> ```

Install the binary to /usr/local/bin:

```bash
sudo install core/v0.11.1/tanzu-core-linux_amd64 /usr/local/bin/tanzu
```

At the command line in a new terminal, initialize the Tanzu CLI:
```bash
tanzu init
```
> ```bash
> Checking for required plugins...
> Installing plugin 'login:v0.11.1'
> Installing plugin 'management-cluster:v0.11.1'
> Installing plugin 'package:v0.11.1'
> Installing plugin 'pinniped-auth:v0.11.1'
> Installing plugin 'secret:v0.11.1'
> Successfully installed all required plugins
> ✔  successfully initialized CLI
> ```

At the command line in a new terminal, run tanzu version to check that the correct version of the CLI is properly installed:
```bash
tanzu version
```

> ```bash
> version: v0.11.1
> buildDate: 2022-02-14
> sha: 4d578570
> ```
 
## Install the Tanzu CLI Plugins

After you have installed the tanzu core executable, you must install the CLI plugins related to Tanzu Kubernetes cluster management and feature operations.

Navigate to the tanzu folder that contains the cli folder.
```bash
cd Downloads
```

Run the following command from the tanzu directory to install all the plugins for this release.
```bash
tanzu plugin sync
```

Check plugin installation status.
```bash
tanzu plugin list
```
If successful, you should see a list of all installed plugins. For example:

> ```bash
>   NAME                DESCRIPTION                                                        SCOPE       DISCOVERY  VERSION  STATUS
>   login               Login to the platform                                              Standalone  default    v0.11.1  installed
>   management-cluster  Kubernetes management-cluster operations                           Standalone  default    v0.11.1  installed
>   package             Tanzu package management                                           Standalone  default    v0.11.1  installed
>   pinniped-auth       Pinniped authentication operations (usually not directly invoked)  Standalone  default    v0.11.1  installed
>   secret              Tanzu secret management                                            Standalone  default    v0.11.1  installed
> ```

## Install the Carvel Tools

Carvel provides a set of reliable, single-purpose, composable tools that aid in application building, configuration, and deployment to Kubernetes.

Tanzu Kubernetes Grid uses the following tools from the Carvel open-source project:

* ytt - a command-line tool for templating and patching YAML files. You can also use ytt to collect fragments and piles of YAML into modular chunks for easy re-use.

* kapp - the applications deployment CLI for Kubernetes. It allows you to install, upgrade, and delete multiple Kubernetes resources as one application.

* kbld - an image-building and resolution tool.

* imgpkg - a tool that enables Kubernetes to store configurations and the associated container images as OCI images, and to transfer these images.

### Install Ytt

Open the cli folder:
```bash
cd Downloads/cli
```
Unpack the `ytt` binary and make it executable.
```bash
gunzip ytt-linux-amd64-v0.35.1+vmware.1.gz
chmod ugo+x ytt-linux-amd64-v0.35.1+vmware.1
```
Move the binary to `/usr/local/bin` and rename it to ytt:
```bash
mv ./ytt-linux-amd64-v0.35.1+vmware.1 /usr/local/bin/ytt
```
At the command line in a new terminal, run `ytt version` to check that the correct version of ytt is properly installed.
```bash
ytt version
```
> ```bash
> ytt version 0.35.1
> ```

### Install Kapp

Unpack the `kapp` binary and make it executable.
```bash
gunzip kapp-linux-amd64-v0.42.0+vmware.1.gz
chmod ugo+x kapp-linux-amd64-v0.42.0+vmware.1
```

Move the binary to `/usr/local/bin` and rename it to kapp:
```bash
mv ./kapp-linux-amd64-v0.42.0+vmware.1 /usr/local/bin/kapp
```
At the command line in a new terminal, run `kapp version` to check that the correct version of kapp is properly installed.
```bash
kapp version
```
> ```bash
> kapp version 0.42.0  
> Succeeded
> ```

### Install Kbld

Unpack the `kbld` binary and make it executable.
```bash
gunzip kbld-linux-amd64-v0.31.0+vmware.1.gz
chmod ugo+x kbld-linux-amd64-v0.31.0+vmware.1
```
Move the binary to `/usr/local/bin` and rename it to kbld:
```bash
mv ./kbld-linux-amd64-v0.31.0+vmware.1 /usr/local/bin/kbld
```
At the command line in a new terminal, run `kbld version` to check that the correct version of kbld is properly installed.
```bash
kbld version 
```
> ```bash
> kbld version 0.31.0
> 
> Succeeded
> ```

### Install Imgpkg

Unpack the imgpkg binary and make it executable.
```bash
gunzip imgpkg-linux-amd64-v0.18.0+vmware.1.gz
chmod ugo+x imgpkg-linux-amd64-v0.18.0+vmware.1
```
Move the binary to /usr/local/bin and rename it to imgpkg:
```bash
mv ./imgpkg-linux-amd64-v0.18.0+vmware.1 /usr/local/bin/imgpkg
```
At the command line in a new terminal, run imgpkg version to check that the correct version of imgpkg is properly installed.
```bash
imgpkg version
```
> ```bash
> imgpkg version 0.18.0
> 
> Succeeded
> ```

# User Managed Packages

A package is a collection of related software that supports or extends the core functionality of the Kubernetes cluster in which the package is installed.

- `Core packages` are automatically installed and managed by Tanzu Kubernetes Grid. These packages are located in the tanzu-core package repository.
- `User-managed` packages are installed and managed by you. These packages are located in the tanzu-standard package repository.

## Prepare Environment
Move the kubeconfig you downloaded on the .kube directory

sudo mv  kubeconfig  ~/.kube/config
> ```

Login on te `Tanzu CLI` the following is a guide line:
```bash
tanzu login --name <choose-a-name-for-supervisor-cluster> --kubeconfig ~/.kube/config --context kubernetes-admin@kubernetes
```
In my Test it is like this:

> ```bash
> tanzu login --name sharedsvc --kubeconfig ~/.kube/config  --context kubernetes-admin@kubernetes
> ```
> 
> ```bash
> ✔  successfully logged in to management cluster using the kubeconfig my-super
> ```
> 



Check if a default storage class is defined:
```bash
kubectl get storageclass
```

If no default storage class is listed, specify one.

Create a namespace to our packages:
```
kubectl create ns  tanzu-package-repo-global 
```

### Package Repo 

Use the tanzu package CLI plugin to add the standard packages repository to the Tanzu CLI, in the tanzu-package-repo-global namespace:

```bash
tanzu package repository add <REPOSITORY-NAME>  -n <NAMESPACE> --url projects.registry.vmware.com/tkg/packages/standard/repo:v1.5.0
```
In my example:

```bash
tanzu package repository add repo-standard -n tanzu-package-repo-global --url projects.registry.vmware.com/tkg/packages/standard/repo:v1.5.0
```
>```bash
> - Adding package repository 'repo-standard'...
>  Added package repository 'repo-standard'
>```

Verify that the repository was installed:

```bash
tanzu package repository list
```

>```bash
> - Retrieving repositories...
>   NAME           REPOSITORY                                                      STATUS               DETAILS
>   repo-standard  projects.registry.vmware.com/tkg/packages/standard/repo:v1.5.0  Reconcile succeeded
>```

Verify that packages from the repository are available:

```bash
tanzu package available list -A
```

Your version could be different, this is only an exaple of output:

> ```bash
>   NAME                           DISPLAY-NAME  SHORT-DESCRIPTION                                                                                           LATEST-VERSION         NAMESPACE
>   cert-manager.tanzu.vmware.com  cert-manager  Certificate management                                                                                      1.5.3+vmware.2-tkg.1   tanzu-package-repo-global
>   contour.tanzu.vmware.com       contour       An ingress controller                                                                                       1.18.2+vmware.1-tkg.1  tanzu-package-repo-global
>   external-dns.tanzu.vmware.com  external-dns  This package provides DNS synchronization functionality.                                                    0.10.0+vmware.1-tkg.1  tanzu-package-repo-global
>   fluent-bit.tanzu.vmware.com    fluent-bit    Fluent Bit is a fast Log Processor and Forwarder                                                            1.7.5+vmware.2-tkg.1   tanzu-package-repo-global
>   grafana.tanzu.vmware.com       grafana       Visualization and analytics software                                                                        7.5.7+vmware.2-tkg.1   tanzu-package-repo-global
>   harbor.tanzu.vmware.com        harbor        OCI Registry                                                                                                2.3.3+vmware.1-tkg.1   tanzu-package-repo-global
>   multus-cni.tanzu.vmware.com    multus-cni    This package provides the ability for enabling attaching multiple network interfaces to pods in Kubernetes  3.7.1+vmware.2-tkg.2   tanzu-package-repo-global
>   prometheus.tanzu.vmware.com    prometheus    A time series database for your metrics                                                                     2.27.0+vmware.2-tkg.1  tanzu-package-repo-global
>   ```



## Deploy Cert-Manager on Tanzu Cluster 

Confirm that the cert-manager package is available in your workload cluster:
```bash
tanzu package available list cert-manager.tanzu.vmware.com -A
```


Install the `Cert Manager package`:

```bash
tanzu package install cert-manager --package-name cert-manager.tanzu.vmware.com --namespace tanzu-package-repo-global --version 1.1.0+vmware.1-tkg.2
```

>```bash
> - Installing package 'cert-manager.tanzu.vmware.com'
> | Getting namespace 'tanzu-package-repo-global'
> / Getting package metadata for 'cert-manager.tanzu.vmware.com'
> | Creating service account 'cert-manager-tanzu-package-repo-global-sa'
> | Creating cluster admin role 'cert-manager-tanzu-package-repo-global-cluster-role'
> | Creating cluster role binding 'cert-manager-tanzu-package-repo-global-cluster-rolebinding'
> - Creating package resource
> - Package install status: Reconciling
> 
> 
>  Added installed package 'cert-manager' in namespace 'tanzu-package-repo-global'
>```

Confirm that the `cert-manager` package has been installed:
```bash
tanzu package installed list -A
```
>```bash
> - Retrieving installed packages...
>   NAME          PACKAGE-NAME                   PACKAGE-VERSION       STATUS
>   cert-manager  cert-manager.tanzu.vmware.com  1.1.0+vmware.1-tkg.2  Reconcile succeeded
>```

The cert-manager package and its resources, such as the cert-manager app, are installed in the namespace that you specify when running the tanzu package install command.



If the status is not Reconcile Succeeded, view the full status details of the cert-manager app. Viewing the full status can help you to troubleshoot the problem.


Confirm that the `cert-manager-pods` are running:
```bash
kubectl get pods -A
```
> ```bash
> NAMESPACE      NAME                                                        READY   STATUS    RESTARTS   AGE
> cert-manager   cert-manager-78897c8dc5-pfh7s                               1/1     Running   0          2m21s
> cert-manager   cert-manager-cainjector-86cdb8577c-nrr2s                    1/1     Running   0          2m21s
> cert-manager   cert-manager-webhook-ff45bc699-k8vdd                        1/1     Running   0          2m21s
> ```

The Cert Manager pods and any other resources associated with the Cert Manager component are created in the cert-manager namespace.

---

## Deploy Contour on Tanzu Cluster

After you have set up the cluster, you must first create the configuration file that is used when you install the Contour package and then install the package.

Download a configuration file named `contour-data-values.yaml`.:
```bash
wget https://raw.githubusercontent.com/etragni/contour/main/contour-data-values.yaml
```

Retrieve the version of the available package:

```bash
tanzu package available list contour.tanzu.vmware.com -A
```
>```
> - Retrieving package versions for contour.tanzu.vmware.com...
>   NAME                      VERSION                RELEASED-AT                     NAMESPACE
>   contour.tanzu.vmware.com  1.17.1+vmware.1-tkg.1  2021-07-23 20:00:00 +0200 CEST  tanzu-package-repo-global
>```

Remove all comments in the contour-data-values.yaml file:
```bash
yq -i eval '... comments=""' contour-data-values.yaml
```
Install the package:
```
tanzu package install contour \
--package-name contour.tanzu.vmware.com \
--version 1.17.1+vmware.1-tkg.1 \
--values-file contour-data-values.yaml \
--namespace tanzu-package-repo-global
```
>```
> - Installing package 'contour.tanzu.vmware.com'
> | Getting namespace 'tanzu-package-repo-global'
> / Getting package metadata for 'contour.tanzu.vmware.com'
> | Creating service account 'contour-tanzu-package-repo-global-sa'
> | Creating cluster admin role 'contour-tanzu-package-repo-global-cluster-role'
> | Creating cluster role binding 'contour-tanzu-package-repo-global-cluster-rolebinding'
> | Creating secret 'contour-tanzu-package-repo-global-values'
> - Creating package resource
> / Package install status: Reconciling
> 
> 
>  Added installed package 'contour' in namespace 'tanzu-package-repo-global'
>```

Confirm that the `contour` package has been installed:
```
tanzu package installed list -A
```



Confirm that Contour and Envoy are running in the tanzu-system-ingress namespace:
```bash
kubectl get pods -A
```

>```bash
> ...
> tanzu-system-ingress           contour-56bbb95977-ch946                                                          1/1     Running   0          6m27s
> tanzu-system-ingress           contour-56bbb95977-gm7xd                                                          1/1     Running   0          6m27s
> tanzu-system-ingress           envoy-669ct                                                                       2/2     Running   0          6m28s
> tanzu-system-ingress           envoy-jhjs5                                                                       2/2     Running   0          6m28s
> tanzu-system-ingress           envoy-nm8hw                                                                       2/2     Running   0          6m28s
> ...
>```


## Deploy Fluent Bit on a Tanzu Kubernetes Cluster
Download the manifest from 
```
wget https://raw.githubusercontent.com/etragni/contour/main/fluent-bit-data-values-example.yaml
```
```
tanzu package available list fluent-bit.tanzu.vmware.com -A
```

>```
> | Retrieving package versions for fluent-bit.tanzu.vmware.com...
>   NAME                         VERSION               RELEASED-AT                     NAMESPACE
>   fluent-bit.tanzu.vmware.com  1.7.5+vmware.1-tkg.1  2021-05-13 20:00:00 +0200 CEST  tanzu-package-repo-global
>```

Retrieve the template from the package itself by following the procedure described in Retrieve the Data Values Template.
```bash
image_url=$(kubectl -n tanzu-package-repo-global get packages fluent-bit.tanzu.vmware.com.1.7.5+vmware.1-tkg.1 -o jsonpath='{.spec.template.spec.fetch[0].imgpkgBundle.image}')
```

```bash
echo $image_url
```
>```
> projects.registry.vmware.com/tkg/packages/standard/fluent-bit@sha256:c83d038c57f244aae2819dd77dc5184bb3e1ec96524d3f6a09fe8a244b7bc9e4
>```

Run imgpkg with image_url to retrieve the package image bundle:

```bash
imgpkg pull -b $image_url -o /tmp/fluentbit-1.7.5+vmware.1-tkg.1

cp /tmp/fluentbit-1.7.5+vmware.1-tkg.1/config/values.yaml fluent-bit-data-values.yaml
```


Make any changes needed to your `fluent-bit-data-values.yaml` file based on the example file tat you downloaded (modify output section ad filter section).

#### Remember to not use the example file because the startng manifest could be different on your version. 


remove all comments in it:
```bash
yq -i eval '... comments=""' fluent-bit-data-values.yaml
```
Run tanzu package install to deploy the package:
```
tanzu package install fluent-bit \
--package-name fluent-bit.tanzu.vmware.com \
--version 1.7.5+vmware.1-tkg.1 \
--values-file fluent-bit-data-values.yaml \
--namespace tanzu-package-repo-global
```
>```
> \ Installing package 'fluent-bit.tanzu.vmware.com'
> | Getting namespace 'tanzu-package-repo-global'
> / Getting package metadata for 'fluent-bit.tanzu.vmware.com'
> | Creating service account 'fluent-bit-tanzu-package-repo-global-sa'
> | Creating cluster admin role 'fluent-bit-tanzu-package-repo-global-cluster-role'
> | Creating cluster role binding 'fluent-bit-tanzu-package-repo-global-cluster-rolebinding'
> | Creating secret 'fluent-bit-tanzu-package-repo-global-values'
> - Creating package resource
> - Package install status: Reconciling
> 
> 
>  Added installed package 'fluent-bit' in namespace 'tanzu-package-repo-global'
>```
Confirm that the fluent-bit package is installed. For example:
```
tanzu package installed list -A
```



## Deploy Prometheus into the Workload Cluster

Prometheus is an open-source systems monitoring and alerting toolkit. Tanzu Kubernetes Grid includes signed binaries for Prometheus that you can deploy on Tanzu Kubernetes clusters to monitor cluster health and services.

Retrieve the version of the available package:
```
tanzu package available list prometheus.tanzu.vmware.com -A
```
>```
> \ Retrieving package versions for prometheus.tanzu.vmware.com...
>   NAME                         VERSION                RELEASED-AT                     NAMESPACE
>   prometheus.tanzu.vmware.com  2.27.0+vmware.1-tkg.1  2021-05-12 20:00:00 +0200 CEST  tanzu-package-repo-global
>```


### Deploy Prometheus with Custom Values

To install the Prometheus package using user-provided values:

Retrieve the template of the Prometheus package’s tanzu-package-repo-global configuration:

```bash
image_url=$(kubectl -n tanzu-package-repo-global get packages prometheus.tanzu.vmware.com.2.27.0+vmware.1-tkg.1 -o jsonpath='{.spec.template.spec.fetch[0].imgpkgBundle.image}')
```
```bash
imgpkg pull -b $image_url -o /tmp/prometheus-package-2.27.0+vmware.1-tkg.1
```
>```bash
> Pulling bundle 'projects.registry.vmware.com/tkg/packages/standard/prometheus@sha256:27af034c1c77bcae4e1f7f6d3883286e34419ea2e88222642af17393cd34e46a'
>   Extracting layer 'sha256:44798ebd112b55ea792f0198cf220a7eaed37c2abc531d6cf8efe89aadc8bff2' (1/1)
> 
> Locating image lock file images...
> One or more images not found in bundle repo; skipping lock file update
> 
> Succeeded
>```

move the values file:
```
cp /tmp/prometheus-package-2.27.0+vmware.1-tkg.1/config/values.yaml prometheus-data-values.yaml
```

This creates a configuration file named `prometheus-data-values.yaml` that you can modify. See Retrieve the Data Values Template for more about this sequence of commands.

For information about configuration parameters to use in `prometheus-data-values.yaml`, see Prometheus Package Configuration Parameters below.


Change the following value:
**false to true** 

Change the fqdn with your DOMAIN RECORD 

>```yaml
> ingress:
>   enabled: `true`
>   virtual_host_fqdn: "prometheus.corp.tanzu"
>   prometheus_prefix: "/"
>   alertmanager_prefix: "/alertmanager/"
>   prometheusServicePort: 80
>   alertmanagerServicePort: 80
> ...
>```


After you make any changes needed to your prometheus-data-values.yaml file, remove all comments in it:
```bash
yq -i eval '... comments=""' prometheus-data-values.yaml
```

Deploy the package:


```
tanzu package install prometheus \
--package-name prometheus.tanzu.vmware.com \
--version 2.27.0+vmware.1-tkg.1 \
--values-file prometheus-data-values.yaml \
--namespace tanzu-package-repo-global
```
>```
> | Installing package 'prometheus.tanzu.vmware.com'
> | Getting namespace 'tanzu-package-repo-global'
> / Getting package metadata for 'prometheus.tanzu.vmware.com'
> | Creating service account 'prometheus-tanzu-package-repo-global-sa'
> | Creating cluster admin role 'prometheus-tanzu-package-repo-global-cluster-role'
> | Creating cluster role binding 'prometheus-tanzu-package-repo-global-cluster-rolebinding'
> / Creating secret 'prometheus-tanzu-package-repo-global-values'
> - Creating package resource
> \ Package install status: Reconciling
>```

```
tanzu package installed list -A
```


### Note if you have proble with prometheus pod to start (write permission on volume)

ADD Chown Pod :


```
vi pod-prometheus-fix.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: prometheus-fix-volume
  namespace: tanzu-system-monitoring
  labels:
spec:
  volumes:
  - name: storage-volume
    persistentVolumeClaim:
      claimName: prometheus-server
  containers:
  restartPolicy: OnFailure
  - name: fix-container
    image: ubuntu
    imagePullPolicy: IfNotPresent
    command: ["/bin/sh"]
    args: ["-c", "chown -R 65534:65534 /data && echo 'true'"]
    volumeMounts:
    - mountPath: /data
      name: storage-volume
```
 then delete the prometheus deployment:
 
```
kubectl delete deploy -n tanzu-system-monitoring prometheus-server
```

now apply the pod to change permission of the volume:

```
kubectl apply -f pod-prometheus-fix.yaml
```
when thwe pod is running and complete, the deployment of prometheus pod will be ready.


Create a DNS record to map `prometheus.system.tanzu` to the External IP address of the Envoy load balancer.

```
kubectl get svc -n tanzu-system-ingress
```
>```
> NAMESPACE                 NAME                            TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
> tanzu-system-ingress      contour                         ClusterIP      198.54.13.185    <none>        8001/TCP                     18h
> tanzu-system-ingress      envoy                           LoadBalancer   198.54.213.239   10.10.1.9     80:31187/TCP,443:31903/TCP   18h
>```

Access the Prometheus dashboard by navigating to your fqdn in a browser.

![prometheus](/Images/tanzu-packages/prometheus.png)


## Deploy Grafana on Tanzu Kubernetes Clusters

```
tanzu package available list grafana.tanzu.vmware.com -A
```

> ```
> \ Retrieving package versions for grafana.tanzu.vmware.com...
>   NAME                      VERSION               RELEASED-AT                     NAMESPACE
>   grafana.tanzu.vmware.com  7.5.7+vmware.1-tkg.1  2021-05-19 20:00:00 +0200 CEST  tanzu-package-repo-global
> ```

Retrieve the template of the Grafana package’s tanzu-package-repo-global configuration:

```
image_url=$(kubectl -n tanzu-package-repo-global get packages grafana.tanzu.vmware.com.7.5.7+vmware.1-tkg.1 -o jsonpath='{.spec.template.spec.fetch[0].imgpkgBundle.image}')
```

```
imgpkg pull -b $image_url -o /tmp/grafana-package-7.5.7+vmware.1-tkg.1

cp /tmp/grafana-package-7.5.7+vmware.1-tkg.1/config/values.yaml grafana-data-values.yaml
```


This creates a configuration file named `grafana-data-values.yaml` that you can modify. See Retrieve the Data Values Template for more about this sequence of commands.


Edit `grafana-data-values.yaml` and replace secret. 

**admin_password** with a Base64-encoded password. 

To generate a Base64 encoded password, run:
In my example i use this standart password, you need to change it
```
echo -n 'mypassword' | base64
```
```
mypassword results in the encoded password bXlwYXNzd29yZA==
```

Modify Also the fqdn like you did for Prometheus with your DOMAIN RECORD

**(Optional)** Modify the Grafana datasource configuration in `grafana-data-values.yaml`. Grafana is configured with Prometheus as a default data source.

**(Optional)** If you have customized the Prometheus deployment namespace and it is not deployed in the default namespace, tanzu-system-monitoring, you need to change the Grafana datasource configuration in `grafana-data-values.yaml`.



After you make any changes needed to your grafana-data-values.yaml file, remove all comments in it:

```
yq -i eval '... comments=""' grafana-data-values.yaml
```

Deploy the package:

If the target namespace exists in the cluster, run:

```
tanzu package install grafana \
--package-name grafana.tanzu.vmware.com \
--version 7.5.7+vmware.1-tkg.1 \
--values-file grafana-data-values.yaml \
--namespace tanzu-package-repo-global
```

>```
> tanzu package install grafana --package-name grafana.tanzu.vmware.com --version 7.5.7+vmware.1-tkg.1 --values-file grafana-data-values.yaml --namespace tanzu-package-repo-global
> \ Installing package 'grafana.tanzu.vmware.com'
> | Getting namespace 'tanzu-package-repo-global'
> / Getting package metadata for 'grafana.tanzu.vmware.com'
> | Creating service account 'grafana-tanzu-package-repo-global-sa'
> | Creating cluster admin role 'grafana-tanzu-package-repo-global-cluster-role'
> | Creating cluster role binding 'grafana-tanzu-package-repo-global-cluster-rolebinding'
> | Creating secret 'grafana-tanzu-package-repo-global-values'
> - Creating package resource
> \ Package install status: Reconciling
> 
>  Added installed package 'grafana' in namespace 'tanzu-package-repo-global'
>```


After you deploy Grafana, you can verify that the deployment is successful:

Confirm that the grafana package is installed. For example:
```
tanzu package installed list -A
```

### Note if you have proble with Grafana pod to start (write permission on volume)

ADD Chown Pod :


```
vi pod-grafana-fix.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: grafana-fix-volume
  namespace: tanzu-system-dashboards
  labels:
spec:
  volumes:
  - name: storage-volume
    persistentVolumeClaim:
      claimName: grafana-pvc
  containers:
  restartPolicy: OnFailure
  - name: fix-container
    image: ubuntu
    imagePullPolicy: IfNotPresent
    command: ["/bin/sh"]
    args: ["-c", "chown -R 472:472 /var/lib/grafana && echo 'true'"]
    volumeMounts:
    - mountPath: /var/lib/grafana
      name: storage-volume
  restartPolicy: OnFailure
```
 then delete the grafana deployment:
 
```
kubectl delete deploy -n tanzu-system-dashboards grafana
```

now apply the pod to change permission of the volume:

```
kubectl apply -f pod-grafana-fix.yaml
```
when the pod is running and complete, the deployment of grafana pod will be ready.


The grafana package and its resources, such as the grafana app, are installed in the namespace that you specify when running the tanzu package install command.


Confirm that the new services are running by listing all of the pods that are running in the cluster:
```
kubectl get pods -A
```


After Grafana is deployed, the grafana package creates a Contour HTTPProxy object with a Fully Qualified Domain Name (FQDN) that you specified on the grafana-data-values.yaml.

Create an entry in your DNS that points the External IP address to this FQDN

To use this FQDN to access the Grafana dashboard:

```
Navigate to your fqdn
```

Logn with :
```
Username : admin
Password : mypassword
```

![graphana](/Images/tanzu-packages/graphana.png)


## Deploy Harbor Registry as a Shared Service

Harbor is an open-source, trusted, cloud-native container registry that stores, signs, and scans content. Harbor extends the open-source Docker distribution by adding the functionalities usually required by users such as security and identity control and management

You can use the Harbor shared service as a private registry for images that you want to make available to all of the workload clusters that you deploy from a given management cluster. An advantage to using the Harbor shared service is that it is managed by Kubernetes, so it provides greater reliability than a stand-alone registry.
```
tanzu package available list harbor.tanzu.vmware.com -A
```

>```
> | Retrieving package versions for harbor.tanzu.vmware.com...
>   NAME                     VERSION               RELEASED-AT                     NAMESPACE
>   harbor.tanzu.vmware.com  2.3.3+vmware.1-tkg.1  2021-07-07 20:00:00 +0200 CEST  tanzu-package-repo-global
>```
Create a configuration file named harbor-data-values.yaml: 

```
image_url=$(kubectl -n tanzu-package-repo-global get packages harbor.tanzu.vmware.com.2.3.3+vmware.1-tkg.1 -o jsonpath='{.spec.template.spec.fetch[0].imgpkgBundle.image}')
```

```
imgpkg pull -b $image_url -o /tmp/harbor-package-2.3.3
```


```bash
cp /tmp/harbor-package-2.3.3/config/values.yaml harbor-data-values.yaml
```

Set the mandatory passwords and secrets in the `harbor-data-values.yaml` file by doing one of the following:

```bash
bash /tmp/harbor-package-2.3.3/config/scripts/generate-passwords.sh harbor-data-values.yaml
```
This willpopulate all your file with random passwords:

>```
> Successfully generated random passwords and secrets in harbor-data-values.yaml
>```

Modify the harbor-data-values.yaml


Set the hostname setting to the hostname (fqdn) you want to use to access Harbor. For example, harbor.yourdomain.com:

>```yaml
> #! The FQDN for accessing Harbor admin UI and Registry service.
> hostname: harbor.xxxxxxxxxxxxx
>```

If you used the generate-passwords.sh script, optionally update the harborAdminPassword with something that is easier to remember:

>```yaml
> #! Required The initial password of Harbor admin.
> harborAdminPassword: ##########
>```


Remove all comments in the `harbor-data-values.yaml` file:

```
yq -i eval '... comments=""' harbor-data-values.yaml
```

Install the package:

```
tanzu package install harbor \
--package-name harbor.tanzu.vmware.com \
--version 2.3.3+vmware.1-tkg.1 \
--values-file harbor-data-values.yaml \
--namespace tanzu-package-repo-global
```

>```
> \ Installing package 'harbor.tanzu.vmware.com'
> | Getting namespace 'tanzu-package-repo-global'
> / Getting package metadata for 'harbor.tanzu.vmware.com'
> | Creating service account 'harbor-tanzu-package-repo-global-sa'
> | Creating cluster admin role 'harbor-tanzu-package-repo-global-cluster-role'
> | Creating cluster role binding 'harbor-tanzu-package-repo-global-cluster-rolebinding'
> | Creating secret 'harbor-tanzu-package-repo-global-values'
> - Creating package resource
> | Package install status: Reconciling
>```

### Note 
Patch the Harbor package with another overlay as follows, to create initContainers objects that can set directory ownership and permissions:

Create a file fix-fsgroup-overlay.yaml containing the code below:

```bash
wget https://raw.githubusercontent.com/etragni/contour/main/fix-fsgroup-overlay.yaml
```


Create a generic secret with the overlay:
```
kubectl -n tanzu-package-repo-global create secret generic harbor-database-redis-trivy-jobservice-registry-image-overlay -o yaml --dry-run=client --from-file=fix-fsgroup-overlay.yaml | kubectl apply -f -
```

>```
> secret/harbor-database-redis-trivy-jobservice-registry-image-overlay created
>```

Patch the Harbor package with the secret:
```
kubectl -n tanzu-package-repo-global annotate packageinstalls harbor ext.packaging.carvel.dev/ytt-paths-from-secret-name.1=harbor-database-redis-trivy-jobservice-registry-image-overlay
```

>```
> packageinstall.packaging.carvel.dev/harbor annotated
>```
Remove all the Harbor pod:
```
kubectl delete deploy -n tanzu-system-registry --all
kubectl delete sts -n tanzu-system-registry --all

```

Verify that the package is reconciling again
```
tanzu package installed list -A
```

>```
> \ Retrieving installed packages...
>   NAME          PACKAGE-NAME                   PACKAGE-VERSION        STATUS               NAMESPACE
>   cert-manager  cert-manager.tanzu.vmware.com  1.1.0+vmware.1-tkg.2   Reconcile succeeded  tanzu-package-repo-global
>   contour       contour.tanzu.vmware.com       1.17.1+vmware.1-tkg.1  Reconcile succeeded  tanzu-package-repo-global
>   fluent-bit    fluent-bit.tanzu.vmware.com    1.7.5+vmware.1-tkg.1   Reconcile succeeded  tanzu-package-repo-global
>   grafana       grafana.tanzu.vmware.com       7.5.7+vmware.1-tkg.1   Reconcile succeeded  tanzu-package-repo-global
>   harbor        harbor.tanzu.vmware.com        2.2.3+vmware.1-tkg.1   Reconciling          tanzu-package-repo-global
>   prometheus    prometheus.tanzu.vmware.com    2.27.0+vmware.1-tkg.1  Reconcile succeeded  tanzu-package-repo-global
>```


Obtain the Harbor CA certificate from the harbor-tls secret in the tanzu-system-registry namespace:

```
kubectl -n tanzu-system-registry get secret harbor-tls -o=jsonpath="{.data.ca\.crt}" | base64 -d
```

> ```
> -----BEGIN CERTIFICATE-----
> ##################################################################
>##################################################################
> -----END CERTIFICATE-----
> ```

Obtain the address of the Envoy service load balancer.

```
kubectl get svc -n tanzu-system-ingress
```


If you deployed Harbor on a shared services cluster that is running on vSphere, you must add the External IP to fqdn mapping on the DNS or add corresponding A records in your DNS server

Users can now connect to the Harbor UI by navigating to 
```
Navigate to your fqdn
```

![harbor](/Images/tanzu-packages/harbor-1.png)


in a Web browser and log in as user admin with the harborAdminPassword that you configured in harbor-data-values.yaml.


![harbor-2](/Images/tanzu-packages/harbor-2.png)

If Harbor uses a self-signed certificate, download the Harbor CA certificate from
```
 https://<fqdn>/api/v2.0/systeminfo/getcert
```


---
Evangelista Tragni

Desotech srl
