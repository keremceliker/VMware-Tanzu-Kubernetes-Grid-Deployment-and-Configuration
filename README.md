# Quick Deployment VMware Tanzu Kubernetes Grid using Docker Container !
*Written by Kerem ÇELİKER*
- Twitter: **`@CloudRss`**
- Linkedin: **`linkedin.com/in/keremceliker`**
- Blog: **`www.keremceliker.com`**

The vSphere 7 with Kubernetes and Tanzu Kubernetes Grid via TKG, which VMware has built into VCF 4.0 (VMware Cloud Foundation), were introduced in mid-April 2020 in accordance with vSphere 6.7, vSphere 7. It provides the Kubernetes environment, which PKS provides at a basic level, to manage the vSphere with Kubernetes very easily centrally from within vCenter. Also VCF 4.0 and vSphere 7 can be built-in vCenter 7 where vSphere with Kubernetes. 

The most important thing is DevOps & DevCloud teams can create their Kubernetes clusters and work on their Kubernetes clusters through the CLI isolated from vCenter with the authority they want to have..

In this article, I will show you how to install "VMware Tanzu Kubernetes Grid (Pivotal) and Integrate into VMware ESXi Infrastructure with Kubernetes.


  ![aws-diagram](images/3.JPG)
  
  

## Pre-Requisites

- Ubuntu or RedHat Linux Server (I prefer to have it but you can go on Windows10 if you wonder)
- VMware Tanzu "TKG-CLI" will be installed
- Govc CLI needs to be configured it.
- KubeCtl always have to be installed.
- Docker Container version 18.09 must be installed in your Linux or Windows build
	*As part of the TKG Cli deployment, it creates a OnPrem Kubernetes Cluster on the VMware Guest and uses it to attach the boot-strap to the target Tanzu Kubernete Grid.
- VMware vSphere ESXi 6.7
- Python3 Tool
- Putty
- PuttyGen

## Instructions on VMware Tanzu Kubernetes Grid using Kubernetes & Pivotal Concepts

1- The following needs to be Have and Downloaded:
```
Photon v3 Kubernetes v1.17.3 OVA File for Linux
**********
Photon v3 Capv Haproxy v0.6.3 OVA File for Linux
```

  ![aws-diagram](images/3.JPG)
  
  

2- *Install Govc for Linux :*
```
wget https://github.com/vmware/govmomi/releases/download/v0.22.1/vc_linux_amd64.gz
root@keremcontrollervm:/home/keremc# gzip -d govc_linux_amd64.gz
root@keremcontrollervm:/home/keremc# chmod +x govc_linux_amd64
root@keremcontrollervm:/home/keremc# mv govc_linux_amd64 /usr/local/bin/govc
```

3- *Install Tanzu TKG Tool for Linux :*

wget https://github.com/vmware/govmomi/releases/download/tkg-linux-amd64-v1.0.0_vmware.1.gz

**Follow the above same-steps as Govc

  ![aws-diagram](images/3.JPG)
  

4- Create Docker Container on Your Linux VM for the Tanzu TKG installation on the vCenter vSphere for Boot-Strap Process;
```
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
$ apt-cache madison docker-ce
$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io (Choose 5.18 or 5.19, It's up to you)
$ sudo docker run hello-world (for Verify to correctly running Docker Contaianer Services)
```

  ![aws-diagram](images/3.JPG)



5- *Create a Blank-Text with *.json script file :*
```
root@keremcontrollervm:/home/keremc# cat keremvc.gov
export GOVC_URL=https://192.168.1.3
export GOVC_USERNAME=administrator@vsphere.local
export GOVC_PASSWORD=KeremCeliker2020-!
export GOVC_INSECURE=true
export GOVC_DATASTORE=/Datacenter/datastore/K8sForNAS
export GOVC_RESOURCE_POOL='*/Resources'
export GOVC_DATACENTER=Datacenter
```

6- *Let's make pre-setup to connect to VMware vCenter and create Json script on it:*
```
 govc import.spec photon-3-v1.17.3_vmware.2.ova | jq '.Name="photon-3-v1.17.3_vmware.2"' | jq '.NetworkMapping[0].Network="DR-Overlay"' > photon-3-v1.17.3_vmware.1.json
```

7- Import the Photon OVA file to vCenter by JSON script;
```
govc import.ova -options=photon-3-v1.17.3_vmware.2.json photon-3-v1.17.3_vmware.2.ova
```

  ![aws-diagram](images/3.JPG)

8- Must be also complete on Proxy OVA File with the same-step;


Don't forget do tag follows below as OVA template with GOVC CLI
```
govc vm.markastemplate photon-3-v1.17.3
govc vm.markastemplate photon-3-capv-haproxy-v0.6.3
```

9- Extract Tanzu TKG CLI file move to under path /usr/loca/bin/ 
```
mv tkg-linux-amd64-v1.0.0_vmware.1 /usr/local/bin/tkg
```

10- Use SSH connection with the following tunneling by Putty to start a Tanzu Kubernetes GUI for Browser
```
tkg init --ui -v 6
```

11- And now You are ready to Install VMware Tanzu Kubernetes Grid Page from Chrome/FireFox
```
http://127.0.0.1/#/ui
```


  ![aws-diagram](images/3.JPG)


12- Go with Step by Step Deploy Management Cluster on vSphere  ;

- Set your IaaS Provider Informations as SSH Key,Username-Password.

- Select and Input your Control-Plane, Instance Type as Development or Prod, MGMT Cluster Name and Load-Balancer API Server
   
   *Just skip Production Section for now :-) Ha-Proxy OVA template that will do as the load balancer for management plane on Kubernetes.
   
     ![aws-diagram](images/3.JPG)
	

- Consider your VM-Resources Pool as Pool-Name with Path and Folder/DataStore

- Make sure Kubernetes run in the correct Network VXLAN and Adapter as Pod/Worker Nodes

  ![aws-diagram](images/3.JPG)
  

- Choose your specify the OS Template or ISO (You can quick-skip it this step with Next)

13- Please Review your all Tanzu & Kubernetes Configurations and Deploy & Enjoy ! :)


14- Let's Show and Check Tanzu Kubernetes Cluster in VMware vSphere
```
tkg get management-cluster
kubectl get nodes
```
  ![aws-diagram](images/3.JPG)
  
  
15-As Straight-Forward we will create a cool small Hello-World page on the VMware Tanzu K8s Grid for Test.
```
tkg create cluster Hello-Tanzu-From-Kerem --plan prod --kubernetes-version v1.18.3_vmware.2 --controlplane-machine-count 1 --worker-machine-count 1
```

- Let's take a short-look what's going on Kubernetes Cluster Inside.
```
kubectl get nodes
tkg get cluster
```

  ![aws-diagram](images/3.JPG)
  
  


References: 

