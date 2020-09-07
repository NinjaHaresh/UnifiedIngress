
## Welcome to Haresh's Repository for the Unified Ingress (or Single-Tier ADC) project!
Hi, my name is Haresh Advani. This is the repository for the config files for my video demo of Unified Ingress Architecture

Update 2020-02 - corrected files to work for K8s v1.16 & 1.17

You can clone these files easily by going to a terminal on your linux master node, cd to your home folder and type:
>#### git clone https://github.com/NinjaHaresh/UnifiedIngress 

A new sub-folder will be created called UnifedIngress with the files. If you already have this folder, please either rename the old folder, or you can use this command to clone to a different folder

>#### git clone https://github.com/NinjaHaresh/UnifiedIngress  UnifiedIngressFolder2

This is the repository of Kubernetes config files to deploy the Citrix Unified Gateway configuration shown in my demo on youtube located here:
This YouTube video is part of my Kubernetes101 YouTube channel (Please subscribe if you like my content & smash the 'like' button)
#### https://youtu.be/pUP-VEM8MlM  (14 min demo video)

This architecture is described at the following links:
Single-Tier (or Unified Ingress) Diagram:
https://developer-docs.citrix.com/projects/citrix-k8s-ingress-controller/en/latest/deployment-topologies/#single-tier-topology

This Single-Tier design is discussed in the larger Citrix Validated Reference Design here:
https://docs.citrix.com/en-us/advanced-concepts/design-guides/cloud-native-networking-openshift-validated-reference-design.html

Citrix CIC (Citrix Ingress Controller) GitHub repository is the source of the CIC files:
https://github.com/citrix/citrix-k8s-ingress-controller

The files are listed here for your convenience. These are listed in the order you should deploy them.
5 files are part of this deployment. 
Each file can be deployed using :
> #### kubectl apply -f filename.yaml -n namespace
Remember these files have some variables you have to customize for your environment. I've written the needed changes below next to each filename.
                    
File Name  | Changes you need to make before deploying this
------------- | -------------
app.yaml      | no changes, but you can change # of replicas if you like
service.yaml  | no changes needed 
rbac.yaml     | two fields for 'namespace' must match your intended namespace. They are coded for a namespace called unified
cic.yaml      | change 172.16.16.74 to your NSIP (NetScaler's Mgmt IP or NetScaler's SNIP with mgmt enabled, if using an HA Pair), Change "nsroot" to your NetScaler admin username but keep it in double quotes, change 2nd "nsroot" to the NetScaler admin password of NetScaler VPX but keep it in double quotes. Note: passwords should be kept in K8s secrets, but this is just a lab so I've kept it simple here.
ingress.yaml |change 172.16.16.75 to your desired VIP IP in your lab network. In this simple beginner lab, the IP is hard coded, but in real use, we use the Citrix IPAM controller to issue an IP from a pre-determined pool of VIP IPs.

To Deploy these files, this is the order in which I deployed them:

```on Master node:
kubectl create ns unified
kubectl -n unified create -f rbac.yaml  #note the namespace within this file must match the namespace we just created
kubectl -n unified create -f app.yaml
kubectl -n unified create -f service.yaml
kubectl -n unified create -f cic.yaml
```

Before deploying the ingress.yaml, I would suggest you open an SSH connection to the external VPX and run the following to tail the ns.log. This will show you all the commands the CIC shoots to the VPX

```On NetScaler:
shell
cd /var/log
tail -f ns.log
```
Now go ahead and deploy the ingress. 
```on Master node:
kubectl -n unified create -f ingress.yaml
```
Immediately keep an eye on the tail -f of the /var/log/ns.log on the NetScaler CLI to see the commands being shot into the NetScaler by the CIC. 

View my 14 min video to see this in action which should answer many of your questions.
I'll add more here later on what you should expect after deploying each file above.
