# Kubernetes

There are a few command-line utilities we have to set up before we can get started with Mware Application Development Framework on Kubernetes. Follow the guides below to set up Kubectl and Helm.

* [helm CLI](https://helm.sh/docs/intro/install/)
* [kubectl CLI](https://kubernetes.io/docs/tasks/tools/#kubectl)

> ### 📘Info
>
> We recommend running on Kubernetes nodes with at least 1GB of memory, but we recommend larger instances for higher volume use cases.

***

### Setting up a Kubernetes Cluster

If you don't already have an existing Kubernetes cluster, follow one of the guides below for your provider.

* [AWS guide](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)
* [Google Cloud guide](https://cloud.google.com/kubernetes-engine/docs/how-to)
* [Minikube](https://minikube.sigs.k8s.io/docs/start/)
* [K3S](https://rancher.com/docs/k3s/latest/en/quick-start/)
* [MicroK8S](https://microk8s.io/docs)

Depending on what provider you use, you may also need to install an [Ingress controller](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/) and a [Storage provisioner](https://kubernetes.io/docs/concepts/storage/storage-classes/).

Here are some good default choices for each.

**Ingress controllers:**

* If you're on AWS EKS, the [AWS load balancer controller](https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html) is a good choice. It provisions AWS Application Load Balancers for your Ingress resources.
* Google Cloud has the [HttpLoadBalancer add-on.](https://cloud.google.com/kubernetes-engine/docs/how-to/load-balance-ingress)
* For anything else, the [ingress-nginx](https://kubernetes.github.io/ingress-nginx/) project should work well.

**Storage provisioners:**

* If you're on AWS EKS, the [EBS CSI driver](https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html) can manage provisioning EBS volumes for your `PersistentVolumeClaim` resources.
* Google Cloud have the [Compute Engine persistent disk CSI driver.](https://cloud.google.com/kubernetes-engine/docs/how-to/persistent-volumes/gce-pd-csi-driver)
* Homelab clusters might consider using something like the [NFS provisioner](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner).

If you want to make use of `HorizontalPodAutoscaler` resources you will also need to install [metrics-server](https://github.com/kubernetes-sigs/metrics-server) into your cluster.

***

### Install Helm Chart

Now that you have your Kubernetes cluster up and running, you can install the Helm chart which will provision all of the relevant services for running it in a Kubernetes environment. Run the commands below to download the Helm chart from our repository, and install it.

Shell

```shell
helm repo add ...
helm repo update
helm install --create-namespace --namespace ....
```

Wait a few moments as it creates all the containers and resources. You can then run:

Shell

```shell
kubectl get pods -n 
```

You should now be able to see your new installation up and running in your Kubernetes cluster.

```
NAME                              READY   STATUS             RESTARTS         AGE
app-service-69b7888b7-h25cp       1/1     Running            0                90m
couchdb-0                         1/1     Running            0                90m
minio-service-59c757bb5-q6xt2     1/1     Running            0                90m
proxy-service-6449dd9fdf-v5r6z    1/1     Running            0                90m
redis-service-c6f59dcf5-lh7v5     1/1     Running            0                90m
worker-service-55b49f4cdc-qr5b7   1/1     Running            0                90m
```

### Configuring your Ingress resource

To use your new installation, you'll need to configure an Ingress resource. One is configured by default, but there are some extra steps required to get it working depending on your cluster.

#### AWS EKS

If you're running on AWS EKS, we ship an ALB-ready Ingress resource as part of the chart. You'll need to disable the deafult Ingress and enable the ALB one:

Shell

```shell
helm upgrade -n ...  --set ingress.enabled=false --set awsAlbIngress.enabled=true
```

After a few minutes your new Ingress resource should be provisioned and you should be able to get the address for it like so:

Shell

```shell
$ kubectl --context qa -n ... get ingress
NAME               CLASS    HOSTS   ADDRESS                                                                 PORTS   AGE
ingress-.... <none>   *       4372843243278.eu-west-1.elb.amazonaws.com   80      9m5s
```

Visit the Ingress address in your browser and you will see that your installation is up and running.

<figure><img src="https://files.readme.io/694ff03-CleanShot_2023-12-05_at_13.59.34.png" alt="Budibase admin interface" width="375"><figcaption><p>admin interface</p></figcaption></figure>

If you'd like to set up HTTPS, you can also create a certificate using [AWS ACM](https://aws.amazon.com/certificate-manager/) and specify the certificate ARN on your Ingress resource:

Shell

```shell
helm upgrade -n .... --set ingress.enabled=false --set awsAlbIngress.enabled=true --set awsAlbIngress.certificateArn=...
```

From here, if you'd like to use a custom domain name make sure it's a CNAME that points to Ingress address and that your certificate includes the custom domain name.

#### ingress-nginx

If you're using `ingress-nginx` in your cluster, you'll want to configure an Ingress something like this (replacing `example.com` with whatever custom domain you would like to point at your installation):

YAML

```yaml
ingress:
  enabled: true
  className: "nginx"
  hosts:
    - host: "example.com"
      paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: proxy-service
              port:
                number: 10000
```

Save this as a file called `values.yaml` and then upgrade your Helm release:

Shell

```shell
helm upgrade -n -f values.yaml
```

Now find the external address if your `ingress-nginx` installation by running:

Shell

```shell
❯ kubectl -n ingress-nginx get services | grep LoadBalancer
ingress-nginx-controller                   LoadBalancer   10.99.103.243    192.168.0.90   80:32221/TCP,443:32172/TCP   2y20d
```

If you're running a homelab with, for example, [metallb](https://metallb.org/), your `ingress-nginx` `LoadBalancer` will be exposed on an IP address in your local network as above (`192.168.0.90`). If you're running `ingress-nginx` in EKS, you'll see an AWS load balancer domain name in this column instead.

If `ingress-nginx` is exposed with an IP address, create an `A` record in your DNS service of choice and point your domain at it (or an `AAAA` record if it's IPv6). If it's a domain name, create a `CNAME` record instead.

### Setting up persistent storage

#### AWS EKS

First, make sure you've followed the steps to [install the Amazon EBS CSI driver](https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html). After you've done that, add the following to your `values.yaml` to enable persistent storage for all of the services that need it:

YAML

```yaml
couchdb:
  persistentVolume:
    enabled: true
    storageClass: "gp3"
    
services:
  objectStore:
    storageClass: "gp3"
  redis:
    storageClass: "gp3"
```

And then update your installation:

```
helm upgrade -n .... -f values.yaml
```

#### GCP GKE

Follow the [guide to enable Compute Engine persistent disk](https://cloud.google.com/kubernetes-engine/docs/how-to/persistent-volumes/gce-pd-csi-driver) in your GKE cluster. When you've done that, add the following to your `values.yaml` to enable persistent storage for all of the services that need it:

```
couchdb:
  persistentVolume:
    enabled: true
    storageClass: "standard-rwo"
   
services:
  objectStore:
    storageClass: "standard-rwo"
  redis:
    storageClass: "standard-rwo"
```

And then update your installation:

```
helm upgrade -n ... -f values.yaml
```

### Upgrading your version

To upgrade to the latest version via Helm, you can run the following command:

Shell

```shell
helm repo update
helm upgrade -n ... --reuse-values
```

***

### Configuration

#### Scaling&#x20;

You can scale up the number of pods in your installation by updating the `replicaCount` of a given service in the `values.yaml` of the helm chart. For example, if we wanted to scale up the worker and app service due to high load, we can update our `values.yaml` to have a higher `replicaCount`. Once updated, upgrade your chart to apply the changes.

YAML

```yaml
services:
  proxy:
    replicaCount: 1
  apps:
    replicaCount: 2
  worker:
    replicaCount: 2
```

#### Secrets Management

If you have createSecrets set to true in your `values.yaml`, it  will create the following credentials for you:

* An internal API key, that can be used for API requests.
* a JWT secret
* Object store access key (if using MinIO)
* Object store secret key (if using MinIO)\
  If you need to read the value of your secrets, you can do so using `kubectl` and the following commands to read the values out from your k8s secrets:

Shell

```shell
# for internal API key
kubectl get secret ... -o go-template='{{ .data.internalApiKey }}' -n ...| base64 --decode

# JWT secret
kubectl get secret ... -o go-template='{{ .data.jwtSecret }}' -n ...| base64 --decode

# MinIO Access Key
kubectl get secret ... -o go-template='{{ .data.objectStoreAccess }}' -n ...| base64 --decode

# MinIO Secret Key
kubectl get secret ... -o go-template='{{ .data.objectStoreSecret }}' -n ...| base64 --decode
```

#### Redis

The Helm chart ships with a [Redis](https://redis.io/) server that will be included by default. If you want to use your own external Redis cluster, you can configure the `values.yaml` file in the helm chart to switch off the one by turning enabled off. Here's what your configuration may look like if you wanted to disable the default bundled Redis and use an external Redis cluster hosted on `myrediscluster.io`.

yaml

```yaml
  services:
    redis:
      enabled: false
      port: 6379
      url: "myrediscluster.io"
      password: "your-redis-password"
```

#### CouchDB

The Helm chart will automatically bring up a one-node CouchDB cluster within your Kubernetes cluster. If you would rather use an existing CouchDB cluster, you can turn off the one it supplies and point at your own. Please note - you must set up search in your [CouchDB cluster](https://docs.couchdb.org/en/stable/ddocs/search.html) in order to use all the search functionality that it provides. Below is an example configuration of how you would point it to a CouchDB installation hosted on `mycouch.io`:

YAML

```yaml
  services:
    couchdb:
      enabled: false
      url: "http://mycouch.io:1234"
      user: "couchuser"
      password: "couchpassword"
```

#### MinIO/Amazon S3

Mware Application Development Framework ships with a MinIO server included for object storage. Since MinIO is Amazon S3 compliant, you can switch out the bundled MinIO for an S3 bucket in your AWS account. Here's how your **values.yaml** should look if you want to use S3 instead of MinIO.

YAML

```yaml
services:  
  objectStore:
    minio: false
    accessKey: "your-access-key" # AWS_ACCESS_KEY
    secretKey: "your-secret-key" # AWS_SECRET_ACCESS_KEY
```

#### Reference

For documentation on all other options available for your `values.yaml` file, you can go to the [chart README on GitHub](https://github.com/Budibase/budibase/tree/master/charts/budibase).

***

### Troubleshooting

If you need a hand or have discovered a bug, please raise a discussion on our GitHub discussions forum. For Kubernetes installations, try to include the following information in your discussion:

* Which Kubernetes provider you are using (EKS/GKE/MiniKube etc.)
* Screenshots/Logs of errors that are occurring
* A copy of your `values.yaml` if you have changed any configuration (please be careful not to share anything sensitive, like passwords!)
* Anything else that is relevant to the issue you are having.
