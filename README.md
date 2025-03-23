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

# Version v2 and v3
The APIs are hosted at:
- v2: http://35.200.185.69:8000/v2/autocomplete
- v3: http://35.200.185.69:8000/v3/autocomplete
  
The goal is to systematically query the APIs and collect all unique names.  

## v2 API

◆ **Approach**
- Behavior :

  The  API responds with a JSON onject containing:
  - **version:** The API version (e.g., v2).
  - **count:** The number of results returned.
  - **results:** A list of strings (e.g., ["40", "400", "40n351v", ...]).

   Results can include numbers and alphanumeric strings.

- Query Strategy:

   1. Single-Digit Numbers:
      - Query all single-digit numbers (0–9), totaling 10 queries.
     
   2. Two-Digit Numbers:
      - Query all two-digit numbers (00–99), totaling 100 queries.
     
   3. Two-Character Alphanumeric Strings:
      - Query all two-character combinations of letters (a-z) and digits (0-9), totaling 1,296 queries.
     
- Progress Tracking:
  - The script tracks the number of completed queries and prints progress (e.g., Progress: 10/1406 queries completed).
 
- Rate Limiting Handling:
  - A delay of 1 second is added between requests to avoid hitting the API's rate limit.
  - If the API returns a 429 Too Many Requests error, the script waits for 10 seconds and retries the query.
 
- Efficient Storage:
  - Unique names are stored in a set to automatically handle duplicates.

◆ **Results**
- **Total Queries**
  - Single-Digit Numbers: 10 queries.
  - Two-Digit Numbers: 100 queries.
  - Two-Character Alphanumeric Strings: 1,296 queries.
  - **Total Queries: 1,406 queries.**
 
- **Total Unique Names Extracted**
  - 7873 unique names were extracted from the API.
 
- **Output File**
  - The extracted names are saved in extracted_names_v2.txt.


## v3 API

◆ **Approach**
- Behavior :

  The  API responds with a JSON onject containing:
  - **version:** The API version (e.g., v3).
  - **count:** The number of results returned.
  - **results:** A list of strings (e.g.,["a", "a e+skbrns", "a ifs1.-", ...] ).

   Results can include letters, numbers, special characters (+, -, .), and spaces.

- Query Strategy:

   1. Single-Character Queries:
      - Query all single-character combinations (a-z, 0-9, +, -, ., ), totaling 40 queries.
     
   2. Recursive Querying
      - Use the results from single-character queries to build longer queries recursively.
      - For example, if a returns a+, query a+ to get a+b, a+c, etc.
      - This avoids querying all possible combinations and focuses on valid prefixes.

- Progress Tracking:
  - The script tracks the number of completed queries and prints progress (e.g., Progress: 10 queries completed).
 
- Rate Limiting Handling:
  - A delay of 1 second is added between requests to avoid hitting the API's rate limit.
  - If the API returns a 429 Too Many Requests error, the script waits for 10 seconds and retries the query.
 
- Efficient Storage:
  - Unique names are stored in a set to automatically handle duplicates.

◆ **Results**
- **Total Queries**
  - Single-Character Queries: 40 queries.
  - Recursive Queries: ~570 queries.
  - **Total Queries: 610 queries.**
 
- **Total Unique Names Extracted**
  - 570 unique names were extracted from the API.
 
- **Output File**
  - The extracted names are saved in extracted_names_v3.txt.


# **Conclusion**
    
This project successfully demonstrates the extraction of unique names from the v1, v2, and v3 versions of an undocumented autocomplete API. By systematically querying the APIs and handling rate limiting, the scripts efficiently collected:

- v1: Extracted 6,720 unique names using a brute-force approach with two-letter combinations.

- v2: Extracted 7,873 unique names by querying single-digit numbers, two-digit numbers, and two-character alphanumeric combinations.

- v3: Extracted 570 unique names using recursive querying based on valid prefixes, including letters, numbers, special characters, and spaces.

The use of progress tracking and rate limiting handling ensured an optimized and transparent extraction process. The results were saved to files (extracted_names_v1.txt, extracted_names_v2.txt, and extracted_names_v3.txt) for further analysis. This project highlights the importance of systematic exploration, adaptability to API behavior, and efficient handling of constraints like rate limiting. The approach can be adapted for similar APIs, making it a valuable tool for data extraction tasks.
