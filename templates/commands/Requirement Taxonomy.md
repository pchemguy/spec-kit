## Requirement Types

  - **Functional**
    Functional requirements define the specific features and operations a system must perform to meet business and user needs. They describe what the system should do and how it interacts with users or other systems.
    - Focus on the behavior and functionality of the system.
    - Represent features that can be directly observed and tested in the final product.
    - Examples include
        - user authentication
        - data processing
        - search functionality
        - payment processing
        - report generation
  - **Non-Functional**
    Non-functional requirements (NFRs) define how a system should operate, focusing on performance, reliability, and user experience rather than specific features. They ensure the system is efficient, secure, and maintainable over time.
    - ***Performance:** speed and responsiveness
    - **Security:** protection against unauthorized access
    - **Usability:** ease of use
    - **Reliability:** system stability and availability
    - **Scalability:** ability to handle growth
    - **Maintainability:** ease of updates and fixes
    - **Portability:** ability to run in different environments
- **Extended**
  Extended requirements define additional capabilities or considerations that enhance the system but are not part of the core functional features. These requirements help improve monitoring, reliability, and future expansion of the system.
    - **Logging:** recording system activities and errors for debugging and analysis
    - **Monitoring & Alerting:** tracking system health, performance, and failures
    - **Analytics:** collecting usage data to understand user behavior and system performance
    - **Backup & Disaster Recovery:** ensuring data can be restored in case of failures
    - **Rate Limiting:** controlling the number of requests to prevent system overload or abuse
    - **Feature Flags / A-B Testing:** enabling controlled feature releases and experiments

## Scenario Types
  
> [!NOTE] Scenario Taxonomy
> 
> **Scenarios** describe _behavioral flows_ — sequences of actions/events that occur under some conditions.
> They are _contextualized behaviors_, such as user journeys or system interactions.

  - Primary
  - Alternate
  - Exception / Error
  - Recovery

## Context
  
  - Dependencies
  - Assumptions

## Requirement Quality

- **Completeness**: Are all necessary requirements present?
- **Clarity/Ambiguity**: Are requirements unambiguous and specific?
- **Consistency/Conflict**: Do requirements align with each other?
- **Coverage**: Are all scenarios/edge cases addressed?
- **Measurability**: Can requirements be objectively verified?
