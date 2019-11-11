# Akraino

- [Akraino Release 1](https://www.lfedge.org/projects/akraino/release-1/)
- [Akraino Demo Edge Lightweight IoT (ELIOT) - IoT Gateway](https://www.lfedge.org/projects/akraino/release-1/edge-lightweight-iot-eliot-iot-gateway/)
  - [Eliot Source Code](https://gerrit.akraino.org/r/gitweb?p=eliot.git;a=shortlog)
    - k8s master, kubelet, kubernetes-cni, kubeadm, docker, Eliot Manager, Edge Node, calico, calico rbac
    -  kubeedge

## Blueprint

- Akraino uses the “blueprint” concept to address specific Edge use cases to support an end-to-end solution.
- A blueprint is a declarative configuration of the entire stack-- i.e., edge platform that can support edge workloads and edge APIs.
- To address specific use cases, a blueprint architecture is developed by thecommunity and a declarative configuration is used to define all the components used within that architecture such as hardware, software, tools to manage the entire stack, and method of deployment (Blueprints aremaintained using full CI/CD integration and testing by the community for ready download and install).


## Demo

> [Akraino StarlingX](https://wiki.akraino.org/download/attachments/6128319/Akraino-StarlingX-EdgeX.mp4?version=1&modificationDate=1553808128000&api=v2)

Deliverables

- Alarm and status aggregation at central site.
- Automated software patch installation across all subclouds.
- Automated synchronization of flavors and images fron central cloud to subclouds.
- Deploying and running EdgeX in a subcloud.

## StarlingX

### Deliverables

- StarlingX Far Edge Distributed Cloud API Documentation
- StarlingX Far Edge Distributed Cloud Architecture Documentation
- StarlingX Far Edge Distributed Cloud Installation Guide
  - [Here](https://wiki.akraino.org/display/AK/StarlingX+Far+Edge+Distributed+Cloud+Installation+Guide)
  - Added Gerrit review link.
- StarlingX Far Edge Distributed Cloud Release Notes (this document)
- StarlingX Far Edge Distributed Cloud Test Documentation

### Resources

- https://wiki.akraino.org/display/AK/Jenkins+Guide
- https://jenkins.akraino.org/sandbox
