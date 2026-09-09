---
title: "Technical interoperability: API"
teaching: 60 # teaching time in minutes
exercises: 60 # exercise time in minutes
---
    
:::::::::::::::::::::::::::::::::::::: questions 

- What is technical interoperability in research data infrastructures?
- What is a REST API?
- How do APIs enable machine-to-machine workflows?
- How do APIs depend on structural and semantic interoperability?
- How can we programmatically manage datasets using the 4TU.ResearchData API?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

By the end of this episode, learners will be able to:

- Define APIs as mechanisms of technical interoperability.
- Explain core API concepts 
- Understand the relevance of the use of APIs for research
- Interact with a repository WEB API using curl.
- Create and manage dataset metadata programmatically.


::::::::::::::::::::::::::::::::::::::::::::::::

## Technical interoperability and APIs

Technical interoperability concerns how systems access and exchange information.

While structural interoperability ensures that data follow predictable formats (e.g. NetCDF arrays and dimensions) and semantic interoperability ensures shared meaning (e.g. CF conventions), technical interoperability ensures that software systems can reliably exchange data and metadata without human intervention. In practice, technical interoperability is achieved through standardized protocols, of which APIs are the most prominent example.


An API (Application Programming Interface) defines how one system can request services or data from another system in a precise, machine-readable way.

APIs enable:

- Automated data retrieval

- Programmatic publication of datasets

- Distributed processing pipelines
- Machine-to-machine workflows

- Cross-institutional integration of infrastructures

Then:

> APIs operationalize technical interoperability.

## APIs and Web APIs: core concepts

An **API (Application Programming Interface)** defines how one software system can interact with another in a structured and predictable way.

APIs can take different forms depending on where and how systems communicate. For example:

* **Library or programming APIs** allow software code to interact with functions, classes, or modules provided by a software package.
* **Operating system APIs** allow applications to interact with system resources such as files, memory, or hardware.
* **Database APIs** allow applications to query and modify information stored in databases.
* **Web APIs** allow applications and services to communicate over a network, usually using standard web technologies.

In research data infrastructures, **Web APIs are particularly important** because repositories, data services, analysis platforms, and other distributed systems often need to exchange data and metadata across institutional and technical boundaries.

For this reason, this lesson focuses primarily on **Web APIs**.

### What is a Web API?

A **Web API** provides a machine-accessible interface through which one system can request data or services from another over the web.

Web APIs are an important mechanism for **technical interoperability** because they allow systems to exchange data and metadata programmatically, without requiring a person to interact manually with a website.

For example, a research data repository may provide a Web API that allows software to:

* search for datasets;
* retrieve dataset metadata;
* access or download data;
* publish new datasets;
* update existing metadata;
* connect repository services to other research infrastructures.

In this way, an API can transform a repository from a platform designed mainly for human interaction into **programmable research infrastructure**.

### Main concepts of a Web API

A Web API usually defines:

* **Endpoints** — addresses that identify resources or services provided by the API.
* **Requests** — messages sent by a client to ask for information or trigger an operation.
* **HTTP methods** — indicate the type of operation requested.
* **Parameters** — provide additional information to refine or control a request.
* **Representations** — structured formats used to exchange information, commonly JSON.
* **Responses** — messages returned by the API, containing requested information and information about whether the request succeeded.
* **Authentication and authorization** — mechanisms used to identify users or software and determine which operations they are allowed to perform.

Most Web APIs use **HTTP (Hypertext Transfer Protocol)** for communication.

Common HTTP methods include:

* `GET` — retrieve information;
* `POST` — submit information, often to create a new resource;
* `PUT` — replace or update a resource;
* `PATCH` — partially update a resource;
* `DELETE` — remove a resource.

Responses commonly contain structured, machine-readable data. **JSON (JavaScript Object Notation)** is one of the most widely used formats for exchanging information through Web APIs.

For example, instead of presenting dataset metadata only as a webpage for humans to read, a Web API may return the same information as structured JSON that software can process automatically.

:::: callout

### Web APIs in research data infrastructures

For research data services, additional design characteristics can improve interoperability and long-term reuse, including:

* **stable identifiers** for datasets and other resources;
* **documented API specifications** describing how requests and responses work;
* **consistent machine-readable metadata**;
* **API versioning** so services can evolve without unexpectedly breaking existing workflows;
* **standardised error and status responses** that software can interpret automatically.

These features make it easier to connect repositories with research software, automated workflows, and distributed infrastructures.

**Web APIs provide a machine-to-machine interface through which technical interoperability can be put into practice.**

::::::::::::::::::::


### Relation to structural and semantic interoperability

APIs do not operate in isolation. APIs depend on structural interoperability: JSON responses must follow well-defined schemas.

APIs depend on semantic interoperability: Metadata fields, vocabularies, and controlled terms ensure that machines interpret content consistently.

Without structural and semantic agreement, an API may be technically functional but scientifically meaningless.

### Relevance of APIs for climate and atmospheric sciences

Climate and atmospheric research often depends on **large, distributed, and continuously updated datasets** produced by satellites, radar systems, weather stations, numerical models, and research infrastructures.

In practice, researchers may need to:

* retrieve observations from remote data services;
* query metadata to find datasets for a specific location, variable, or time period;
* combine data from multiple institutions or repositories;
* trigger automated processing or analysis workflows;
* publish processed datasets and metadata back to a repository;
* connect research software to external services without manually downloading and uploading files.

For example, a workflow could automatically:

1. query a data service for radar observations;
2. retrieve metadata and identify the required files;
3. process the selected data in Python;
4. generate derived products;
5. publish the results and associated metadata to a research repository.

APIs make these steps **programmable and repeatable**. This is particularly important when workflows need to be rerun regularly, applied to many datasets, or shared with other researchers.

For climate and atmospheric sciences, APIs therefore support technical interoperability by allowing **data services, analysis tools, models, and repositories to exchange information automatically as part of the same workflow**.


::::::::::::::::::::::::::::::::::: challenge

## APIs and Technical interoperability : True or False?

1. A Web API can allow software to retrieve data from a repository without a user manually interacting with the repository website.
2. All APIs are Web APIs.
3. GET, POST, PUT, PATCH, and DELETE are HTTP methods commonly used by Web APIs.
4. Web API must return JSON in order to be technically interoperable.
5. If two systems can exchange data through an API, this automatically means that they interpret the scientific meaning of the data in the same way.
6. A technically functional API can still be difficult to reuse if its responses do not follow a predictable structure.
7. Stable identifiers, documented endpoints, and API versioning can make research workflows easier to reproduce and maintain.
8. APIs can support automated workflows that connect data services, analysis software, and research repositories.
9. APIs remove the need for structural and semantic interoperability.


:::::::::::::::::::::::: solution

1. True — Web APIs enable programmatic access to data and services without requiring manual interaction with a website.
2. False — Web APIs are one type of API. Other types include programming-library, operating-system, and database APIs.
3. True — These are HTTP methods commonly used by Web APIs to request different operations.
4. False — JSON is widely used, but Web APIs can exchange information using other machine-readable formats.
5. False — Technical interoperability enables systems to exchange information, but shared scientific meaning depends on semantic interoperability.
6. True — Predictable schemas and structured responses are important for software to process API responses reliably.
7. True — These characteristics help software workflows remain understandable, reusable, and less likely to break when services evolve.
8. True — APIs can connect different components of a computational workflow, for example retrieving observations, processing them, and publishing derived data.
9. False — APIs depend on structural and semantic interoperability. Data still need predictable structures and shared meaning to be reused correctly.


:::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::


## Hands on 4TU.ResearchData WEb API

The 4TU.ResearchData repository provides a REST API that allows programmatic access to its datasets and metadata. This enables researchers to integrate data publication and retrieval into their automated workflows.

The documentation for the 4TU.ResearchData REST API can be found at: https://djehuty.4tu.nl/


::::::::::::::::::::instructor

This section could be shown as a live demo or a step-by-step walkthrough, depending on the audience and format of the lesson. The key is to demonstrate how to interact with the API using command-line tools like `curl`, and to explain the underlying concepts of RESTful APIs as you go through the examples.

:::::::::::::::::

### Query datasets using the 4TU.ResearchData API

Get datasets or software deposited in 4TU (via curl)

```bash

curl -X GET "https://data.4tu.nl/v2/articles"  | jq

```

### What is curl?

curl stands for **Client URL**. 

It’s a command-line tool that allows you to transfer data to or from a server using various internet protocols, most commonly HTTP and HTTPS.

It is especially useful for making API requests — you can send GET, POST, PUT, DELETE requests, upload or download files, send headers or authentication tokens, and more.

### Why curl works for APIs

REST APIs are based on the HTTP protocol, just like websites. When you visit a webpage, your browser sends a GET request and displays the HTML it gets back. When you use curl, you do the same thing, but in your terminal. For example: 

`curl https://data.4tu.nl/v2/articles` This sends an HTTP GET request to the 4TU.ResearchData API.

### Key reasons why curl is used:

It’s built into most Linux/macOS systems and easily installable on Windows.

Scriptable: usable in bash scripts, notebooks, automation.

Supports headers, query parameters, tokens, POST data, etc.

Can output to files (>, -o, -O) or pipe to processors like jq.

### How to download a specific file using `curl`

| Command                | Behavior                               |
| ---------------------- | -------------------------------------- |
| `curl URL`             | Prints file to screen (no saving)      |
| `curl -O URL`          | Downloads and saves with original name |
| `curl -o filename URL` | Downloads and saves with custom name   |
| `curl -L -O URL`       | Follows redirects and saves file       |
| `curl -C - -O URL`     | Resumes an interrupted download        |

### Add parameters to the same endpoint to filter results 

- Open the documentation: https://djehuty.4tu.nl/ (in-development)

:::::::::::::::::::::::::::::::::: challenge

## Practicing  API calls with `curl`

1. Show in the terminal the metadata of 2 datasets published since May 1st 2025 using `curl` and `jq` to format the output.
2. Save the information of 2 datasets published since May 1st 2025 using `curl` to a file called `data.json` in the current directory.
3. Show in the screen the metadata of 10 software published since January 1st 2025.



:::::::::::::::::::::::::: solution

```bash 

curl "https://data.4tu.nl/v2/articles?limit=2&published_since=2025-05-01" | jq

curl "https://data.4tu.nl/v2/articles?limit=2&published_since=2025-05-01" > data.json

curl "https://data.4tu.nl/v2/articles?item_type=9&limit=10&published_since=2025-01-01" | jq
```

:::::::::::::::::::::::::::


:::::::::::::::::::::::::::::::::::::::::::


### Get information per dataset ID 

You get the dataset information by running the call to `/v2/articles/uuid`: 

```bash 

curl "https://data.4tu.nl/v2/articles/03c249d6-674c-47cf-918f-1ef9bdafe749" | jq 

``` 

### Get all the files per dataset ID

You get the dataset information by running the call to `/v2/articles/uuid/files`

```bash

curl "https://data.4tu.nl/v2/articles/03c249d6-674c-47cf-918f-1ef9bdafe749/files" | jq 

```

:::::::::::::::: instructor

Open this link : https://data.4tu.nl/v2/articles/03c249d6-674c-47cf-918f-1ef9bdafe749/files 
in the browser to check the uuid of a file to download (the readme, the last file) 
for the following step.

:::::::::::::::::::::::::::



####  Search Datasets by Keyword

```bash

curl --request POST  --header "Content-Type: application/json" --data '{ "search_for": "atmospheric" }' https://data.4tu.nl/v2/articles/search | jq


```

```bash

curl --request POST  --header "Content-Type: application/json" --data '{ "search_for": "netcdf" }' https://data.4tu.nl/v2/articles/search | jq

```

The 4TU.ResearchData API also supports the creation, the metadata update , the file upload and submission for review tasks. For more information visit the documentation page [djehuty.4tu.nl](https://djehuty.4tu.nl/) 

:::::::::: keypoints

- APIs operationalize technical interoperability by enabling standardized machine-to-machine interaction.

- Web APIs use HTTP methods, predictable endpoints, JSON representations, stable identifiers, and authentication mechanisms.

- APIs depend on structural interoperability (schemas) and semantic interoperability (controlled vocabularies).

- Command-line tools such as curl provide direct access to API functionality and enable automation.

- The 4TU.ResearchData API supports full dataset lifecycle management: discovery, creation, metadata update, file upload, and submission for review.

::::::::::

