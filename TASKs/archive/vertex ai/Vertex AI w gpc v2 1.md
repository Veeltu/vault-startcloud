

## On-Prem to Vertex AI connection in GCP


##### PSC lets you create a private, secure connection between your on-premises network and Vertex AI. 


***


**How it Works (Simplified):**

1. **The Endpoint:** Creating a PSC endpoint within  Google Cloud Virtual Private Cloud (VPC) network. This endpoint acts as the entry point for your on-premises traffic.
2. **Private Connection:** Instead of routing your traffic over the public internet, it goes through a private connection (e.g., Cloud VPN or Cloud Interconnect) to Google Cloud VPC network.
3. **Reaching Vertex AI:** The PSC endpoint in VPC forwards requests to Vertex AI services, all without ever leaving the Google Cloud network.

**Benefits of Using PSC:**

- **Enhanced Security:** No public internet exposure minimizes the risk of data exfiltration and unauthorized access.
- **Private Communication:** Keeps your data within a private network.
- **Simplified Networking:** Streamlines network configuration compared to some other methods.

**Key Components:**

- **On-premises Network:** Your existing network infrastructure.
- **Cloud VPN/Interconnect:** A secure connection between your on-premises network and your Google Cloud VPC.
- **Google Cloud VPC:** Your private network within Google Cloud.
- **Private Service Connect Endpoint:** The connection point within your VPC for accessing Vertex AI.
- **Vertex AI:** Google Cloud's machine learning platform.



***


#### **PSC Options for Vertex AI**

**Core Idea:** PSC allows private and secure connections to managed services (like Google APIs or services in other VPCs) without exposing traffic to the public internet.

There are two main ways to use Private Service Connect with Vertex AI:

- **PSC for Google APIs:** This allows you to privately access Vertex AI APIs (e.g., prediction, training) from within your VPC network. Your on-premises network connects to your VPC via Cloud VPN or Interconnect, and then accesses Vertex AI through the PSC endpoint in your VPC3.
- **PSC for Vertex AI resources:** This enables private access to specific Vertex AI resources like `CustomJob`.

Connecting from on-premises to Vertex AI, the **PSC for Google APIs** approach is the more relevant one. It's about privately reaching the Vertex AI _service_, rather than a specific resource within Vertex AI.


***


### **Configuration for On-Premises to Vertex AI via PSC & Shared VPC**



#### **Shared VPC Network**
A Shared VPC network that allows multiple projects to share a common VPC network. Includes:
- subnet for PSC
- subnet for Shared-VPC
- Network Attachment

***Network Attachment*** In that context, is a resource that allows a producer Virtual Private Cloud (VPC) network to initiate connections to a consumer VPC network through a Private Service Connect interface

##### Configuration:

1. **Creation of a Network Attachment in the Host VPC Project**: The subnet used for the Network Attachment must be created in the Host VPC project. The Network Attachment is created in the service project.

2. **Private Service Connect Configuration in the Service Project**: The Vertex AI resource is configured in the service project, pointing to the Network Attachment created in the Host VPC project.

3. **Permissions Settings**: Ensure that the AI Platform Service Agent in the service project has the Compute Network Admin role in the Host VPC project.

4. **Firewall Configuration**: Configure firewall rules in the consumer VPC project to allow communication with the Network Attachment.


**Subnet Range:** Minimal range for Vertex AI requires at least a `/28` subnetwork.

Vertex AI can only reach the RFC 1918 ranges specified in the required PRIMARY_RANGE. Vertex AI can't reach the following non-RFC 1918 ranges:
	- `100.64.0.0/10`
	- `192.0.0.0/24`
	- `192.0.2.0/24`
	- `198.18.0.0/15`
	- `198.51.100.0/24`
	- `203.0.113.0/24`
	- `240.0.0.0/4`



***


#### **Private Service Connect (PSC) Endpoint Configuration:**

Private Service Connect endpoints are internal IP addresses in a consumer VPC network that can be directly accessed by clients in that network. Endpoints are created by deploying a **forwarding rule** that references a **service attachment** a bundle of Google APIs.


- **Project:** This is configured in the _host project_ of your Shared VPC.
- **Creating the Endpoint:** Create a global address with the purpose `PRIVATE_SERVICE_CONNECT`. Use the `gcloud compute addresses create` command.
- **Forwarding Rule:** Create a forwarding rule that directs traffic to the Google APIs bundle.

examples:
```
gcloud compute addresses create psc-ip \
--global \
--purpose=PRIVATE_SERVICE_CONNECT \
--addresses=100.100.10.10 \
--network=aiml-vpc # Replace with your Shared VPC name
```
	
```
gcloud compute forwarding-rules create pscvertex \
--global \
--network=aiml-vpc # Replace with your Shared VPC name\
--address=psc-ip \
--target-google-apis-bundle=all-apis
```


***


#### **Load Balancer (Not Directly Involved):**
    
The `target-google-apis-bundle` in the **forwarding rule** handles the "load balancing" to the Google APIs. You don't provision a separate load balancer instance. The **PSC endpoint** is the entry point.


***


#### **On-Premises Configuration:**

**Configuration of Connection to On-Prem**For connecting to the on-premises network, consider the following:

- **Establishing a Hybrid Connection**: Use Cloud VPN or Cloud Interconnect to connect the on-premises network to the VPC in Google Cloud.
- **Configuring Cloud Router**: Set up a Cloud Router to exchange routes between the on-premises network and the VPC.
- **Creating a PSC Endpoint**: Create a PSC endpoint in the VPC network that will be used to access the Vertex AI API from the on-premises network. The PSC IP address must be advertised as a custom route advertisement from the Cloud Router to the on-premises network.
- **Updating DNS and Firewalls**: Update the DNS in the on-premises network to resolve the Vertex AI API to the PSC IP address. Update firewalls to allow access to the PSC IP address.







***
***
ref.

Vertex AI networking overview
https://cloud.google.com/vertex-ai/docs/general/netsec-overview

*Use Private Service Connect to access Vertex AI online predictions from on-premises
https://cloud.google.com/vertex-ai/docs/general/vertex-psc-googleapis

Set up a Private Service Connect interface for Vertex AI resources
https://cloud.google.com/vertex-ai/docs/general/vpc-psc-i-setup

About accessing Vertex AI services through Private Service Connect endpoints
https://cloud.google.com/vertex-ai/docs/general/psc-endpoints

Private Service Connect
https://cloud.google.com/vertex-ai/docs/general/psc-endpoints


Prywatny dostęp do punktów końcowych prognozowania online Vertex AI przy użyciu PSC
https://codelabs.developers.google.com/codelabs/vertex-psc-googleapis?hl=pl#2

