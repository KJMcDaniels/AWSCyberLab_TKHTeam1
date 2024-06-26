# Multi-Subnet Cybersecurity Training Lab Setup on AWS
> Authors: Emmanuel Apiteu, Zedd Chisholm (Team Lead), Abraham DeLaCruz, KJ McDaniels, Opeyemi Olaleye, Alawi Raashed, Yonisbel Soto (Project Manager)

## Table of Contents

1. [Introduction](#introduction)
2. [Initial Setup and Configuration](#initial-setup-and-configuration)
   - [AWS Account Preparation](#aws-account-preparation)
   - [Environment Setup](#environment-setup)
3. [VPC and Networking Configuration](#vpc-and-networking-configuration)
   - [VPC Creation](#vpc-creation)
   - [Subnet Creation](#subnet-creation)
   - [Internet Gateway and Route Tables](#internet-gateway-and-route-tables)
4. [Security Configurations](#security-configurations)
   - [Security Groups and ACLs](#security-groups-and-acls)
   - [IAM Policies for Resource Access](#iam-policies-for-resource-access)
5. [Instance Management](#instance-management)
   - [EC2 Instance Launch](#ec2-instance-launch)
   - [Instance Networking and Security](#instance-networking-and-security)
6. [AWS Security Services Integration](#aws-security-services-integration)
   - [Enable Amazon GuardDuty](#enable-amazon-guardduty)
7. [Testing and Validation](#testing-and-validation)
8. [Troubleshooting and Optimization](#troubleshooting-and-optimization)
9. [Documentation and Collaboration](#documentation-and-collaboration)
10. [Conclusion and Next Steps](#conclusion-and-next-steps)
11. [Appendices](#appendices)
    - [A: Glossary of Terms](#a-glossary-of-terms)
12. [References](#references)

## Introduction

This document serves as a guide for setting up a multi-subnet cybersecurity training lab within AWS. The lab is designed to provide hands-on experience with securing cloud infrastructure using AWS's native security services. This README outlines the steps to configure a VPC with multiple subnets, security groups, and AWS security services for a comprehensive cybersecurity training environment.

## Initial Setup and Configuration

### AWS Account Preparation

- **Account Creation:** Ensure that you have an AWS account created and you're able to access the AWS Management Console.
    - You can find detailed instructions to do so in the [AWS Account Creation Guide](/assets/docs/AWS-Account-Creation.md)
- **IAM Roles and Policies:** Set up IAM roles with appropriate policies to provide your team with necessary access levels.

### Environment Setup

- Get familiar with the AWS Management Console.
- Check and adjust service limits in your region to suit the lab requirements.

## VPC and Networking Configuration

### VPC Creation

#### 1. Design the CIDR blocks for your VPC and subnets

- **Determine CIDR for VPC**: Choose a CIDR block for the VPC. A common practice is to use a `/16` CIDR block (e.g., `10.0.0.0/16`) to provide ample IP addresses for lab scenarios.
- **Plan Subnets**: Divide the VPC CIDR block into smaller CIDR blocks for each subnet. Planning might include a public subnet for internet-facing resources and private subnets for backend systems. For example:
  - Public Subnet: `10.0.1.0/24`
  - Training Tools Subnet: `10.0.2.0/24`
  - Active Directory Subnet (Optional): `10.0.3.0/24`
  - Docker Containers Subnet (Optional): `10.0.4.0/24`

#### 2. Navigate to the VPC dashboard and create a new VPC

- **Access the VPC Dashboard**: Log into the AWS Management Console, navigate to the "Services" menu, and select "VPC" under the "Networking & Content Delivery" category.
- **Launch VPC Wizard**: Click on "Launch VPC Wizard" to start the VPC creation process.
- **VPC Configuration**:
  - Choose "VPC with a Single Public Subnet" for simplicity or "VPC with Public and Private Subnets" for more complex setups.
  - Enter the VPC CIDR block and subnet CIDR as designed in the previous step.
  - Name your VPC and subnets for easy identification (e.g., "CyberSecLab-VPC" and "CyberSecLab-PublicSubnet").
  - Configure other options as necessary, such as enabling DNS hostname and DNS resolution.
- **Create VPC**: Review your configurations and click "Create VPC" to establish your new VPC.

### Subnet Creation

1. Plan and create four subnets: Public, Training Tools, Active Directory (optional), and Docker Containers (optional).
2. Assign appropriate CIDR blocks to each subnet.

### Internet Gateway and Route Tables

1. Create and attach an Internet Gateway to your VPC.
2. Set up route tables for your subnets, ensuring proper routing for internet access and inter-subnet communication.

## Security Configurations

### Security Groups and ACLs

- Create and configure security groups for your EC2 instances.
- Set up Network Access Control Lists (ACLs) for additional security at the subnet level.

### IAM Policies for Resource Access

- Define IAM policies that grant or restrict access to AWS resources, ensuring least privilege.

## Instance Management

### EC2 Instance Launch

- Select appropriate AMIs and instance types for your lab scenarios.
- Launch EC2 instances, configuring them with key pairs for SSH access.

### Instance Networking and Security

- Associate each instance with its respective subnet and security group.

## AWS Security Services Integration

### Enable Amazon GuardDuty

![AWS GuardDuty](assets/imgs/Guard-Duty-Finds-Portscan-01.jpg "Our AWS GuardDuty Dashboard")

Amazon GuardDuty is a threat detection service that offers continuous monitoring and protection for your AWS accounts and workloads. To enable GuardDuty, follow the steps below:

1. **Access GuardDuty Console**:
   - Navigate to the AWS Management Console and open the [GuardDuty console](https://console.aws.amazon.com/guardduty/).
   - Click on 'Get Started'.

2. **Enable GuardDuty Service**:
   - Review the introduction information provided by GuardDuty.
   - Click on 'Enable GuardDuty' to activate the service.

3. **Automatic Monitoring**:
   - GuardDuty starts analyzing and monitoring your AWS environment as soon as it is enabled, using data sources like AWS CloudTrail, VPC Flow Logs, and DNS logs to detect suspicious activities.

4. **Set Up Permissions**:
   - The service will create a service-linked role named `AWSServiceRoleForAmazonGuardDuty` that provides it with the necessary permissions to access your logs and metrics.

5. **Configure Additional Settings (Optional)**:
   - You may choose to customize GuardDuty settings by adding trusted IP lists or threat lists.
   - Configure GuardDuty to publish findings to Amazon CloudWatch Events for triggering responses or notifications.

6. **Validate Configuration**:
   - Generate and review sample findings in the GuardDuty console under 'Settings' by clicking 'Generate sample findings' to ensure proper setup.

7. **Monitoring and Maintenance**:
   - Regularly review findings and alerts from GuardDuty.
   - Implement additional alerts or notifications as required for comprehensive security monitoring.

Regular updates and reviews of GuardDuty configurations are recommended to keep up with new threats and changing AWS environments.

### Generate Sample Findings

1. **Generate Findings**: Within the GuardDuty console, navigate to 'Settings', under 'Sample findings', select 'Generate sample findings'.
2. **Review Findings**: Check the 'Findings' section for samples marked with `[SAMPLE]`.

![Sample Findings w/ GuardDuty](assets/imgs/Guard-Duty-Generate-Sample-Findings-Findings.jpg "Sample Findings w/ GuardDuty")

### Export Findings to Amazon S3 Bucket (Optional)

1. **Create S3 Bucket**: Set up S3 export settings from GuardDuty settings for long-term findings storage.
2. **Create KMS Key**: Establish a new KMS key for encryption via [AWS KMS console](https://console.aws.amazon.com/kms).

### Set Up Alerts with Amazon SNS (Optional)

1. **Create SNS Topic**: Create a new topic named 'GuardDuty' in the [Amazon SNS console](https://console.aws.amazon.com/sns/v3/home).
2. **Create EventBridge Rule**: Use [EventBridge console](https://console.aws.amazon.com/events/) to make a rule that sends GuardDuty findings to the SNS topic.

### Amazon Inspector

Amazon Inspector is an automated security assessment service that helps to identify potential security issues within your AWS resources. It can automatically assess applications for vulnerabilities or deviations from best practices, and it produces detailed security findings.

#### Configuring Amazon Inspector

1. **Enable Amazon Inspector**:
   - Navigate to the Amazon Inspector console within the AWS Management Console.
   - Select 'First-time setup' and follow the prompts to define the assessment target.

2. **Define Assessment Target**:
   - Set up an assessment target by selecting the AWS resources you want to assess, typically based on tags assigned to your EC2 instances or the entire VPC.

3. **Create an Assessment Template**:
   - Create an assessment template, which includes the target, the rules packages to evaluate, and the duration for the assessment run.

4. **Run the Assessment**:
   - Start the assessment run. Amazon Inspector will analyze the behavior of the instances and the network configuration.

5. **Review Findings**:
   - Once the assessment is complete, review the findings under the 'Findings' section of the Amazon Inspector console.
   - Findings include a description of the issue, its severity, and recommendations for mitigation.

6. **Integrate with AWS Services** (Optional):
   - Utilize other AWS services such as AWS Lambda, SNS, or CloudWatch Events to automate responses to specific findings.
  
7. **On-Demand Scan** (Optional):
   - Navigate to Inspector CIS Scans and select "Create New Scan" in order to create an on-Bemand scan. For Example:
     ![Create New Scan](assets/imgs/Inscpector-CIS-Create-Scan-02.jpg "Create a New Scan")
  
   ![AWS Inspector On-Demand Scan Results](assets/imgs/Inspector-Scan-Results-01.jpg "Inspector On-Demand Scan Results")

### AWS WAF

AWS WAF is a web application firewall service that helps to protect your web applications from common web exploits that could affect application availability, compromise security, or consume excessive resources.

#### Setting up AWS WAF

1. **Go to AWS WAF & Shield**:
   - Access the AWS WAF & Shield console through the AWS Management Console.

2. **Create a Web ACL**:
   - Click 'Create web ACL' and define a name and a resource (such as an Application Load Balancer, API Gateway, or CloudFront distribution) to associate with the web ACL.

3. **Configure Rules**:
   - Configure rules to block or allow requests based on various criteria, including IP addresses, HTTP headers, HTTP body, or custom URIs.

4. **Set up Rule Actions**:
   - Define actions for each rule - either block, allow, or count the requests that match the rule's criteria.

5. **Deploy the Web ACL**:
   - Review the configurations and deploy the web ACL to the selected resource.

6. **Monitor Requests**:
   - Monitor the requests that are allowed or blocked by AWS WAF by using Amazon CloudWatch metrics.

7. **Fine-tuning** (Optional):
   - As you monitor, fine-tune your rules to minimize false positives and ensure legitimate traffic is not inadvertently blocked.

For detailed instructions and best practices on setting up and managing AWS Inspector and AWS WAF, consult the official AWS documentation.


### Amazon CloudWatch

Amazon CloudWatch is an essential tool for logging and monitoring your EC2 instances. By configuring CloudWatch, you can collect logs from your EC2 instances and gain insights into their performance and behavior. 

![AWS CloudWatch Ec2 Metrics](assets/imgs/CloudWatch-EC2-Metrics.jpg "AWS CloudWatch EC2 Metrics")

Follow these steps to set up logging for an EC2 instance with CloudWatch:

1. **Create the IAM Role Necessary to Run the CloudWatch Agent on EC2 Instances**: Create an IAM role with the necessary permissions to allow the EC2 instance to send logs to CloudWatch. The role should have the `CloudWatchAgentServerPolicy` managed policy attached.

2. **Download and Configure the CloudWatch Agent Using the Command Line**: Download the CloudWatch agent package for the EC2 instance. Find detailed instructions in the [CloudWatch Agent documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/download-cloudwatch-agent-commandline.html).

3. **Installing and Running the CloudWatch Agent on the EC2 Servers**: Attach the IAM role to the server that will run the agent. Install the agent package and start using the agent configuration create. Find detailed instructions in the [CloudWatch Agent documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/download-cloudwatch-agent-commandline.html).

4. **Verify Logs in CloudWatch Console**: Once the agent is installed and configured, verify that logs are being sent to CloudWatch by checking the CloudWatch Logs section in the AWS Management Console.

5. **Set Up Alarms**: Create CloudWatch alarms to monitor specific metrics and trigger notifications or automated actions based on predefined thresholds.

<!-- 6. **Create Dashboards**: Build custom CloudWatch dashboards to visualize key metrics and logs from your EC2 instances. -->

For more information on CloudWatch, refer to the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html).

## Testing and Validation

- Conduct network connectivity tests and validate the configuration of AWS security services.

### Port Scanning and Exploitation with Kali Linux
![nmap Port Scan w/ Kali Lunix](assets/imgs/Kali-atk-win-nmap-scan-01.jpg "Kali Lunix nmap scan of Victim Machine")

In the process of testing our lab's security, we utilized Kali Linux, a powerful tool for cybersecurity experts, to conduct a port scan against the victim machine. Here's a brief overview of the steps we took:

1. **Conducting Port Scan with Nmap**:
   - We ran an Nmap scan to discover open ports on the victim machine.
   - The command used was `nmap <victim-ip-address>`.
   - Initially, Nmap reported the host as down due to the lack of responses to ping probes.
   - We then executed `nmap -Pn <victim-ip-address>` to treat the host as up and bypass ping scanning, which revealed open ports.

2. **Identifying Vulnerable Services**:
   - The Nmap scan identified a service running on port 3389/tcp known for RDP (Remote Desktop Protocol), a potential point of entry.

3. **Exploitation Using Metasploit**:
   - With the open ports and services identified, we launched Metasploit, a widely used framework for exploit development and execution.
   - We targeted the services discovered during the Nmap scan to attempt to exploit known vulnerabilities.

This process highlighted the importance of regular vulnerability scanning within network environments and served as a practical exercise in identifying and mitigating potential security threats.


## Troubleshooting and Optimization

### Common Issues and Solutions

- **SSH Connectivity Issues**:
  - Confirmed that the Internet Gateway was correctly attached to the VPC.
  - Reviewed and updated route tables to ensure proper routing for SSH traffic.
  - Validated security group rules to allow SSH access (port 22) from the appropriate IP ranges.

- **Performance Optimization**:
  - Monitored resource usage to identify and alleviate bottlenecks.
  - Optimized instance sizes and types based on performance data.
  - Implemented scaling policies to handle variable load efficiently.

- **Security Best Practices**:
  - Continuously updated and refined Security Groups and Network ACLs to enforce the principle of least privilege.
  - Regularly audited our environment using AWS security tools to preemptively identify and mitigate potential security issues.

## Documentation and Collaboration

A rigorous approach to documentation and collaboration has been central to the lab's successful setup and operation:

### Documenting Every Step:

- Thoroughly documented the setup process, configurations, and any issues encountered along with their resolutions in a shared repository.
- Maintained clear and descriptive commit messages and documentation updates to provide context and rationale for changes.

### Collaboration Tools:

- Utilized Git for version control, allowing for coordinated and trackable team contributions.
- Employed project management and communication tools to facilitate clear and efficient task delegation and status tracking among team members.

## Conclusion and Next Steps

With the foundational setup of our AWS-based cybersecurity training lab complete, we are poised to enter the next phase. We plan to:

- Introduce more advanced threat scenarios and simulate responses.
- Continuously improve our security posture by integrating the latest AWS services and features.
- Conduct regular training sessions to keep the team adept at recognizing and responding to the latest cybersecurity threats.

### Future Improvement Ideas
- **Adding a Bastion Host:** Enhances network segmentation and provides a controlled environment for sensitive operations away from public access.
- **Integrating Advanced Security Features:** Utilize services like AWS Shield for DDoS protection and AWS Key Management Service for secure key storage.
- **Using additional Managed Tools:** Incorporate advanced AWS services such as AWS Systems Manager for streamlined management and automation.

## Appendices

### A: Glossary of Terms

| Term | Definition |
|------|------------|
| Amazon CloudWatch | A monitoring service for AWS cloud resources and the applications you run on AWS. |
| Amazon GuardDuty | A threat detection service that continuously monitors for malicious activity and unauthorized behavior. |
| Amazon Inspector | An automated security assessment service that helps improve the security and compliance of applications deployed on AWS. |
| Amazon VPC | Amazon Virtual Private Cloud lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network. |
| CIDR Block | Classless Inter-Domain Routing. An IP addressing scheme that improves the allocation of IP addresses. |
| EC2 | Amazon Elastic Compute Cloud. A web service that provides secure, resizable compute capacity in the cloud. |
| IAM | AWS Identity and Access Management. It helps you securely control access to AWS resources. |
| Internet Gateway | A VPC component that allows communication between instances in your VPC and the internet. |
| NACL | Network Access Control List. An optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets. |
| Port Scanning | Using a network tool like Nmap to scan a computer for open ports. |
| RDP | Remote Desktop Protocol. A proprietary protocol developed by Microsoft that provides a user with a graphical interface to connect to another computer over a network connection. |
| Route Table | A set of rules, called routes, that determine where network traffic from your subnet or gateway is directed. |
| Security Group | A virtual firewall for an instance to control inbound and outbound traffic. |
| SSH | Secure Shell. A cryptographic network protocol for operating network services securely over an unsecured network. |
| Subnet | A range of IP addresses in your VPC. |
| VPC | Virtual Private Cloud. An isolated section of the AWS cloud where you can launch resources in a virtual network that you define. |
| WAF | Web Application Firewall. Helps protect your web applications from common web exploits. |
| AWS CLI | AWS Command Line Interface. A unified tool to manage your AWS services. |
| SDK | Software Development Kit. A collection of software tools and libraries for creating applications for a specific device or operating system. |

## References

Please view our references on our [AWS Links](/assets/docs/res/AWS-Links.md) reference document. Here you can find additional link to useful AWS Documentation and Resources.