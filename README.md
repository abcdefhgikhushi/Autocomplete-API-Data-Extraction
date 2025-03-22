# Autocomplete API Data Extraction

This project involves extracting all possible names from an undocumented autocomplete API. The API is hosted at http://35.200.185.69:8000/v1/autocomplete, 
and the goal is to systematically query the API and collect all unique names.

## Approach

### 1. Exploration of the API
◆ **Initial Testing :**

- The API endpoint /v1/autocomplete?query=<string> was tested with various inputs (e.g., single letters, numbers, special characters) to understand its behavior.

- The API responds with a JSON object containing the following fields:

  - **version** : Indicates the API version (e.g., v1,v2).
  - **count** : The number of results returned in the results array.
  - **results** : A list of strings (e.g., ["ng", "ngbayf", "ngfdropx", ...]).
    
◆ **Query Behavior :**

- The API uses **prefix matching**. For example:
  - Querying with n returns results starting with n (e.g., ng, ngbayf).
  - Querying with ng returns results starting with ng (e.g., ngbayf, ngfdropx).

◆ **Rate Limiting :**

- The API enforces rate limiting, returning a 429 Too Many Requests error if too many requests are made in a short period.
- To handle this, a delay of 1 second was added between requests, and retries were implemented for rate-limited queries.

### 2. Data Extraction

◆ **Brute-Force Approach :**

- The script queries the API with all possible two-letter combinations (aa, ab, ..., zz), totaling 676 requests.
- For each query, the script processes the response and extracts unique names.

◆ **Handling Rate Limiting :**

- If the API returns a 429 error, the script waits for 10 seconds and retries the query.
- A delay of 1 second is added between requests to avoid hitting the rate limit.

◆ **Storing Results :**

- Unique names are stored in a Python set to automatically handle duplicates.
- The final list of names is saved to a file (extracted_names.txt).

## Tools and Scripts

### 1. Python Script
- The main script (extract_names.py) is written in Python and uses the requests library to interact with the API.
- The script systematically queries the API with all two-letter combinations and extracts unique names.

### 2. Testing Tools
- **Command Prompt:** Used curl to manually test the API and observe the response format.
Example: curl "http://35.200.185.69:8000/v1/autocomplete?query=n"

## Results

### Total Requests Made
- The script made 676 requests (26 letters × 26 letters) to the API.

### Total Names Extracted
- A total of 6720 unique names were extracted from the API.

### Output File
- The extracted names are saved in extracted_names.txt.
