# Tipstars

## **Introduction**

### Overview

Tipstars is a React Native social media application designed for my friends and I to share, critique, and track sports picks from FanDuel and other sportsbetting platforms.

You can download the application using this testflight link here: <https://testflight.apple.com/join/iIbERDfn>

### Motivation

⁤My friends are always seeking sports advice from others who are considered "subject matter experts" in their respective sport. ⁤⁤The problem is managing multiple group chats, side conversations, and recommendations from various sources has become overwhelming. ⁤⁤Tipstars aims to simplify the process by creating a centralized platform that eliminates the need to juggle between different communication channels and screenshots. ⁤⁤By leveraging this app, friends can make informed sports predictions without the time-consuming effort associated with traditional methods. ⁤

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
  - SNS
- Vault by Hashicorp
- Jenkins
- HAProxy
- Lambda
- Next.js

### Backend

```markdown
Ruby on Rails, GoLang, HaProxy, Jenkins Pipeline Scripts
```

Tipstars was developed to test my infrastructure knowledge, backend capabilities, and scalability requirements. The backend of Tipstars operates within a Virtual Private Cloud (VPC) to enhance security by isolating servers from external interference. Internal servers communicate through custom private Route53 records within the VPC.

Tipstars follows the Unix philosophy and microservices principle, boasting a scalable microservice architecture. For external access, traffic is directed through an Amazon Elastic Load Balancer (ELB) available at https://api.staging.tipstarsbetting.com. The ELB consists of 3 servers hosting HAProxy instances, which distribute traffic to the various microservice clusters. Each microservice cluster comprises 3 servers and is deployed across two private subnets to mitigate the impact of a subnet failure.

Deployment of each microservice follows Continuous Deployment/Continuous Integration (CI/CD) best practices with the open-source framework Jenkins. Jenkins is hosted on a server accessible only via a VPN, using OPENSense's OpenVPN AMI. A deployment job for each microservice is triggered by GitHub hooks when a merge successfully passes tests on the master branch of the respective repository.

These microservices communicate with a PostgreSQL database cluster that can scale on command to ensure high availability and throughput for data input and output.

Both microservices leverage Ruby on Rail's user-friendly features and excellent Object-Relational Mapping (ORM), making them popular among startups. They implement JWT cookie-based authentication to track users across each microservice while maintaining isolation to perform their specific functions. Communication between the microservices only occurs when necessary, following the microservices principle.

There are GoLang Lambda jobs that run every 5 minutes to update game data if there is no user traffic refreshing it. These tasks ensure that sports information, particularly for games that haven't been settled yet, is updated periodically. Platforms like FanDuel and DraftKings expire their game data 12 hours after an event finishes. The event data can be updated either through user traffic or by running these GoLang jobs.

GoLang was chosen over Node.js for these Lambda functions as a new challenge, reduce fees with AWS, and benefit from performance gains and faster cold start times.

![](https://holocron.so/uploads/128b6c33-image.png)

### Frontend

```markup
React Native, Objective-C, Figma, TypeScript
```

To enhance my efficiency on this project, I opted in favor of using React Native, a framework I have used extensively. Prior to coding the project, I designed the user interface in Figma, followed by developing a custom component library to emphasize reusability within the React ecosystem.

From creating a custom styles library relatively similar to TailwindCSS  for this React Native project with appearance responsiveness, to developing custom React Native bridged packages for seamless integration with third-party applications like FanDuel and DraftKings, this application showcases my expertise in app development and React fundamentals.

Feel free to demo the application or watch the following videos to see Tipstars in action!

#### Virtualization and feed

This application's feed utilizies ideologies commonly found in other infinite scroll lists

- **Stale While Revalidate (SWR):** SWR was initially introduced in 2010, but it gained popularity among React applications after Vercel developed its own react hook package. Tipstars utilizes its custom natively bridged SWR hook to ensure app readiness upon startup or unsuspense. Unlike the SWR hook, the custom native module aims to use the device's native storages instead of storing cache in the JavaScript layer.
- **Virtualization:** Virtualization becomes vital as an application's content grows. Tipstars focuses on maximizing the FPS of both the JavaScript layer and native layer simultaneously. Even with continuously looped animations, there is no drop in FPS between the JavaScript and native layers. This is achieved through a deep understanding of component memorization, recyclability, and re-render management, making the process seamless.
- **Content Delivery Service (CDN):** CDNs play an important role in virtualization, surprisingly enough. Ensuring images and other attachments are readily available and cached allows the JavaScript and native layer threads to stream more efficiently when loading components.

_/This project was designed, planned, developed, maintained for all by Mohamed Adam._