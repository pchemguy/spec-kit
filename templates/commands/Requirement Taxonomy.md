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
    - **Performance:** speed and responsiveness
    - **Security:** protection against unauthorized access
    - **Usability:** ease of use
    - **Reliability:** system stability and availability
    - **Scalability:** ability to handle growth
    - **Maintainability:** ease of updates and fixes
    - **Portability:** ability to run in different environments
- **Cross-Cutting Concerns (CCC)**
  Define system-wide capabilities and constraints that affect multiple features and components, e.g.:  
    - **Logging:** recording system activities and errors for debugging and analysis
    - **Monitoring & Alerting:** tracking system health, performance, and failures
    - **Analytics:** collecting usage data to understand user behavior and system performance
    - **Backup & Disaster Recovery:** ensuring data can be restored in case of failures
    - **Rate Limiting:** controlling the number of requests to prevent system overload or abuse
    - **Feature Flags / A-B Testing:** enabling controlled feature releases and experiments

## Scenario Types
  
**Scenarios** describe _behavioral flows_ — sequences of actions/events that occur under some conditions.
They are _contextualized behaviors_, such as user journeys or system interactions.

  - Primary
  - Alternate
  - Exception / Error
  - Recovery

## Context
  
  - Dependencies
  - Assumptions



| Quality Dimension | Defect Marker  | Key Question                                                                           | Problem                                 |
| ----------------- | -------------- | -------------------------------------------------------------------------------------- | --------------------------------------- |
| Clarity           | [Ambiguity]    | Are requirements unambiguous and specific?                                             | vague, unclear, or multi-interpretation |
| Consistency       | [Conflict]     | Do specification elements align without contradiction?                                 | contradictory or inconsistent           |
| Completeness      | [Gap]          | Are all required functional capabilities and non-functional qualities specified?       | missing requirement or constraint       |
| Coverage          | [Gap]          | Are requirements defined for all relevant scenarios, flows, and conditions?            | missing scenario                        |
| Measurability     | [Unverifiable] | Can requirements be objectively measured or verified?                                  | cannot be objectively verified          |
| Correctness       | [Incorrect]    | Do specification elements accurately reflect intended behavior and domain constraints? | invalid or wrong relative to context    |
| Feasibility       | [Infeasible]   | Can the requirement be realistically implemented?                                      | cannot be realistically implemented     |
| Relevance         | [Redundant]    | Are all requirements necessary and within scope?                                       | unnecessary or duplicate                |

## Quality

- **Completeness**: Are all required functional capabilities and non-functional qualities specified?  
  Requirement set completeness — applies to:
    - Functional Requirements (FR)
    - Non-Functional Requirements (NFR)
- **Clarity**: Are requirements unambiguous and specific?
- **Consistency**: Do requirements align without contradiction?
- **Coverage**: Are requirements defined for all relevant scenarios, flows, and conditions?  
  Behavioral / state space coverage — applies to:
    - scenarios (primary, alternate, error, recovery)
    - edge cases
    - state transitions
- **Measurability**: Can requirements be objectively measured or verified?
- **Relevance**: Are all requirements necessary and within scope?
- **Correctness**: Do specification elements accurately reflect intended behavior and domain constraints?
- **Feasibility**: Can requirement be realistically implemented?

## Defect Markers

Checklist items SHOULD surface specification quality issues using explicit markers:  
  
- [Ambiguity] — unclear, vague, or multi-interpretation statements  
- [Conflict] — contradictory or inconsistent specification elements  
- [Gap] — missing requirement, scenario, or constraint  
- [Assumption] — implicit condition not explicitly stated  
- [Dependency] — reliance on external or unspecified elements  
- [Incorrect] — invalid or wrong requirement relative to context  
- [Unverifiable] — cannot be objectively measured or tested  
- [Infeasible] — requirement cannot be realistically implemented under constraints
- [Redundant] — unnecessary or redundant



Completeness: “Are success criteria defined for all critical user stories? [Gap]”
Coverage: “Do success criteria cover primary and failure scenarios? [Coverage]”
Consistency: “Do success criteria align with stated requirements? [Conflict]”
Clarity: “Are success criteria free of subjective terms? [Ambiguity]”
Measurability: “Can each success criterion be objectively verified? [Unverifiable]”
Feasibility: “Are defined targets realistically achievable? [Infeasible]”
Correctness: “Do success criteria reflect actual user/business outcomes? [Incorrect]”


Each item MUST assess at least one of quality dimensions:

- **Completeness**: Are all necessary requirements present?
- **Clarity**: Are requirements unambiguous and specific?
- **Consistency**: Do requirements align with each other?
- **Coverage**: Are all scenarios/edge cases addressed?
- **Measurability**: Can requirements be objectively verified?