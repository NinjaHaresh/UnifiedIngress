# UnifiedIngress
Config files for my video demo of Unified Ingress Architecture

You can clone these files easily by going to your home folder and typing:
> git clone https://github.com/NinjaHaresh/UnifiedIngress 

A new sub-folder will be created called UnifedIngress with the files.

This is the repository of Kubernetes config files to deploy the Citrix Unified Gateway configuration shown in my demo on youtube located here: https://youtu.be/pUP-VEM8MlM

This architecture is described at the following links:
Single-Tier (or Unified Ingress) Diagram:
https://developer-docs.citrix.com/projects/citrix-k8s-ingress-controller/en/latest/deployment-topologies/#single-tier-topology

This Single-Tier design is discussed in the larger Citrix Validated Reference Design here:
https://docs.citrix.com/en-us/advanced-concepts/design-guides/cloud-native-networking-openshift-validated-reference-design.html

Citrix CIC (Citrix Ingress Controller) GitHub repository is the source of the CIC files:
https://github.com/citrix/citrix-k8s-ingress-controller

The files are listed here for your convenience. These are listed in the order you should deploy them.
5 files are part of this deployment. 
Each file can be deployed using kubectl apply -f <filename> -n namespace

app.yaml
  
service.yaml

rbac.yaml

cic.yaml

ingress.yaml

Before deploying the ingress.yaml, I would suggest you open an SSH connection to the external VPX and run the following to tail the ns.log. This will show you all the commands the CIC shoots to the VPX


shell

cd /var/log

tail -f ns.log

Now go ahead and deploy the ingress. Follow instructions in the docs for this architecture to see the results. View my 14 min video to see this in action which should answer many of your questions.

I'll add more here later on what you should expect after deploying each file above.
