# Validation Runbook  

This runbook outlines how to deploy the NIDS lab, generate traffic, verify ingestion, and tune rules.  

## Deploy Lab  
1. Run `terraform apply` in the `terraform/` directory to build the infrastructure.  
2. Execute the Ansible playbooks to install and configure Suricata, Filebeat and the Elastic stack on the respective hosts.  

## Generate Traffic  
- Use an external machine to send a mix of benign (e.g. ICMP pings, HTTP requests) and malicious traffic (such as patterns that match Suricata rules) through the sensor instance.  
- Ensure that Suricata writes events to `/var/log/suricata/eve.json` on the sensor host.  

## Verify Data Ingestion  
- Access Kibana (port 5601 on the Elastic host) and open the **Discover** page.  
- Confirm that a `filebeat-*` index exists and that Suricata events are present.  
- Check important fields like `event.category`, `src_ip`, `dest_ip`, `suricata.alert.signature`, and `suricata.alert.action` to make sure they are populated.  

## Dashboard Validation  
- Import the provided dashboards from `elastic/` if they are not already loaded.  
- Examine the **Alert vs Drop** visualization to verify that alerts and dropped packets are classified correctly.  
- Review the **Top Attacker IPs** and **Rule Hit Ranking** visualizations to see if the generated traffic appears as expected.  
- Create additional visualizations if needed to highlight suspicious hosts or frequent signatures.  

## Rule Tuning and Testing  
- Modify or add Suricata rules (e.g. a rule that triggers on a specific HTTP string) in the `suricata/` directory.  
- Reload Suricata on the sensor host to apply the new rules.  
- Generate traffic that matches the new rule and confirm that it appears in Kibana dashboards.  
- Adjust rule thresholds or signatures to reduce false positives and negatives.  

## Cleanup  
- When testing is complete, run `terraform destroy` to tear down the lab and free resources. 
