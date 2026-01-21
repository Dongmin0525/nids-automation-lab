# Lab Setup Guide  
This guide walks you through provisioning a small cloud environment and deploying a Suricata‑based Network Intrusion Detection System integrated with the Elastic Stack.  

## Infrastructure Provisioning (Terraform)  
- Define a VPC or equivalent network with a Suricata “sensor” instance and an Elastic Stack instance.    
- Configure security groups/firewall rules so the sensor can forward logs to the Elastic host and permit SSH access from your IP.    
- Run `terraform init` to prepare the working directory and `terraform apply` to create the infrastructure.    
- Use variables to customize provider credentials, region and instance sizes.  

## Configuration Management (Ansible)  
- The `ansible/` directory contains playbooks.    
- On the sensor host:    
  - Install Suricata and enable the `eve.json` output in `suricata.yaml` so alerts are logged in JSON.    
  - Install Filebeat and enable the Suricata module to read `eve.json` and ship events to Elasticsearch.    
- On the Elastic host:    
  - Install Elasticsearch and Kibana with security enabled.    
  - Create an index and service account for Filebeat to write data.    
  - Expose Kibana on port 5601.    
- Use group variables or inventory to define host roles, then run `ansible-playbook install_siem.yml` and `ansible-playbook install_sensor.yml`.  

## Suricata Configuration  
- Place a tuned `suricata.yaml` in `suricata/` enabling `eve-log` with JSON output and including fields such as `src_ip`, `dest_ip`, `protocol`, and `suricata.alert.*` attributes.    
- Update the rule set (for example using the ET Open rules) so Suricata will detect common attacks.    
- Reload Suricata after changing the configuration or rules.  

## Elastic Dashboards  
- Import Kibana dashboards from the `elastic/` folder via Stack Management → Saved Objects → Import.    
- Dashboards should include visualizations such as:    
  - Alert vs. Drop counts.    
  - Top attacker source IPs.    
  - Rule hit rankings.    
- Ensure that Filebeat is creating a `filebeat-*` index pattern and that fields like `event.category`, `suricata.alert.signature`, and `suricata.alert.action` appear in Discover.  

## Test Traffic  
- After deployment, generate benign and malicious traffic through the sensor host (e.g. pings, HTTP requests, patterns matching Suricata rules).    
- Confirm that Suricata logs events to `eve.json` and that Filebeat forwards them to Elasticsearch.  

## Cleanup  
- When finished testing, run `terraform destroy` to tear down the lab environment and avoid ongoing costs. 
