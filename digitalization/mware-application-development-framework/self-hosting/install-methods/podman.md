# Podman

Run Mware Application Development Framework using Podman as the container engine. The usage is very similar to Docker but we are adding notes here for any caveats.

#### Red Hat Enterprise Linux

Testing on RHEL 8.7 and using Podman version 4.2.0 deploying it with podman is similar to the docker command:

```
mkdir -p /data/...
podman run -p 80:80 -v /data/....:/data:Z -e INTERNAL_API_KEY=changeme -e JWT_SECRET=changeme  \
-e MINIO_ACCESS_KEY=minio -e MINIO_SECRET_KEY=changeme -e REDIS_PASSWORD=changeme \
-e COUCHDB_USER=admin -e COUCHDB_PASSWORD=changeme --name ....-d registry.hub.docker.com/.../
```

The `:Z` option on the volume asks Podman to relabel the volume content to match the label inside the container.

#### Debian 11

At the time of writing Debian 11 supplies version 3 of Podman in the official repositories. We have experienced some networking issues with this version which manifests as an inability to load the App templates.

For Debian 11 the example below uses podman packages from alvistack to install version 4.3.1-1 and netavark for networking:

```
source  /etc/os-release
wget http://downloadcontent.opensuse.org/repositories/home:/alvistack/Debian_$VERSION_ID/Release.key -O alvistack_key
cat alvistack_key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/alvistack.gpg  >/dev/null
echo "deb http://downloadcontent.opensuse.org/repositories/home:/alvistack/Debian_$VERSION_ID/ /" | sudo tee  /etc/apt/sources.list.d/alvistack.list
apt update
apt install podman python3-podman-compose podman-netavark 
```

For convenience configure podman to check the docker registry by default. Create the file:

```
/etc/containers/registries.conf
```

And add the following:

```
unqualified-search-registries = ['docker.io']

[[registry]]  
prefix = "docker.io"  
location = "docker.io"
```

Then create the file:

```
/etc/containers/containers.conf
```

Adding the following:

```
[network]  
network_backend = "netavark"
```

Create the data directory and run the container:

```
mkdir -p /data/...

podman run -p 80:80 -v /data/...:/data -e INTERNAL_API_KEY=changeme -e JWT_SECRET=changeme  
-e MINIO_ACCESS_KEY=minio -e MINIO_SECRET_KEY=changeme -e REDIS_PASSWORD=changeme  
-e COUCHDB_USER=admin -e COUCHDB_PASSWORD=changeme --name ...-d .../...
```
