# Akraino

- [Akraini Documentation](https://wiki.akraino.org/display/AK/Documentation)
- [Akraino Sub-Stream Committee](https://wiki.akraino.org/display/AK/Upstream+Sub-committee)
- [Akraino Release 1](https://www.lfedge.org/projects/akraino/release-1/)
- [Akraino Demo Edge Lightweight IoT (ELIOT) - IoT Gateway](https://www.lfedge.org/projects/akraino/release-1/edge-lightweight-iot-eliot-iot-gateway/)

## Blueprint

- Akraino uses the “blueprint” concept to address specific Edge use cases to support an end-to-end solution.
- A blueprint is a declarative configuration of the entire stack-- i.e., edge platform that can support edge workloads and edge APIs.
- To address specific use cases, a blueprint architecture is developed by thecommunity and a declarative configuration is used to define all the components used within that architecture such as hardware, software, tools to manage the entire stack, and method of deployment (Blueprints aremaintained using full CI/CD integration and testing by the community for ready download and install).


## Demos

> [Akraino StarlingX](https://wiki.akraino.org/download/attachments/6128319/Akraino-StarlingX-EdgeX.mp4?version=1&modificationDate=1553808128000&api=v2)

### Elio

> Edge Lightweight and IOT (IOT Gataway and SD-WAN)

Release 1

- [Elio Architecture Document](https://wiki.akraino.org/display/AK/ELIOT+Architecture+Document)
- __Important__ [ELIOT Release 1 Documentation](https://wiki.akraino.org/display/AK/ELIOT+Release+1+Documentation)

Release 2

- [ELIOT IoT Gateway Architecture](https://wiki.akraino.org/display/AK/ELIOT+Release+2+-+IoT+Gateway+Architecture+Document)
- [ELIOT SD-WAN/WAN Edge/ uCPE Architecture](https://wiki.akraino.org/pages/viewpage.action?pageId=20316769)

Source Code

- [Eliot Source Code](https://gerrit.akraino.org/r/gitweb?p=eliot.git;a=shortlog)
  - k8s master, kubelet, kubernetes-cni, kubeadm, docker, Eliot Manager, Edge Node, calico, calico rbac
  - kubeedge, nginx

Blueprints

- See Blueprint directory

### AI on the Edge

- [AI on the Edge](https://wiki.akraino.org/display/AK/Presentation+Videos)

## StarlingX

### Release 1

- [Blueprint Proposal](https://wiki.akraino.org/download/attachments/6128319/Akraino_Blueprint_Far_Edge_v3.pdf?version=2&modificationDate=1541791587000&api=v2)
- [Blueprint Specification](https://wiki.akraino.org/download/attachments/6128319/Far%20Edge%20Akraino%20Blueprint%20v5.pdf?version=1&modificationDate=1544201319000&api=v2)
- [1](https://wiki.akraino.org/display/AK/StarlingX+Far+Edge+Distributed+Cloud?preview=%2F6128319%2F11995237%2FAkraino+Rel+1+Self-Certification+Status+-+StarlingX+Far+Edge+Distributed+Cloud.pptx)

#### Deliverables

See all documentation [here](https://wiki.akraino.org/display/AK/StarlingX+Far+Edge+Distributed+Cloud+Documentation)

- StarlingX Far Edge Distributed Cloud API Documentation
- StarlingX Far Edge Distributed Cloud Architecture Documentation
- StarlingX Far Edge Distributed Cloud Installation Guide
  - [Here](https://wiki.akraino.org/display/AK/StarlingX+Far+Edge+Distributed+Cloud+Installation+Guide)
  - Added Gerrit review link.
- StarlingX Far Edge Distributed Cloud Release Notes (this document)
- [StarlingX Far Edge Distributed Cloud Test Documentation](https://wiki.akraino.org/display/AK/StarlingX+Far+Edge+Distributed+Cloud+Test+Documentation)
  - https://github.com/rohitsardesai83/edgex-on-kubernetes/
  - https://github.com/rohitsardesai83/edgex-on-kubernetes/blob/master/hack/edgex-up.sh
  
#### Demo Features Shown

- Alarm and status aggregation at central site.
- Automated software patch installation across all subclouds.
- Automated synchronization of flavors and images fron central cloud to subclouds.
- Deploying and running EdgeX in a subcloud.

#### Source Code

- Do we have a blueprint directory under source code?

#### CI/CD

- Where is CI/CD infrastucture coming from? [e.g. Eliot](https://gerrit.akraino.org/r/gitweb?p=eliot.git;a=commit;h=08061086d68d8c4f0a027c910c5e498b1016e12a)
- [Akraino Jenkins Master](https://jenkins.akraino.org/view/starlingx/job/starlingx-master-verify/)
- [Akriani Gerrit CI Management](https://gerrit.akraino.org/r/admin/repos/ci-management)

### Release 2


### Questions

- Far Edge Cloud
- Far Edge Family
- Far Edge Species
- From [Far Edge Blueprint Distributed](https://wiki.akraino.org/pages/viewpage.action?pageId=11995356&preview=%2F11995356%2F11997178%2FAkraino_Blueprint_Far_Edge_R2.pdf), under slide 4 "Distributed Far Edge Blueprint Akraino R1", "Little  dependency on Akraino CI/CD"?
- From [Far Edge Blueprint Distributed](https://wiki.akraino.org/pages/viewpage.action?pageId=11995356&preview=%2F11995356%2F11997178%2FAkraino_Blueprint_Far_Edge_R2.pdf), under slide 5 "Distributed Far Edge Akraino R2 Blueprint", "Better integration with Akraino CI/CD"?

### Resources

- https://wiki.akraino.org/display/AK/Jenkins+Guide
- https://jenkins.akraino.org/sandbox
