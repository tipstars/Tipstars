# Tipstars

## **Introduction**

### Overview

Tipstars is a React Native social media application that’s designed for my friends and I to share, critique, and track sports picks from  sports betting platforms like FanDuel.

You can download it with this testflight link: <https://testflight.apple.com/join/iIbERDfn>

### Motivation

⁤My friends are constantly on the lookout from sport subject matter experts. ⁤However, it can be overwhelming to maintain multiple group chats, side conversations, and recommendations from different sources simultaneously. ⁤⁤Tipstars aims to simplify this process by creating a centralized platform, eliminating the need for different communication channels and screenshots, saving time and headaches.

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

Tipstars was developed to test my skills on infrastructure knowledge, backend capabilities, and scalability requirements. The backend operates within a VPC, enhancing security by isolating servers from external interference. Internal servers communicate through custom private Route53 records, within the VPC.

Tipstars follows best Unix and microservice practices, boasting a scalable architecture. For external access, traffic is directed through an Amazon Elastic Load Balancer (ELB), available at https://api.staging.tipstarsbetting.com. The ELB consists of 3 servers hosting HAProxy instances, which distributes traffic to the various microservice clusters. Each microservice cluster comprises of 3 servers and is deployed across two private subnets, mitigating the impact of any failures.

Each microservice's deployment follows best CI/CD with Jenkins. Jenkins is hosted on a server accessible only via a VPN, using OPENSense's OpenVPN AMI. A deployment job for each microservice is triggered by GitHub hooks, when a PR successfully merges with the master, passing all tests.

These microservices communicate with a highly scalalbe PostgreSQL database cluster, ensuring high availability and throughput for data input/output.

Both microservices leverage Ruby on Rail's user-friendly features and excellent Object-Relational Mapping (ORM), a popular choice among startups. They implement JWT cookie-based authentication to track users across each microservice, while maintaining isolation to perform respective functions. Communication between the microservices only occurs on an ad hoc basis, following the microservices principle.

GoLang Lambda jobs run every 5 minutes to update game data, in case no user traffic refreshes it. This ensures sports information is updated periodically, particularly for games that aren't finished. Platforms like FanDuel and DraftKings expire their game data 12 hours following an event, which is updated through either user traffic or by running these GoLang jobs.

GoLang was chosen over Node.js for these Lambda functions as a new challenge, reduce fees with AWS, benefit from performance gains, and faster cold start times.

![](https://holocron.so/uploads/128b6c33-image.png)

### Frontend

```markup
React Native, Objective-C, Figma, TypeScript
```

To enhance my efficiency on this project, I opted in favor of using React Native, a framework I've used extensively. Prior to coding the project, I wireframed the UI in Figma, then developed a custom component library to emphasize reusability.

From creating a custom styles library, relatively similar to TailwindCSS, with appearance responsiveness, to developing custom React Native bridged packages for seamless integration with third-party applications (like FanDuel and DraftKings), this application showcases my expertise in app development and React fundamentals.

Feel free to demo the application or watch the following videos to see Tipstars in action!

#### Virtualization and feed

This application's feed utilizies ideologies commonly found in other infinite scroll lists.

- **Stale While Revalidate (SWR):** SWR was initially introduced in 2010, but it gained popularity among React applications after Vercel developed its own react hook package. Tipstars utilizes its custom natively-bridged SWR hook to ensure app readiness upon startup or unsuspense. Unlike the SWR hook, the custom native module aims to use the device's native storages, instead of storing cache in the JavaScript layer.
- **Virtualization:** As an application's content grows, virtualization becomes vital. Tipstars focuses on maximizing the FPS of both the JavaScript layer and native layer, simultaneously. Even with constant-looped animations, there is no drop in FPS between the JavaScript and native layers. This is achieved through a deep understanding of component memorization, recyclability, and re-render management, making the process seamless.
- **Content Delivery Service (CDN):** CDNs play an important role in virtualization, surprisingly enough. Ensuring images and other attachments are readily available and cached, allows the JavaScript and native layer threads to stream more efficiently, when loading components.

_This project was designed, planned, developed, maintained for all by Mohamed Adam._
