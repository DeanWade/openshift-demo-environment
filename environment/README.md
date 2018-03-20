# OpenShift Demo Environment for Dynatrace

The following combination of `Vagrantfile`, `provision.sh` and `up.sh` files allows you to quickly set up an OpenShift Origin cluster inside a virtual machine that will expose applications to the outside world. The `provision.sh` script will be executed by the `Vagrantfile` (but can also be used standalone) to prepare the environment (as of now based on Ubuntu 16.04). Subsequently, `up.sh` installs an [OpenShift Origin](https://github.com/openshift/origin) release from GitHub and pre-configures the cluster with a variety of sample applications, ready for 1-click deployments.

## Prerequisites
### Supported Operating Systems

The demo environment can be set up directly on Windows 10, or on a virtual machine using Ubuntu 16.04. At the moment it was not sucessful when using the most current Ubuntu version 17.10.  

### Vagrant

[Vagrant](https://www.vagrantup.com/) is a convenience layer on top of virtualization technologies, such as VirtualBox, VMware and others, and has to be installed on your machine. The `Vagrantfile` assumes that you'll be using [VirtualBox](https://www.virtualbox.org/), which is another preqrequisite.

## How to deploy?
Preparation: when using Ubuntu 16.04 as operating system, prepare the environment by executing the `provision.sh` script.

Using the `vagrant up` command, Vagrant will automatically spawn a `ubuntu/xenial64` Vagrant box from [vagrantbox.es](http://www.vagrantbox.es/). 
Once the virtual machine has been launched, run `vagrant ssh` on your terminal to connect into the virtual machine via SSH.

### 1. Apply your settings
#### Bind OpenShift to a public IP and hostname

Once inside the virtual machine, determine its public IP address, e.g. via `ifconfig` and set its value to the `OS_PUBLIC_IP` environment variable in a terminal:

```
export OS_PUBLIC_IP="1.2.3.4"
```

In many cloud environments, such as Amazon EC2 or Google's GCE, you'll need to determine the machine's public IP through your specific cloud provider. If you want to run in such an environment, you'll need to provide your OpenShift machine's public hostname via the `OS_PUBLIC_HOSTNAME` environment variable, too:

```
export OS_PUBLIC_HOSTNAME="openshift.acmeco.com"
```

#### Deploy OneAgent (optional)

In addition to running OpenShift, you'll want to have the demo environment equipped with Dynatrace OneAgent. There are two ways to accomplish this: 
a faster way especially for this demo by applying the following `ONEAGENT_INSTALLER_SCRIPT_URL` environment variable, which has to be taken from the *Deploy Dynatrace / Start Installation / Linux* screen in Dynatrace:

![OneAgent Installation](https://github.com/dynatrace-innovationlab/openshift-demo-environment/raw/images/oneagent-installation.png)

```
export ONEAGENT_INSTALLER_SCRIPT_URL="https://abcdefgh.live.dynatrace.com/..."
```

The other, more common way is to deploy the Dynatrace OneAgent on the OpenShift Container Platform as described in [Dynatrace help](https://www.dynatrace.com/support/help/cloud-platforms/openshift/how-do-i-deploy-dynatrace-oneagent-on-openshift-container-platform/).


#### Speed up application startup times (optional)

You can speed up the deployment of the included sample applications by pre-populating OpenShift's internal container registry with the Docker images required by these applications by setting the `OS_PULL_DOCKER_IMAGES` environment variable to `true`:

```
export OS_PULL_DOCKER_IMAGES="true"
```

### 2. Install and run OpenShift

After you've defined all relevant options, installing and running OpenShift on the machine is as simple as running `up.sh`:

```
./up.sh
```

### 3. Connect to OpenShift

With OpenShift up and running, you can connect to OpenShift from both inside and outside the virtual machine using the `oc login` command or open the OpenShift Web UI in a browser. Both username and password are set to `developer`):

```
oc login https://1.2.3.4:8443 -u developer -p developer --insecure-skip-tls-verify
```

![OpenShift Web UI: Login](https://github.com/dynatrace-innovationlab/openshift-demo-environment/raw/images/openshift-web-ui-login.png)
