# Querying for recent compositions

This section will take you through how to query for recent Compositions held on the CDR, using AQL (Archetype Query Language)

The initial proposed dataset is
- the name of Composition
- the composition identifier 
- the start date of the Composition
- the name of the clinical author (composer)
- the name of the healthcare facility


### Querying an openEHR CDR - Archetype Query language (AQL)

A CDR can be best thought of in terms of a exposing a complex object tree with the `EHR` object at the top, `Compositions` objects handling commits, but where **all** of the information in the tree can be traversed and accessed. Conceptually this is a logical query/graph language like SPARQL, the key difference being that the exact paths to the objects you might need to access are defined in the archetypes and templates you use to define and validate the data.

AQL can be written by hand but is normally done using a tool like the Better Studio 

Understanding AQL is not generally important for third-party developers. Normally the correct AQL will be supplied by the CDR owner, indeed in production it is much more likely that server-side stored queries would be used.

###  `POST /query` | Run an ad-hoc AQL query

This example will retrieve the data requirement above. To run the query - a `querystring` needs to be carried in the `body` of the POST call as raw data with the Header `Content-Type` set to `application/json'

This is the `queryString:` that we need to send.

```sql
SELECT c/uid/value as compositionId, 
    c/name/value as compositionName,
    c/context/start_time/value as startTime, 
    c/composer/name as authorName, 
    c/context/health_care_facility/name as facilityName
FROM EHR e [ehr_id/value = '{{ehrId}}']
CONTAINS COMPOSITION c

ORDER BY c/context/start_time/value DESC
```

!!! warning "Note the ehrId substitution"

You will need to make sure that the `ehrId` variable in the queryString is substituted with the correct `ehrId` for the patient concerned.

Don't worry about this seemingly odd format -it is essentially a mix of SQL and a path-based syntax, like SPARQL or XQuery.

Running the query is easy- just call the `POST /query` call, sending the AQL string in the body of the request but **note that you should strip the string of any linefeed and carriage returns** etc. 

The Postman 'Ad-hoc query' request has a utility function under 'Pre-req' that sanitises the string and sets it into an environment variable before inserting it into the request body.

=== "Postman"
    ![](../images/cdr-run-query-postman.png)

=== "cURL"
    ```
    curl --location --request POST '{{cdr.ehrscapeBaseUrl}}/query' \
        --header 'Content-Type: application/json' \
        --header 'Authorization: {{cdr.authToken}} \
        --data-raw '{
            "aql": "{{queryString}}"
        }'
    ```
=== "NodeJs - Axios"
    ```js
      var axios = require('axios');
        var data = JSON.stringify({"aql":"{{cdr.queryString}}"});

        var config = {
            method: 'post',
            url: '{{cdr.ehrscapeBaseUrl}}/query',
            headers: { 
                'Content-Type': 'application/json', 
                'Authorization': '{{cdr.authToken}}'
            },
            data : data
         };

        axios(config)
        .then(function (response) {
        console.log(JSON.stringify(response.data));
        })
        .catch(function (error) {
        console.log(error);
        });

    ```
=== "Python/requests"
    ```python
    import requests

    url = "https://{{cdr.ehrscapeBaseUrl}}/query"

    payload = "{\n    \"aql\": \"{{queryString}}\"\n}"
    headers = {
    'Content-Type': 'application/json',
    'Authorization': '{{cdr.authToken}}'
    }

    response = requests.request("POST", url, headers=headers, data = payload)

    print(response.text.encode('utf8'))

    ```

#### Response

The AQL response comes as back as an openEHR `resultSet`, which is a tabular shape, the exact format being determined by the AQL itself.

In this example we have asked for scalar values only, but it is possible for AQL to return objects.

The `columns` object shows the openEHR paths and aliases that are are returned in each row. 

```json
{
    "meta": {
        "_type": "RESULTSET",
        "_created": "2020-10-18T15:46:37.208Z",
        "_executed_aql": "SELECT c/uid/value as compositionId, c/name/value as compositionName,\nc/context/start_time/value as startTime, c/composer/name as authorName, c/context/health_care_facility/name as facilityName\nFROM EHR e \nCONTAINS COMPOSITION c\n \nORDER BY c/context/start_time/value DESC"
    },
    "q": "SELECT c/uid/value as compositionId, c/name/value as compositionName,\nc/context/start_time/value as startTime, c/composer/name as authorName, c/context/health_care_facility/name as facilityName\nFROM EHR e \nCONTAINS COMPOSITION c\n \nORDER BY c/context/start_time/value DESC",
    "columns": [
        {
            "name": "compositionId",
            "path": "/uid/value"
        },
        {
            "name": "compositionName",
            "path": "/name/value"
        },
        {
            "name": "startTime",
            "path": "/context/start_time/value"
        },
        {
            "name": "authorName",
            "path": "/composer/name"
        },
        {
            "name": "facilityName",
            "path": "/context/health_care_facility/name"
        }
    ],
    "rows": [
        [
            "687d40df-57d1-4d29-ab41-88396f810de0::4cce5a07-be4d-4318-a94f-3b8401853a20::1",
            "Passport observations",
            "2020-10-13T14:31:17.878Z",
            "moh-jamaica_4cce5a07-be4d-4318-a94f-3b8401853a20",
            null
        ],
        [
            "845db76d-cf06-4fca-9b62-22a7c231f31b::4cce5a07-be4d-4318-a94f-3b8401853a20::1",
            "NCD - first visit",
            "2020-07-22T15:17:03.696Z",
            "Dr Murphy",
            null
        ]
    ]
}```