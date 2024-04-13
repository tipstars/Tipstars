# Tipstars

## **Introduction**

### Overview

Tipstars is a React Native social media application designed for my friends and me to share, critique, and track sports picks from FanDuel and other sportsbetting platforms.

### Motivation

The motivation behind creating this app stems from the growing trend of seeking sports betting advice from friends who perceive themselves as subject matter experts in their respective sports. However, as this practice has become more widespread, managing multiple group chats, side conversations, and recommendations from various sources has become overwhelming. This application aims to streamline and simplify the process by providing a centralized platform that eliminates the need to juggle between different communication channels, screenshots, and scattered information on FanDuel. By leveraging this app, users can make informed and validated sports predictions without the hassle and time-consuming effort associated with traditional methods.

## Technology Used

### Infastructure

- Terraform
- AWS
  - Elastic Load Balancers (ELBS)
  - VPC
  - EC2
  - RDS (PostgreSQL 16.1)
  - S3
  - CloudFront (CDN)
  - Route53
- Vault by Hashicorp
- Jenkins
- HAProxy
- Lambda

### Backend

&lt;Holocron.Callout type="note"&gt;
Ruby on Rails, GoLang, HaProxy, Jenkins Pipeline Scripts
&lt;/Holocron.Callout&gt;

Tipstars was developed to test my infrastructure knowledge, backend capabilities, and scalability requirements. The backend of Tipstars operates within a Virtual Private Cloud (VPC) to enhance security by isolating servers from external interference. Internal servers communicate through custom private Route53 records within the VPC.

Tipstars follows the Unix philosophy and microservices principle, boasting a scalable microservice architecture. For external access, traffic is directed through an Amazon Elastic Load Balancer (ELB) available at https://api.staging.tipstarsbetting.com. This ELB consists of three servers running HAProxy, which distributes traffic to the user and post microservice clusters. Each microservice cluster comprises three servers and is deployed across two private subnets to mitigate the impact of a subnet failure.

Deployment of each microservice follows Continuous Deployment/Continuous Integration (CI/CD) best practices with the open-source framework Jenkins. Jenkins is hosted on a server accessible only via a VPN, using OPENSense's OpenVPN AMI. A deployment job for each microservice is triggered by GitHub hooks when a merge successfully passes tests on the master branch of the respective repository.

These microservices communicate with a PostgreSQL database cluster that can scale on command to ensure high availability and throughput for data input and output.

Both microservices leverage Ruby on Rail's user-friendly features and excellent Object-Relational Mapping (ORM), making them popular among startups. They implement JWT cookie-based authentication to track users across each microservice while maintaining isolation to perform their specific functions. Communication between the microservices only occurs when necessary, following the microservices principle.

![](https://holocron.so/uploads/128b6c33-image.png)

