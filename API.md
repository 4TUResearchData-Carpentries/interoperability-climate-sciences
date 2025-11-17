---
title: "Publish datasets via REST API"
teaching: 60 # aching time in minutes
exercises: 60 # exercise time in minutes
---
    
:::::::::::::::::::::::::::::::::::::: questions 

- What is a REST API?
- Why API are an example of interoperability?
- How to create a draft dataset using the 4TU.ResearchData Rest API?
- How to submit for review a dataset using the 4TU.ResearchData Rest API

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand why APIs are interoperables protocols?
- Know how to submit data to a data repository via its API.

::::::::::::::::::::::::::::::::::::::::::::::::

Content

✔ APIs = technical interoperability layer

APIs enable:
    • Automated data retrieval
    • Publication
    • Distributed pipelines
    • Machine-to-machine workflows
    • Cross-institutional integration
APIs are to infrastructures what CF conventions are to NetCDF:
→ shared semantics + shared rules.

✔ Key API concepts
    • HTTP
    • REST verbs (GET/POST/PUT/DELETE)
    • JSON as structural metadata
    • Stable identifiers
    • Versioning

✔ What APIs must comply with
    • HTTP as transport
    • Predictable URL patterns
    • JSON serialization
    • Metadata standards (schema.org, DCAT, CF, ISO19115)
    • Self-describing endpoints

Hands-on Exercises
    1. Query datasets via 4TU API
    2. Retrieve metadata and interpret JSON
    3. Publish a dataset programmatically
    4. Update metadata
    5. Integration workflow:
    
Sensor API → enrich metadata (CF-like) → publish to 4TU
Reinforces technical interoperability.



:::::::::: keypoints

- APIs (Application Programming Interfaces) are interoperable protocols that enable machine-to-machine communication, allowing automated data retrieval, publication, and integration across distributed systems.
- REST APIs use standard HTTP methods (GET, POST, PUT, DELETE) and JSON serialization to provide predictable and self-describing endpoints, facilitating seamless interaction with data repositories.
- By adhering to established metadata standards and versioning practices, APIs ensure consistent and reliable access to datasets, supporting scalable and interoperable workflows in climate science.


::::::::::::::::::::
