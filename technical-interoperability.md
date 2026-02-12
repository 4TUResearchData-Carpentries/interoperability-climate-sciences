---
title: "Technical interoperability: API"
teaching: 60 # aching time in minutes
exercises: 60 # exercise time in minutes
---
    
:::::::::::::::::::::::::::::::::::::: questions 

- What is technical interoperability in the context of research data infrastructures?

- What is a REST API, and how does it enable machine-to-machine interaction?

- Why are APIs a key building block for interoperable research workflows?

- How can datasets be created, updated, and submitted for review using the 4TU.ResearchData REST API?

- How do APIs depend on structural and semantic interoperability to function reliably?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain technical interoperability and its role alongside structural and semantic interoperability.

- Describe how REST APIs enable automated and scalable data exchange.

- Identify core API concepts such as HTTP methods, JSON serialization, identifiers, and versioning.

- Interact programmatically with a data repository using the 4TU.ResearchData REST API.

- Submit, update, and manage dataset metadata through an API-based workflow.

::::::::::::::::::::::::::::::::::::::::::::::::

## Technical interoperability and APIs

Technical interoperability concerns how systems communicate.

While structural interoperability ensures that data follow predictable formats (e.g. NetCDF arrays and dimensions) and semantic interoperability ensures shared meaning (e.g. CF conventions), technical interoperability ensures that software systems can reliably exchange data and metadata without human intervention. In practice, technical interoperability is achieved through standardized protocols, of which APIs are the most prominent example.


An API (Application Programming Interface) defines how one system can request services or data from another system in a precise, machine-readable way.

APIs enable:

- Automated data retrieval

- Programmatic publication of datasets

- Distributed processing pipelines
- Machine-to-machine workflows

- Cross-institutional integration of infrastructures

## REST APIs: core concepts

A REST API is an application programming interface (API) that conforms to the design principles of the representational state transfer (REST) architectural style, a style used to connect distributed hypermedia systems. REST APIs are sometimes referred to as RESTful APIs or RESTful web APIs.

Most modern research data infrastructures expose REST APIs, which rely on widely adopted web standards.

Key concepts include:

- HTTP as the transport protocol (HTTP- HyperText Transfer Protocol)


- JSON as a structured, machine-readable representation of metadata (JSON- JavaScript Object Notation)

- Stable identifiers for datasets and resources

- Versioning to support long-term reuse and evolution of services

- Self-describing endpoints, where responses contain sufficient metadata to be interpreted by machines

- REST methods:

    - GET – retrieve data or metadata

    - POST – create new resources

    - PUT / PATCH – update existing resources

    - DELETE – remove resources



## Relation to structural and semantic interoperability

APIs do not operate in isolation. APIs depend on structural interoperability: JSON responses must follow well-defined schemas.

APIs depend on semantic interoperability: Metadata fields, vocabularies, and controlled terms ensure that machines interpret content consistently.

Without structural and semantic agreement, an API may be technically functional but scientifically meaningless.

### Why APIs matter for climate and atmospheric sciences

Climate and atmospheric research is inherently computational and distributed:

- Data volumes are large and continuously growing

- Analyses are increasingly automated

- Workflows span institutions, models, sensors, and repositories
- Reproducibility requires programmatic access

APIs make it possible to build end-to-end interoperable workflows, from data acquisition to publication and reuse, without manual intervention.

## 4TU.ResearchData REST API

The 4TU.ResearchData repository provides a REST API that allows programmatic access to its datasets and metadata. This enables researchers to integrate data publication and retrieval into their automated workflows.

The documentation for the 4TU.ResearchData REST API can be found at: https://djehuty.4tu.nl/

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


### Add parameters to the same endpoint to filter results 

- Open the documentation: https://djehuty.4tu.nl/ (in-development)


```bash 

curl "https://data.4tu.nl/v2/articles?limit=2&published_since=2025-05-01" > data.json
```

```bash
curl "https://data.4tu.nl/v2/articles?limit=2&published_since=2025-05-01" | jq
```


::::::::::::: challenge

## Request 10 datasets published from January 1st 2025



::::::::::::::solution

```bash
curl "https://data.4tu.nl/v2/articles?item_type=3&limit=10&published_since=2025-01-01" | jq
```


::::::::::::::::::::::
::::::::::::::::::::::::


:::::::::::: challenge

## Get 10 software records published after 01-01-2025 



::::::::::::::solution

```bash
curl "https://data.4tu.nl/v2/articles?item_type=9&limit=1&published_since=2025-01-01" | jq
```


::::::::::::::::::::::
::::::::::::::::::::::::




### Get information per dataset ID

```bash
curl "https://data.4tu.nl/v2/articles/03c249d6-674c-47cf-918f-1ef9bdafe749" | jq  # /v2/articles/uuid
```

### Get all the files per dataset ID 

```bash
curl "https://data.4tu.nl/v2/articles/03c249d6-674c-47cf-918f-1ef9bdafe749/files" | jq # /v2/articles/uuid/files

```

:::::::::::::::: instructor

Open this link : https://data.4tu.nl/v2/articles/03c249d6-674c-47cf-918f-1ef9bdafe749/files in the browser to check the uuid of a file to download (the readme, the last file) for the following step.


:::::::::::::::::::::::::::

### How to download a specific file

```bash
# print the readme file in the screen 

curl "https://data.4tu.nl/file/03c249d6-674c-47cf-918f-1ef9bdafe749/20382d28-0ed9-4f9b-918a-936a2c6f8f76" # /file/article-uuid/file-uuid

#| Command                | Behavior                               |
#| ---------------------- | -------------------------------------- |
#| `curl URL`             | Prints file to screen (no saving)      |
#| `curl -O URL`          | Downloads and saves with original name |
#| `curl -o filename URL` | Downloads and saves with custom name   |
#| `curl -L -O URL`       | Follows redirects and saves file       |
#| `curl -C - -O URL`     | Resumes an interrupted download        |


```


###  Search Datasets by Keyword

```bash
curl --request POST  --header "Content-Type: application/json" --data '{ "search_for": "aerospace" }' https://data.4tu.nl/v2/articles/search | jq


```

```bash
curl --request POST  --header "Content-Type: application/json" --data '{ "search_for": "architecture" }' https://data.4tu.nl/v2/articles/search | jq
```

#### Create the .env file and copy your private token there

```bash

echo 'API_TOKEN="your_token_here"' > .env

echo "Token loaded: ${API_TOKEN:0:5}..."

source .env

```

## Upload Datasets (POST Requests)


### Basic Upload of metadata to a draft dataset

```bash
curl -X POST https://next.data.4tu.nl/v2/account/articles  --header "Authorization: token ${API_TOKEN_NEXT}" --header "Content-Type: application/json" --data '{ "title": "Dataset RDM session", "authors": [{ "first_name": "Leila", "full_name": "Leila Inigo", "last_name": "Inigo", "orcid_id": "0000-0003-4324-5350" }]  }' | jq
```

### Adding an author to the draft dataset 
- first we need to copy the uuid of the draft dataset created in the previous step in the next.data.4tu.nl website

```bash
curl -X POST "https://next.data.4tu.nl/v2/account/articles/UUID/authors" --header "Authorization: token ${API_TOKEN_NEXT}" --header "Content-Type: application/json" --data '{ "authors": [{ "first_name": "John", "full_name": "Doe", "last_name": "Doe", "orcid_id": "0000-0303-4524-5350" }]  }' | jq
```

### Upload Using YAML Metadata

- They need to download the example_metadata.yaml file 
`curl -o example_metadata.yaml  https://raw.githubusercontent.com/4TUResearchData-Carpentries/WebAPI4RDM/refs/heads/main/Lesson_development/example_metadata.yaml`


#### Upload to next server


```bash
yq '.' example_metadata.yaml | curl -X POST https://next.data.4tu.nl/v2/account/articles -H "Authorization: token ${API_TOKEN_NEXT}" -H "Content-Type: application/json" -d @-
```

#### Upload to the production server

```bash
yq '.' example_metadata.yaml | curl -X POST https://data.4tu.nl/v2/account/articles -H "Authorization: token ${API_TOKEN}" -H "Content-Type: application/json" -d @-
```


#### Command explanation:

`yq '.' example_metadata.yaml` : Converts example_metadata.yaml into JSON

- yq is a command-line tool to read/manipulate YAML (like jq is for JSON).

- `'.'` means "read the full YAML structure as-is".


`-d @-`

- `-d` sends data in the body of the POST request.

- `@-` means: read the request body from stdin (standard input), i.e., the piped-in JSON from yq.



##### Now try to submit it and realize that need a least a file to  submit for review


### File upload 

```bash
curl -X POST "https://next.data.4tu.nl/v3/datasets/dataset-id/upload"   --header "Authorization: token ${API_TOKEN_NEXT}"   --header "Content-Type: multipart/form-data"   -F "file=@absolute-path-to-the-file"
```

#### Now lets take the uuid of the draft just created in the previous example and put it in the endpoint

- For tha data , first download the data using curl from github

`curl -O "https://raw.githubusercontent.com/4TUResearchData-Carpentries/WebAPI4RDM/refs/heads/main/Lesson_development/data_files/test_a.csv"  `


```bash
curl -X POST "https://next.data.4tu.nl/v3/datasets/UUID/upload"   --header "Authorization: token ${API_TOKEN_NEXT}"   --header "Content-Type: multipart/form-data"   -F "file=@ABSOULTE_PATH2FILE"
```

#### FIle upload with strict check for empty files and duplicates

```bash
MD5SUM=$(md5sum "ABSOULTE_PATH2FILE" | awk '{print $1}')
```

```bash
curl -X POST "https://next.data.4tu.nl/v3/datasets/UUID/upload?strict_check=1&md5=${MD5SUM}"   --header "Authorization: token ${API_TOKEN_NEXT}"   --header "Content-Type: multipart/form-data"   -F "file=@ABSOULTE_PATH2FILE"
```

the response of this is that the resource is already available and stops there


### Submit for review 

```bash
yq '.' example_metadata.yaml | curl -X PUT "https://next.data.4tu.nl/v3/datasets/UUID/submit-for-review" --header "Authorization: token ${API_TOKEN_NEXT}" --header "Content-Type: application/json" --data @-
```






:::::::::: keypoints


- Technical interoperability enables reliable machine-to-machine communication by defining standardized protocols through which software systems can exchange data and metadata without human intervention.

- REST APIs implement technical interoperability using web standards, relying on HTTP methods, predictable endpoints, JSON serialization, stable identifiers, and versioning to ensure scalable and reproducible interactions.

- APIs depend on structural and semantic interoperability: JSON payloads must follow well-defined schemas, and metadata must use shared vocabularies and conventions to be scientifically meaningful.

- Command-line tools such as `curl` provide a practical interface to APIs, allowing researchers to issue HTTP requests, inspect responses, and integrate API interactions into scripts and automated workflows.

- The 4TU.ResearchData REST API enables programmatic dataset management, supporting discovery, metadata creation and updates, file uploads, and submission for review as part of interoperable research pipelines.

::::::::::::::::::::
