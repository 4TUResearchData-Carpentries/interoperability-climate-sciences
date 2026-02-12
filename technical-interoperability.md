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
- Explain core REST concepts (HTTP methods, endpoints, JSON, authentication).
- Interact with a repository API using curl.
- Create and manage dataset metadata programmatically.
- Understand the lifecycle of API-driven data publication.

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

Then:

> APIs operationalize technical interoperability.

## REST APIs: core concepts

A REST API is an application programming interface (API) that conforms to the design principles of the representational state transfer (REST) architectural style, a style used to connect distributed hypermedia systems. REST APIs are sometimes referred to as RESTful APIs or RESTful web APIs.

Most modern research data infrastructures expose REST APIs, which rely on widely adopted web standards.

An API defines:

- Endpoints (resources)
- Methods (actions) 
- Representations (JSON) 
- Authentication (identity)

In the case of repositories, APIs transform repositories from storage platforms into programmable infrastructure.

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

### Relevance of APIs for climate and atmospheric sciences

Climate and atmospheric research is inherently computational and distributed:

- Data volumes are large and continuously growing

- Analyses are increasingly automated

- Workflows span institutions, models, sensors, and repositories
- Reproducibility requires programmatic access

APIs make it possible to build end-to-end interoperable workflows, from data acquisition to publication and reuse, without manual intervention.


::::::::::::::::::::::::::::::::::: challenge

## API : True or False?

- An API guarantees semantic interoperability.

- A REST API always uses JSON.

- HTTP methods correspond to resource operations.

- Without stable identifiers, APIs break reproducibility.

- APIs replace the need for structured data formats.


:::::::::::::::::::::::: solution

False — semantics depend on shared vocabularies.

False — JSON is common but not required.

True.

True.

False — APIs depend on structure.


:::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::
## Hands on 4TU.ResearchData REST API

The 4TU.ResearchData repository provides a REST API that allows programmatic access to its datasets and metadata. This enables researchers to integrate data publication and retrieval into their automated workflows.

The documentation for the 4TU.ResearchData REST API can be found at: https://djehuty.4tu.nl/


:::::::::::::::::::: intructor

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

### Add parameters to the same endpoint to filter results 

- Open the documentation: https://djehuty.4tu.nl/ (in-development)

:::::::::::::::::::::::::::::::::: challenge

## Practicing  API calls with `curl`

1. Show in the screen the metadata of 2 datasets published since May 1st 2025 using `curl` and `jq` to format the output.
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

```bash curl 

"https://data.4tu.nl/v2/articles/03c249d6-674c-47cf-918f-1ef9bdafe749" | jq # /v2/articles/uuid 

``` 

### Get all the files per dataset ID

```bash
curl "https://data.4tu.nl/v2/articles/03c249d6-674c-47cf-918f-1ef9bdafe749/files" | jq # /v2/articles/uuid/files

```

:::::::::::::::: instructor

Open this link : https://data.4tu.nl/v2/articles/03c249d6-674c-47cf-918f-1ef9bdafe749/files in the browser to check the uuid of a file to download (the readme, the last file) for the following step.


:::::::::::::::::::::::::::



###  Search Datasets by Keyword

```bash
curl --request POST  --header "Content-Type: application/json" --data '{ "search_for": "aerospace" }' https://data.4tu.nl/v2/articles/search | jq


```

```bash
curl --request POST  --header "Content-Type: application/json" --data '{ "search_for": "architecture" }' https://data.4tu.nl/v2/articles/search | jq
```


### Managing identities 

#### Create the .env file and copy your private token there

Follow these instructions to create the .env file and copy your private token there:

- Open your profile in the 4TU.ResearchData website (www.next.data.4tu.nl) and go to the "Create API token" section , next to the "My Dashboard" section, to create a new token if you don't have one already.
- Save the new token and copy it to your clipboard.
- Create a .env file in the root of your project . 

```bash
touch .env # Create the .env file if it doesn't exist
```
- Open the .env file in a text editor and add the following line:

```bash
EXPORT API_TOKEN="your_token_here"
```
- Check that the token is loaded correctly by running:

```bash

echo 'API_TOKEN="your_token_here"' > .env

echo "Token loaded: ${API_TOKEN:0:5}..."


```
- Activate the .env file in your terminal session:

```bash
source .env
```

### Upload Datasets (POST Requests)


#### Basic Upload of metadata to a draft dataset

```bash
curl -X POST https://next.data.4tu.nl/v2/account/articles  --header "Authorization: token ${API_TOKEN_NEXT}" --header "Content-Type: application/json" --data '{ "title": "Dataset RDM session", "authors": [{ "first_name": "Leila", "full_name": "Leila Inigo", "last_name": "Inigo", "orcid_id": "0000-0003-4324-5350" }]  }' | jq
```

#### Adding an author to the draft dataset 

- First we need to copy the uuid of the draft dataset created in the previous step in the next.data.4tu.nl website

```bash
curl -X POST "https://next.data.4tu.nl/v2/account/articles/UUID/authors" --header "Authorization: token ${API_TOKEN_NEXT}" --header "Content-Type: application/json" --data '{ "authors": [{ "first_name": "John", "full_name": "Doe", "last_name": "Doe", "orcid_id": "0000-0303-4524-5350" }]  }' | jq
```

#### Upload Using YAML Metadata

- They need to download the example_metadata.yaml file 
`curl -o example_metadata.yaml  https://raw.githubusercontent.com/4TUResearchData-Carpentries/WebAPI4RDM/refs/heads/main/Lesson_development/example_metadata.yaml`


#### Upload to next server


```bash
yq '.' example_metadata.yaml | curl -X POST https://next.data.4tu.nl/v2/account/articles -H "Authorization: token ${API_TOKEN_NEXT}" -H "Content-Type: application/json" -d @-
```



- Command explanation:

`yq '.' example_metadata.yaml` : Converts example_metadata.yaml into JSON

- yq is a command-line tool to read/manipulate YAML (like jq is for JSON).

- `'.'` means "read the full YAML structure as-is".


`-d @-`

- `-d` sends data in the body of the POST request.

- `@-` means: read the request body from stdin (standard input), i.e., the piped-in JSON from yq.



### File upload 

```bash
curl -X POST "https://next.data.4tu.nl/v3/datasets/dataset-id/upload"   --header "Authorization: token ${API_TOKEN_NEXT}"   --header "Content-Type: multipart/form-data"   -F "file=@absolute-path-to-the-file"
```

- After this command you will realize that need a least a file to  submit for review


-  Now lets take the `uuid` of the draft just created in the previous example and put it in the endpoint

- For tha data , first download the data using curl from github

`curl -O "https://raw.githubusercontent.com/4TUResearchData-Carpentries/WebAPI4RDM/refs/heads/main/Lesson_development/data_files/test_a.csv"  `


```bash
curl -X POST "https://next.data.4tu.nl/v3/datasets/UUID/upload"   --header "Authorization: token ${API_TOKEN_NEXT}"   --header "Content-Type: multipart/form-data"   -F "file=@ABSOULTE_PATH2FILE"
```


### Submit for review 

```bash
yq '.' example_metadata.yaml | curl -X PUT "https://next.data.4tu.nl/v3/datasets/UUID/submit-for-review" --header "Authorization: token ${API_TOKEN_NEXT}" --header "Content-Type: application/json" --data @-
```




:::::::::: keypoints

- APIs operationalize technical interoperability by enabling standardized machine-to-machine interaction.

- REST APIs use HTTP methods, predictable endpoints, JSON representations, stable identifiers, and authentication mechanisms.

- APIs depend on structural interoperability (schemas) and semantic interoperability (controlled vocabularies).

- Command-line tools such as curl provide direct access to API functionality and enable automation.

- The 4TU.ResearchData API supports full dataset lifecycle management: discovery, creation, metadata update, file upload, and submission for review.

::::::::::

