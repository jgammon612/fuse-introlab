# Red Hat Agile API Integration Workshop - Microservices-based Managed API Integration


## Red Hat 3scale API Management Platform Account
This lab focuses on the deployment and administration of Red Hat 3Scale. One deployment topology available is known as a *hybrid* approach. A *hybrid* Red Hat 3Scale deployment topology is one in which the 3Scale API gateway is self-managed in your own environment. This self-managed API gateway is in two-way communication with the hosted Red Hat 3Scale SaaS API Management Platform (AMP).

![00-3scale-hybrid-deployment.png](./img/00-3scale-hybrid-deployment.png)

The focus of lab 04 of this course is on this hybrid deployment topology. Subsequently, the lab 04 make heavy use of your account in the Red Hat 3Scale SaaS AMP. If you do not have a Red Hat 3Scale SaaS, please register for free trial one at: https://www.3scale.net/signup/.

You will receive an email in your inbox to complete the signup process and activate your account.



## Installing and setup Container Development Kit

Under the folder where you installed the Development Suite, you will find a folder named **cdk** go to **${DEVSUITE_INSTALLTION_PATH}/cdk/components/rhel/rhel-ose/** edit file **Vagrantfile**

Find the IMAGE_TAG and configure the OCP version to v3.4, then save the file.

```
#Modify IMAGE_TAG if you need a new OCP version e.g. IMAGE_TAG="v3.3.1.3"
IMAGE_TAG="v3.4"
```

In a command line console, start up your local Openshift

```
vagrant up
```

Install and setup oc binary client

```
vagrant service-manager install-cli openshift
export PATH=${vagrant_dir}/data/service-manager/bin/openshift/1.4.0:$PATH
eval "$(VAGRANT_NO_COLOR=1 vagrant service-manager install-cli openshift  | tr -d '\r')"
```

Login as admin

```
oc login -u admin
Authentication required for https://10.1.2.2:8443 (openshift)
Username: admin
Password:
Login successful.

```

Install Fuse image stream on OpenShift and Database template for this lab

```
#FIS image
oc create -f https://raw.githubusercontent.com/jboss-fuse/application-templates/master/fis-image-streams.json -n openshift

#MYSQL Database
oc create -f https://raw.githubusercontent.com/openshift/origin/master/examples/db-templates/mysql-ephemeral-template.json -n openshift
```

log back in as developer

```
oc login -u openshift-dev
Authentication required for https://10.1.2.2:8443 (openshift)
Username: openshift-dev
Password:
Login successful.

```

Access OpenShift console by going to the following URL in the browser.

```
https://10.1.2.2:8443
```

Going back to Red Hat JBoss Developer Studio, in OpenShift Explorer view, click on **New Connection Wizard..** to configure OpenShift setting
Enter **https://10.1.2.2:8443** as the **Server** and click on the **retrieve** link to access the token.

![05-token.png](./img/05-token.png)

In the popup window, log in as Developer using ID/PWD openshift-dev/devel. Select ok and check the **Save token** box.

![06-connection.png](./img/06-connection.png)

## Windows Users

- Make sure you disable  Hyper-V functionality under Control Panel
- Add _config.ssh.insert\_key=false_ to **Vagrantfile** ${DEVSUITE_INSTALLATION_PATH}/cdk/components/rhel/rhel-ose/

Thanks to @sigreen

## FAQ
- How to install Maven?  
	- Go to https://maven.apache.org/install.html for detail instructions
- Maven dependency not found?
	- ${MAVEN_INSTALLED_DIR} if you are having trouble downloading from the repositories
