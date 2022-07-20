# Retrieving an openEHR Composition

All data committed to an openEHR CDR is done so via a POST or PUT /composition call - as a JSON or XML 'blob'. 

This section will cover how to retrieve a previously stored Composition by running a `GET / composition` call.

A number of data serialisation options, can be used on retrieval. In this case we will ask for the Better `FLAT JSON` format, as this is what we used when committing the composition, but you can commit and retrieve using different formats if you wish.

#### Better Ehrscape GET /composition example

##### Parameters

`compositionId`: 

This is is the composition UID for the composition you wish to retrieve. Don't worry about how we find that out for now. Just use the UID for the composition you just committed in the last section. 

We will find out how to find compositionIds in a subsequent section.

`format`:

This defines the format of JSON or XML that you are requesting. Use `FLAT` for this example.


## A. Retrieve an openEHR Composition (`FLAT JSON`)

=== "cURL"
  
  ```bash
      curl --location --request GET '{{ehrscapeBaseUrl}}/composition/{{compositionId}}?format=FLAT' \
--header 'Content-Type: application/json' \
--header 'Authorization: {{authToken}}'
  ```


=== "NodeJS + Axios"
  ```js

    var axios = require('axios');

var config = {
  method: 'get',
  url: '{{ehrscapeBaseUrl}}/composition/{{compositionId}}?format=STRUCTURED',
  headers: { 
    'Content-Type': 'application/json', 
    'Authorization': '{{authToken}}'
  }
};

axios(config)
.then(function (response) {
  console.log(JSON.stringify(response.data));
})
.catch(function (error) {
  console.log(error);
});

```


### Response for GET /composition

If the composition is found a `200` code will be returned along with the composition object , which you will note now includes the uid, but should otherwise be identical to that which you previously submitted.

```json
{
    "meta": {
        "href": "https://cdr.code4health.org/rest/v1/composition/7b20dc2b-6494-467d-8986-469367f7c75b::4cce5a07-be4d-4318-a94f-3b8401853a20::1"
    },
    "compositionUid": "7b20dc2b-6494-467d-8986-469367f7c75b::4cce5a07-be4d-4318-a94f-3b8401853a20::1",
    "format": "FLAT",
    "templateId": "JMOHW - Passport observations.v0",
    "composition": {
        "passport_observations/_uid": "7b20dc2b-6494-467d-8986-469367f7c75b::4cce5a07-be4d-4318-a94f-3b8401853a20::1",
        "passport_observations/language|code": "en",
        "passport_observations/language|terminology": "ISO_639-1",
        "passport_observations/territory|code": "JM",
        "passport_observations/territory|terminology": "ISO_3166-1",
        "passport_observations/context/start_time": "2020-10-18T16:16:21.449Z",
        "passport_observations/context/setting|code": "238",
        "passport_observations/context/setting|value": "other care",
        "passport_observations/context/setting|terminology": "openehr",
        "passport_observations/blood_pressure/systolic|magnitude": 169.0,
        "passport_observations/blood_pressure/systolic|unit": "mm[Hg]",
        "passport_observations/blood_pressure/diastolic|magnitude": 692.0,
        "passport_observations/blood_pressure/diastolic|unit": "mm[Hg]",
        "passport_observations/blood_pressure/time": "2020-10-18T16:16:21.452Z",
        "passport_observations/blood_pressure/language|code": "en",
        "passport_observations/blood_pressure/language|terminology": "ISO_639-1",
        "passport_observations/blood_pressure/encoding|code": "UTF-8",
        "passport_observations/blood_pressure/encoding|terminology": "IANA_character-sets",
        "passport_observations/height_length/height_length|magnitude": 609.33,
        "passport_observations/height_length/height_length|unit": "cm",
        "passport_observations/height_length/time": "2020-10-18T16:16:21.453Z",
        "passport_observations/height_length/language|code": "en",
        "passport_observations/height_length/language|terminology": "ISO_639-1",
        "passport_observations/height_length/encoding|code": "UTF-8",
        "passport_observations/height_length/encoding|terminology": "IANA_character-sets",
        "passport_observations/body_weight/weight|magnitude": 380.49,
        "passport_observations/body_weight/weight|unit": "kg",
        "passport_observations/body_weight/time": "2020-10-18T16:16:21.453Z",
        "passport_observations/body_weight/language|code": "en",
        "passport_observations/body_weight/language|terminology": "ISO_639-1",
        "passport_observations/body_weight/encoding|code": "UTF-8",
        "passport_observations/body_weight/encoding|terminology": "IANA_character-sets",
        "passport_observations/blood_glucose/test_name|code": "14743-9",
        "passport_observations/blood_glucose/test_name|value": "Glucose [Moles/volume] in Capillary blood by Glucometer",
        "passport_observations/blood_glucose/test_name|terminology": "LOINC",
        "passport_observations/blood_glucose/laboratory_analyte_result/analyte_name|code": "14743-9",
        "passport_observations/blood_glucose/laboratory_analyte_result/analyte_name|value": "Glucose [Moles/volume] in Capillary blood by Glucometer",
        "passport_observations/blood_glucose/laboratory_analyte_result/analyte_name|terminology": "LOINC",
        "passport_observations/blood_glucose/laboratory_analyte_result/analyte_result|magnitude": 52.61,
        "passport_observations/blood_glucose/laboratory_analyte_result/analyte_result|unit": "mmol/L",
        "passport_observations/blood_glucose/time": "2020-10-18T16:16:21.453Z",
        "passport_observations/blood_glucose/language|code": "en",
        "passport_observations/blood_glucose/language|terminology": "ISO_639-1",
        "passport_observations/blood_glucose/encoding|code": "UTF-8",
        "passport_observations/blood_glucose/encoding|terminology": "IANA_character-sets",
        "passport_observations/pulse_oximetry/spo|numerator": 0.0,
        "passport_observations/pulse_oximetry/spo|denominator": 100.0,
        "passport_observations/pulse_oximetry/spo|type": 2,
        "passport_observations/pulse_oximetry/spo": 0.0,
        "passport_observations/pulse_oximetry/time": "2020-10-18T16:16:21.455Z",
        "passport_observations/pulse_oximetry/language|code": "en",
        "passport_observations/pulse_oximetry/language|terminology": "ISO_639-1",
        "passport_observations/pulse_oximetry/encoding|code": "UTF-8",
        "passport_observations/pulse_oximetry/encoding|terminology": "IANA_character-sets",
        "passport_observations/body_temperature/temperature|magnitude": 72.7,
        "passport_observations/body_temperature/temperature|unit": "[degF]",
        "passport_observations/body_temperature/time": "2020-10-18T16:16:21.455Z",
        "passport_observations/body_temperature/language|code": "en",
        "passport_observations/body_temperature/language|terminology": "ISO_639-1",
        "passport_observations/body_temperature/encoding|code": "UTF-8",
        "passport_observations/body_temperature/encoding|terminology": "IANA_character-sets",
        "passport_observations/clinical_synopsis/notes": "Notes 9",
        "passport_observations/clinical_synopsis/language|code": "en",
        "passport_observations/clinical_synopsis/language|terminology": "ISO_639-1",
        "passport_observations/clinical_synopsis/encoding|code": "UTF-8",
        "passport_observations/clinical_synopsis/encoding|terminology": "IANA_character-sets",
        "passport_observations/category|code": "433",
        "passport_observations/category|value": "event",
        "passport_observations/category|terminology": "openehr",
        "passport_observations/composer|name": "moh-jamaica_4cce5a07-be4d-4318-a94f-3b8401853a20"
    },
    "deleted": false,
    "lastVersion": true,
    "ehrId": "b4a4577f-7496-4053-ae60-45e22cfc9952",
    "lifecycleState": "COMPLETE"
}```

#### Other data formats

The Better Ehrscape API offers several other serialisation formats. You can have a look at these by simply changing the `format` parameter on the `GET / composition` call, and the call Header `Accept` to switch between JSON and XML.


##### 'FLAT JSON'

This uses the same path-shortening mechanism as structured JSON but flattens all of the tree structure to a set of name-value pairs. Some developers prefer this to the STRUCTURED format.

```
format=FLAT
Accept : `application/json'

```
##### 'RAW JSON'

This is very similar to, but not identical to the openEHR Canonical JSON format, which essentially supercedes it. It very closely adheres tothe openEHR Reference model specification but is pretty voluminous

```
format=RAW
Accept : `application/json'
```

##### 'RAW XML'

This is 'canonical' openEHR XML which is also accepted by the openEHR REST  API. It is the lingu-franca for all openEHR CDRs, even thosewhich do not support the REST CDR API, will normally accept and expose data in this XML format. 

```
format=RAW
Accept : `application/xml'
```









