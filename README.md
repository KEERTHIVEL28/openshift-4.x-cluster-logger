OpenShift Container Platform - Cluster Logging
=========

This role deploys Cluster Logging on OpenShift 4.5.


Requirements
------------
* Ansible
* OpenShift 4.5 Deployed
* OpenShift cli

Role Variables
--------------

Type  | Description  | Default Value
--|---|--
use_ansible_k8s | use ansible k8s module | true
openshift_url  | OpenShift Login URL |  https://master.example.com:6443
openshift_token  | OpenShift Token used to login to OpenShift | 1234567890
insecure_skip_tls_verify  | Skip insecure tls verify | true
template_owner  |  owner of OpenShift tempalte  |  root
provision_elasticsearch_operator  | Deploy ElasticSearch Operator  |  yes
eo_channel  | Update Channel for the  Elasticsearch Operator Subscription | "4.5"
eo_installPlanApproval  | Install Plan for Elasticsearch Operator | "Automatic"
eo_source  | Needed if  installed on a restricted network, also known as a disconnected cluster | "redhat-operators"
eo_sourceNamespace  | Source Namespace of the Operator Installation | "openshift-marketplace"
eo_name  | Name of the Operator | "elasticsearch-operator"
provision_cluster_logging_operator  | Deploy Cluster Logging Operator  |  true
clo_channel  |  Update Channel for the  Cluster Logging  Operator Subscription |  "4.5"
clo_source  |  Needed if  installed on a restricted network, also known as a disconnected cluster  |  redhat-operators
clo_name  |  Name of the Operator |  cluster-logging
clo_sourceNamespace  | Source Namespace of the Operator Installation |  openshift-marketplace
provision_cluster_logging_instance  | Deploy Cluster Logging Instance  |  true
enable_es_storage  |  Set to true to define Elasticsearch Storage Endpoints |  false
replication_policy  |  Elasticsearch replication policy |  SingleRedundancy
management_state  |  [Configure Managed or Unmanaged State](https://docs.openshift.com/container-platform/4.5/logging/config/cluster-logging-management.html)  |  Managed
es_node_count  | Configure ElasticSearch node count  |  "2"
es_memory_limit  | Configure ElasticSearch node memory limit  |  2Gi
es_cpu_requests  | Configure ElasticSearch node cpu requests  |  200m
es_memory_requests  | Configure ElasticSearch node memory requests  |  2Gi
es_storageclassname  |  ElasticSearch storageclass to be used  |  "gp2"
es_storage_size  | Configure ElasticSearch node storage size  |  "200G"
kibana_replicas  | Configure Kibana replica count  |  "1"
kibana_memory_limit  | Configure Kibana  memory limit  |  1Gi
kibana_cpu_requests  |  Configure Kibana  cpu requests |  500m
kibana_memory_requests  | Configure Kibana  memory requests  |  1Gi
curation_memory_limit  | Curation memory limit  |  200Mi
curation_cpu_requests  | Curation cpu requests  |  200m
curation_memory_requests  |  Curation memory requests |  200Mi
curation_schedule  |  Curation Schedule  |  `"*/5 * * * *"`
collection_memory_limit  |  Collection memory limit |  1Gi
collection_cpu_requests  | Collection CPU requests  |  1G
delete_deployment  | delete the deployment and project cluster operator | false

Dependencies
------------
Requirements for Ansible k8
```
pip3 install kubernetes
pip3 install openshift
```
Requirements for openshift cli 
```
install oc command under /usr/bin
```

Installation
------------
```
$ ansible-galaxy install tosin2013.openshift_4_x_cluster_logger
```

Standard Playbook using Ansible k8
----------------
```YAML
- hosts: localhost
  become: yes
  vars:
    use_ansible_k8s: true
    clo_channel: "4.5"
    eo_channel: "4.5"
    delete_deployment: false
    insecure_skip_tls_verify: true
    provision_elasticsearch_operator: true
    provision_cluster_logging_operator: true
    provision_cluster_logging_instance: true
    enable_es_storage: false
  roles:
  - tosin2013.openshift_4_x_cluster_logger
```

Minimal Playbook  for Testing  using Ansible k8
----------------
```YAML
- hosts: localhost
  become: yes
  vars:
    use_ansible_k8s: true
    clo_channel: "4.5"
    eo_channel: "4.5"
    delete_deployment: false
    insecure_skip_tls_verify: true
    provision_elasticsearch_operator: true
    provision_cluster_logging_operator: true
    provision_cluster_logging_instance: true
    enable_es_storage: false
    es_node_count: 2
    es_memory_limit: 2Gi
    es_cpu_requests: 200m
    es_memory_requests: 2Gi
  roles:
    - tosin2013.openshift_4_x_cluster_logger
```

Standard Playbook using OpenShift CLI
----------------

```YAML
- hosts: localhost
  become: yes
  vars:
    openshift_token: 1234567890
    openshift_url: https://master.example.com:6443
    use_ansible_k8s: false
    clo_channel: "4.5"
    eo_channel: "4.5"
    delete_deployment: false
    insecure_skip_tls_verify: true
    provision_elasticsearch_operator: true
    provision_cluster_logging_operator: true
    provision_cluster_logging_instance: true
    enable_es_storage: false
  roles:
  - tosin2013.openshift_4_x_cluster_logger
```

Minimal Playbook  for Testing  using OpenShift CLI
----------------
```YAML
- hosts: localhost
  become: yes
  vars:
    openshift_token: 1234567890
    openshift_url: https://master.example.com:6443
    use_ansible_k8s: false
    clo_channel: "4.5"
    eo_channel: "4.5"
    delete_deployment: false
    insecure_skip_tls_verify: true
    provision_elasticsearch_operator: true
    provision_cluster_logging_operator: true
    provision_cluster_logging_instance: true
    enable_es_storage: false
    es_node_count: 2
    es_memory_limit: 2Gi
    es_cpu_requests: 200m
    es_memory_requests: 2Gi
  roles:
    - tosin2013.openshift_4_x_cluster_logger
```

Documentation
-------
*Links:*
* [Understanding cluster logging and OpenShift Container Platform](https://docs.openshift.com/container-platform/4.5/logging/cluster-logging.html)
* [About deploying cluster logging](https://docs.openshift.com/container-platform/4.5/logging/cluster-logging-deploying-about.html)
* [Deploying cluster logging](https://docs.openshift.com/container-platform/4.5/logging/cluster-logging-deploying.html)
* [Changing cluster logging management state](https://docs.openshift.com/container-platform/4.5/logging/config/cluster-logging-management.html)
* [Configuring cluster logging](https://docs.openshift.com/container-platform/4.5/logging/config/cluster-logging-configuring.html)
* [Configuring Elasticsearch to store and organize log data](https://docs.openshift.com/container-platform/4.5/logging/config/cluster-logging-elasticsearch.html)
* [Configuring Kibana](https://docs.openshift.com/container-platform/4.5/logging/config/cluster-logging-kibana.html)
* [Curation of Elasticsearch Data](https://docs.openshift.com/container-platform/4.5/logging/config/cluster-logging-curator.html)

*Elasticsearch replication policy*  
You can set the policy that defines how Elasticsearch shards are replicated across data nodes in the cluster:  

* FullRedundancy. The shards for each index are fully replicated to every data node.
* MultipleRedundancy. The shards for each index are spread over half of the data nodes.
* SingleRedundancy. A single copy of each shard. Logs are always available and recoverable as long as at least two data nodes exist.
* ZeroRedundancy. No copies of any shards. Logs may be unavailable (or lost) in the event a node is down or fails.

License
-------

GPL-3.0

Author Information
------------------

This role was created in 2020 by Tosin Akinosho
