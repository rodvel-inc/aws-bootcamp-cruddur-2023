# Week 0 — Billing and Architecture

## Activities

The bootcamp starts with an explanation of the concepts, goals, scope and tools that will be reached and used throughout. 

### Architecting your Cloud
| Concept                          | Description                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------|
| What is good architecture?       | It is good when it meets the requirements.                                                         |
| What is a requirement?           | - It is something that the project must achieve in the end.                                        |
|                                  | - It can be technical or business oriented.                                                        |
| Examples of requirements         | - Meets certain ISO standards                                                                      |
|                                  | - 99,9% uptime                                                                                     |
|                                  | - Users can do a specific task                                                                     |
| Good architecture also addresses | - Risks                                                                                            |
|                                  | - Assumptions                                                                                      |
|                                  | - Constraints                                                                                      |
| What are risks?                  | Things and situations that can prevent the project from being successful and have to be mitigated: |
|                                  | - Single Points of Failure                                                                         |
|                                  | - User Commitment (C-Level, sponsors, developers, etc.)                                            |
|                                  | - Late delivery                                                                                    |
| What are assumptions?            | They are factors held as true for the planning and implementation phases:                          |
|                                  | -	Sufficient network bandwidth                                                                    |
|                                  | -	Stakeholders will be available to make decisions                                                |
|                                  | -	Budget is approved                                                                              |
| What are constraints?            | Things that limit the development of the project:                                                  |
|                                  | - Time                                                                                             | 
|                                  | - Budget                                                                                           |
|                                  | - Vendor selection                                                                                 |

### What to do with R(isks) A(ssumptions) C(onstraints)?
Well, these can be used as a foundation for the creation of the different diagrams that can be made. The following are some of these diagrams:

| Concept                      |Description                                                                                                                         |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
|Conceptual Design (High level)|- It is not a technical design                                                                                                       |
|                              |- It is created by business stakeholders and architects                                                                              |
|                              |- It organizes and defines concepts and rules                                                                                        |
|                              | - “Napkin design”                                                                                                                   |
|Logical Design                |- Defines how the system should be implemented: it must have DNS resolution, it must use a noSQL db, etc…without names or sizes yet. |
|                              | - This design should be transferable to multiple alternatives of physical designs.                                                  | 
|Physical Design               | It is a representation of the actual thing that will be built:                                                                      |
|                              | - IP addresses of the EC2 instances                                                                                                 |
|                              | - ARN for the API Gateway, etc.                                                                                                     |

### Diagrams

## Conceptual
Link to the diagram [here](https://lucid.app/lucidchart/15b4a5b2-ec2c-4d83-99d8-0460fbf2922a/edit?viewport_loc=3010%2C420%2C2219%2C1041%2C0_0&invitationId=inv_b4f59066-61e6-485e-9bec-f016a8d141d4).

![Conceptual design](/_docs/assets/conceptual.jpg "Conceptual design")

## Logical
Link to the diagram [here](https://lucid.app/lucidchart/c333b586-db78-4a7d-b6dd-8f22d17a8c83/edit?viewport_loc=-533%2C-66%2C4992%2C2343%2CV3dxByeoB0H6&invitationId=inv_fe3cd5fc-e7fa-44f8-ae98-850c76912554).

![Logical design](/_docs/assets/logical.jpeg "Logical design")

