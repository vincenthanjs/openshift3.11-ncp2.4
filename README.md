# How to install Openshift Container Platform OCP with NSX-T NCP
[Home Page](https://github.com/vincenthanjs/openshift3.11-ncp2.4)



The last few blog posts I wrote about the installation steps for Openshift Container Platform (OCP) with NSX-T NCP attracted some good interest from the community as well as VMware internal folks. However, those materials were written quite awhile back and some of software used then were not up to date. My customers were also looking at the later versions of software. Lastly, in OCP 3.11, the ansible playbooks for NSX-T NCP integration comes out of the box and therefore makes the integration much simpler. Therefore, gave the reason to write this blog post.

The high level steps remains unchanged. However, the part 5 in this case has been streamline into the Openshift installation.

# Table Of Contents
[Openshift with NSX-T Installation Part 1 Overview](https://github.com/vincenthanjs/openshift3.11-ncp2.4)

[Openshift with NSX-T Installation Part 2: NSX-T](https://github.com/vincenthanjs/openshift3.11-ncp2.4)

[Openshift with NSX-T Installation Part 3: RHEL Preparation](https://github.com/vincenthanjs/openshift3.11-ncp2.4)

[Openshift with NSX-T Installation Part 4: Openshift Installation](https://github.com/vincenthanjs/openshift3.11-ncp2.4)

[Openshift with NSX-T Installation Part 5: NCP and CNI Integration (Combine into Part 4)](https://github.com/vincenthanjs/openshift3.11-ncp2.4)

[Openshift with NSX-T Installation Part 6: Demo App](https://github.com/vincenthanjs/openshift3.11-ncp2.4)


** For fellow VMware colleagues, to save you time for preparing the RHEL templates and VMs for OCP install, I have exported out the VMs from my Lab. I have uploaded in onedrive. Email me, I will happily share the link to download. Size is about 7GB.

![](2019-10-28-19-35-05.png)



Components:

    Compute – vSphere 6.7+ (vCenter Server + ESXi) and Enterprise Plus license
    Storage – VSAN or other vSphere Datastores
    Networking & Security – NSX-T 2.3
    Openshift Container Platform 3.11 Enterprise
    RHEL 7.6

Software Download:

Here is the complete list of software that needs to be downloaded to deploy Openshift Container Platform and NSX-T.
Software 	Download URL
NSX-T 	nsx-unified-appliance-2.3.0.0.0.10085405.ova (From 2.2 onwards, you can deploy NSX-T Controllers and Edges from the NSX-T Manager)
https://my.vmware.com/web/vmware/details?productId=673&downloadGroup=NSX-T-230
nsx-container-2.3.2.11695762.zip https://my.vmware.com/web/vmware/details?productId=673&downloadGroup=NSX-T-230#drivers_tools

![](2019-10-28-19-34-07.png)

RHEL 	https://access.redhat.com/downloads/
The version I used: rhel-server-7.6-x86_64-dvd.iso

 ##Openshift Installation & NSX-T NCP Integration

    1. On every node, install docker.
<pre><code>
        yum install docker-1.13.1
</code></pre>
    2. On the master node or jumphost, run the pre-requisites playbook.
<pre><code>
        ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/prerequisites.yml
</code></pre>
    3. On every node,
<pre><code>
        docker load -i /root/nsx-container-2.3.2.11695762/Kubernetes/nsx-ncp-rhel-2.3.2.11695762.tar
        docker image tag registry.local/2.3.2.11695762/nsx-ncp-rhel nsx-ncp
</code></pre>
    ![](2019-10-28-19-37-36.png)
    4. On the master node or jumphost, run the deploy-cluster playbook.
<pre><code>
        ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/deploy_cluster.yml
</code></pre>
    ![](2019-10-28-19-36-47.png)

    Success!!!
    ![](2019-10-28-19-38-15.png)