
## What is SDLC?
- `SDLC` stands for **Software Development Life Cycle**. It is a structured process followed by software development teams to `design`, `develop`, `test` and `deploy software systems` efficiently and with high quality.

### ✅ Phases of SDLC
1. Requirement Gathering & Analysis.
   -
   - Understand what the users need
   - Gather functional and non-functional requirements.
   - Outcome: Requirements Specification Document.
  
2. Planning
   -
   - Define scope, resources, timelines, cost and risks.
   - Create a project plan.
  
3. Design
   -
   - Convert requirements into a blueprint.
   - Includes High-Level Design(HLD) and Low-Level Design(LLD).
   - Outcome: Design Documents, Architecture.
  
4. Development
   -
   - Actual coding of the software.
   - Developers build the system based on the design
  
5. Testing
   -
   - Verify that the software works as expected.
   - Types include unit testing, integration testing, system testing, and user acceptance testing (UAT)
  
6. Deployment
   -
   - Release the software to the production environment.
   - Can be a full rollout or in stages (beta release).
  
7. Maintenance
   -
   - Fix bugs, make improvements, update features.
   - Ongoing support for the software product.
  
### ✅ Common SDLC Models
| Model         | Description                                                                 |
| ------------- | --------------------------------------------------------------------------- |
| **Waterfall** | Linear and sequential. Each phase must be completed before the next starts. |
| **Agile**     | Iterative and incremental. Work is delivered in sprints.                    |
| **V-Model**   | Testing is planned parallel to development.                                 |
| **Spiral**    | Combines design and prototyping in iterations. Focuses on risk analysis.    |
| **Iterative** | Develop a version, get feedback, and improve in cycles.                     |

-------------------------------------------------------------------------------------------------------------------------
### ✅ Agile Model (Recommended)
#### 🚀 Why Agile is Suitable:
- Iterative Development: You deliver in small sprints (2–4 weeks), which allows for continuous feedback and improvement.
- Frequent Delivery: Working software is delivered frequently, giving the client quick visibility into progress.
- Client Collaboration: Agile emphasizes regular interaction with the client and stakeholders.
- Flexibility: You can adapt to changing requirements, which is common in real-world projects.


#### 📌 When You Might Choose Other Models:
| Model         | When to Use                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| **Waterfall** | Fixed scope, one-time delivery, no scope for changes. Example: Government projects. |
| **V-Model**   | When testing is as critical as development (e.g., healthcare or aviation software). |
| **Spiral**    | High-risk projects needing heavy prototyping and risk analysis.                     |
| **Hybrid**    | Mix of Agile and Waterfall for large enterprises needing structure and agility.     |

#### 📦 Typical Agile Stack in Java Full Stack Projects:
| Layer         | Tech Stack Example                             |
| ------------- | ---------------------------------------------- |
| Frontend      | React, Angular, or Vue                         |
| Backend       | Spring Boot / Spring MVC                       |
| Database      | MySQL / PostgreSQL / MongoDB                   |
| CI/CD         | Jenkins, Docker, GitHub Actions                |
| Project Tools | JIRA (for Agile boards), Confluence, Bitbucket |
| Testing       | JUnit, Mockito, Selenium                       |

------------------------------------------------------------------------------------------------------------------
- To calculate and manage the time required for a particular development task, developers and project teams in the IT industry typically use a combination of the following tools:
#### ✅ 1. JIRA (by Atlassian) – Most Common
##### 🔧 Used For:
- Time estimation (Story Points / Hours)
- Sprint planning and task breakdown
- Logging time spent (Time Tracking)
- Generating burndown charts, velocity reports

##### 📌 How it Helps:
- You estimate time during sprint planning (Original Estimate)
- Log actual hours worked (Time Spent)
- Compare estimated vs actual time
- Monitor team performance over time

##### ✅ Example in JIRA:
| Task           | Original Estimate | Time Spent | Remaining |
| -------------- | ----------------- | ---------- | --------- |
| Build REST API | 6h                | 5h         | 1h        |
| React UI Form  | 4h                | 4h         | 0h        |
| Unit Tests     | 3h                | 2h         | 1h        |

##### 🔍 How to Estimate Time Effectively:
- Break tasks into small units (2–8 hours)
- Use experience and past sprint velocity
- Discuss with team during planning
- Apply buffers for review and rework
- Track actual time for future accuracy

---------------------------------------------------------------------------------------
## What type of project are you working on?
### ✅ 1. Based on Client Type
| Project Type               | Description                                                                     |
| -------------------------- | ------------------------------------------------------------------------------- |
| **Client-Based (Service)** | Delivered to a specific external client (e.g., Accenture delivering for a bank) |
| **Product-Based**          | Built for mass market or internal use (e.g., SaaS platforms like Salesforce)    |

### ✅ 2. Based on Business Domain
| Domain              | Examples                                 |
| ------------------- | ---------------------------------------- |
| **Banking/Finance** | Loan management, credit scoring          |
| **E-commerce**      | Shopping apps, cart and payment gateways |
| **Healthcare**      | Patient record systems, diagnostics      |
| **Education**       | LMS, online learning platforms           |
| **Government**      | Public portals, document digitization    |
| **Telecom**         | Billing, network management              |

### ✅ 3. Based on Architecture
| Type              | Description                             |
| ----------------- | --------------------------------------- |
| **Monolithic**    | One large codebase (older projects)     |
| **Microservices** | Modules communicate over APIs           |
| **Serverless**    | Function-as-a-Service (like AWS Lambda) |
| **Event-driven**  | Uses messaging queues or events         |

### ✅ 4. Based on Development Type
| Type                    | Description                             |
| ----------------------- | --------------------------------------- |
| **Web Application**     | React + Spring Boot full-stack          |
| **Mobile Application**  | Android/iOS with Java/Kotlin or Flutter |
| **Desktop Application** | Swing/JavaFX/.NET-based                 |
| **API-Only**            | Backend-only services                   |

### ✅ 5. Based on Delivery Methodology (SDLC)
| Type               | Description                          |
| ------------------ | ------------------------------------ |
| **Agile**          | Iterative with sprints               |
| **Waterfall**      | Linear and sequential                |
| **DevOps Enabled** | CI/CD pipelines, continuous delivery |
| **Hybrid**         | Combination of Waterfall & Agile     |
