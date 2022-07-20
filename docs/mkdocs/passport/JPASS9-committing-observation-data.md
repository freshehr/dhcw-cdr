# Committing Observation data

All data committed to an openEHR CDR is done so via a POST /composition call - as a JSON or XML 'blob'. 

As it is committed, the data will be validated against both the appropriate openEHR template and the underlying Reference model schema.

If the data is valid, it will be stored in the CDR and is allocated a unique ID, which is returned by the POST /composition call

This section will submit an example Composition to the CDR by running a `POST / composition` call.

A number of data serialisation options, can be used. In this case we will use the Better `STRUCTURED JSON` format, as it is somewhat easier to use than the current openEHR `CANONICAL JSON or XML` formats.

!!! note
Note that this example uses the Better `Better Ehrscape API` which has a slightly different base URL and parameters than the `openEHR REST API`,though the data is stored identically and can be accessed from both end points.

#### Ehrscape POST /composition example

##### Parameters

`ehrId`: 

This is is the internal CDR identifier for a specific patient. When a patient is registered with the CDR,an EHR object is created with a unique `ehr_id` identifier, and is associated with an external `subjectId` and subjectNamespace e.g an NHS Number, CHI number, or a local hospital number.

We will find out how to work out the correct ehrId for a patient in the next section.

Generally when you first open a patient record session, you will retrieve their `ehrId` via their `subjectID` and `subjectNamespace`. We will explain how to do that in the next section.

For testing purposes, you should use a known `ehrId`. If you have a Postman environment file, an example will be in there, otherwise you can find out how to identify valid ehrIds [here]()


`templateId`: 

This is the identifier of the openEHR template, against which you need to validate the composition. Use `DHI - Urology_PROMs-v0` for this example.  


`format`: 

This defines the format of JSON or XML that you are sending. Use `FLAT` for this example.


## A. Commit an openEHR Composition (`FLAT JSON`)

=== "cURL"
  
  ```bash
      curl 
      --location 
      --request POST '{{ehrscapeBaseUrl}}/composition?ehrId={{ehrId}}&templateId={{templateId}}&format=STRUCTURED' \
      --header 'Content-Type: application/json' \
      --header 'Authorization: {{auth_token}}' \
      --data-raw '{
      "{
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
        "passport_observations/language|code": "en",
        "passport_observations/language|terminology": "ISO_639-1",
        "passport_observations/territory|code": "JM",
        "passport_observations/territory|terminology": "ISO_3166-1"
    }'
  ```


=== "NodeJS + Axios"
  ```js

    var axios = require('axios');

    var data = JSON.stringify({"prostate_cancer_proms_report": // trimmed for brevity}");

    var config = {
      method: 'post',
      url: '{{ehrscapeBaseUrl}}/composition?ehrId={{ehrId}}&templateId={{templateId}}&format=STRUCTURED',
      headers: { 
        'Content-Type': 'application/json', 
        'Authorization': '{{authToken}}'
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


### Response for POST /composition

```json
{
    "meta": {
        "href": "https://cdr.code4health.org/rest/v1/composition/59e8f5a2-6fb6-47d5-9acd-ae6309d0f1dd::a81f47c6-a757-4e34-b644-3ccc62b4a01c::1"
    },
    "action": "CREATE",
    "compositionUid": "59e8f5a2-6fb6-47d5-9acd-ae6309d0f1dd::a81f47c6-a757-4e34-b644-3ccc62b4a01c::1"
}
```

#### The `Composition Id`

The compositionUid is the unique identifier allocated to (and held within) every composition by the CDR.

You will see that it ends in `...::1`. The `1` is the version of this composition instance. If you need to update the instance (perhaps because of an error), you need to do so via a PUT / composition call and if successful the composition version number will clock up to `::2`.

In essence every commit is versioned and retained for medico-legal reasons. 

Similarly when a composition is deleted, this is a logical delete and the composition can always be retrieved, though is not normally accessible via querying.

We will go through the process of updating a composition later.

For now let's just [retrieve the composition](JPASS10-retrieving-observation-data.md) we just committed, via the GET /composition call.








