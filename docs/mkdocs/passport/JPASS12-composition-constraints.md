# Understanding composition Constraints and validation


As it is committed, the data will be validated against both the appropriate openEHR template and the underlying Reference Model schema.

If the data is valid, it will be stored in the CDR and is allocated a unique `compositionID`, which is returned by the POST /composition call on a successful call.

One of the key challenges in working with openEHR, is in understanding the complexities of the various datatypes and the other validation rules that apply for any particular template.

The constraint and validation rules are applied in by

1. The Reference model

2. Archetype-level constraints

3. Template-level constraints


### How to figure out which constraints apply?

As an example, I know that a particular coded entry list (perhaps drop-down) allows only a fixed selection of possible answers, which are probably coded ( a good example being a PROMS score). Where can I find how these are defined?

The ultimate source of truth is the `.opt` 'Operational template' which is uploaded to the CDR. It is essentially an aggregation of all the archetype constraints, and any local template constraints, which are than applied ot the underlying RM to give the full validation target.

The .opt is a faithful representation of the underlying in-memory objects but is a pretty difficult beast to parse and understand.

Fortunately Better provide a 'web template' utility which generates a JSON version of the validation statement but in a much more understandable format -both for human consumption and parsing. The web template facility is available both from the Ehrscape API, and as an export option from the openEHR Archetype Designer. We understand that ehrBase are developing a similar export facility and we expect this to become part of the openEHR standard in due course.

#### Better Ehrscape: `GET /template - Retrieve a web template'

##### Parameters

**`templateId`**:  
This is the identifier of the openEHR template, against which you need to validate the composition, in this case `DHI - Urology_PROMs-v0`  

**`format`**:  
This defines the format of JSON or XML that you are sending. Use `STRUCTURED` for this example.

    
##### Request

=== "Postman"
    ![](../images/dhi-web-template-postman.png)

=== "cURL" 
    ```bash

        curl --location \
        --request GET '{{cdr.ehrscapeBaseUrl}}/template/JMOHW - Passport observations.v0' \
        --header 'Authorization: {{cdr.authToken}}' \
    ```
=== "NodeJS/Axios"
    ```js
        var axios = require('axios');

        var config = {
         method: 'get',
         url: '{{cdr.ehrscapeBaseUrl}}/template/JMOHW - Passport observations.v0',
        headers: { 
            'Content-Type': 'application/json', 
            'Authorization': '{{cdr.authToken}}', 
    
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

        url = "{{cdr.ehrscapeBaseUrl}}/template/JMOHW - Passport observations.v0"
   
        headers = {
        'Content-Type': 'application/json',
        'Authorization': '{{cdr.authToken}}',
        }

        response = requests.request("GET", url, headers=headers)

        print(response.text.encode('utf8'))
    ```

##### Response
```json
{
    "meta": {
        "href": "https://cdr.code4health.org/rest/v1/template/JMOHW%20-%20Passport%20observations.v0"
    },
    "webTemplate": {
        "templateId": "JMOHW - Passport observations.v0",
        "version": "2.3",
        "defaultLanguage": "en",
        "languages": [
            "en"
        ],
        "tree": {
            "id": "passport_observations",
            "name": "Passport observations",
            "localizedName": "Passport observations",
            "rmType": "COMPOSITION",
            "nodeId": "openEHR-EHR-COMPOSITION.encounter.v1",
            "min": 1,
            "max": 1,
            "localizedNames": {
                "en": "Passport observations"
            },
            "localizedDescriptions": {
                "en": "Interaction, contact or care event between a subject of care and healthcare provider(s)."
            },
            "aqlPath": "",
            "children": [
                {
                    "id": "context",
                    "rmType": "EVENT_CONTEXT",
                    "nodeId": "",
                    "min": 1,
                    "max": 1,
                    "aqlPath": "/context",
                    "children": [
                        {
                            "id": "start_time",
                            "name": "Start_time",
                            "rmType": "DV_DATE_TIME",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/context/start_time",
                            "inputs": [
                                {
                                    "type": "DATETIME"
                                }
                            ],
                            "inContext": true
                        },
                        {
                            "id": "setting",
                            "name": "Setting",
                            "rmType": "DV_CODED_TEXT",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/context/setting",
                            "inputs": [
                                {
                                    "suffix": "code",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "value",
                                    "type": "TEXT"
                                }
                            ],
                            "inContext": true
                        }
                    ]
                },
                {
                    "id": "blood_pressure",
                    "name": "Blood pressure",
                    "localizedName": "Blood pressure",
                    "rmType": "OBSERVATION",
                    "nodeId": "openEHR-EHR-OBSERVATION.blood_pressure.v2",
                    "min": 0,
                    "max": 1,
                    "localizedNames": {
                        "en": "Blood pressure"
                    },
                    "localizedDescriptions": {
                        "en": "The local measurement of arterial blood pressure which is a surrogate for arterial pressure in the systemic circulation."
                    },
                    "annotations": {
                        "comment": "Most commonly, use of the term 'blood pressure' refers to measurement of brachial artery pressure in the upper arm."
                    },
                    "aqlPath": "/content[openEHR-EHR-OBSERVATION.blood_pressure.v2]",
                    "children": [
                        {
                            "id": "systolic",
                            "name": "Systolic",
                            "localizedName": "Systolic",
                            "rmType": "DV_QUANTITY",
                            "nodeId": "at0004",
                            "min": 0,
                            "max": 1,
                            "localizedNames": {
                                "en": "Systolic"
                            },
                            "localizedDescriptions": {
                                "en": "Peak systemic arterial blood pressure  - measured in systolic or contraction phase of the heart cycle."
                            },
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.blood_pressure.v2]/data[at0001]/events[at0006]/data[at0003]/items[at0004]/value",
                            "inputs": [
                                {
                                    "suffix": "magnitude",
                                    "type": "DECIMAL",
                                    "validation": {
                                        "range": {
                                            "minOp": ">=",
                                            "min": 0.0,
                                            "maxOp": "<",
                                            "max": 1000.0
                                        },
                                        "precision": {
                                            "minOp": ">=",
                                            "min": 0,
                                            "maxOp": "<=",
                                            "max": 0
                                        }
                                    }
                                },
                                {
                                    "suffix": "unit",
                                    "type": "CODED_TEXT",
                                    "list": [
                                        {
                                            "value": "mm[Hg]",
                                            "label": "mm[Hg]",
                                            "localizedLabels": {
                                                "en": "mmHg"
                                            },
                                            "validation": {
                                                "range": {
                                                    "minOp": ">=",
                                                    "min": 0.0,
                                                    "maxOp": "<",
                                                    "max": 1000.0
                                                },
                                                "precision": {
                                                    "minOp": ">=",
                                                    "min": 0,
                                                    "maxOp": "<=",
                                                    "max": 0
                                                }
                                            }
                                        }
                                    ]
                                }
                            ],
                            "termBindings": {
                                "SNOMED-CT": {
                                    "value": "[SNOMED-CT(2003)::271649006]",
                                    "terminologyId": "SNOMED-CT"
                                }
                            }
                        },
                        {
                            "id": "diastolic",
                            "name": "Diastolic",
                            "localizedName": "Diastolic",
                            "rmType": "DV_QUANTITY",
                            "nodeId": "at0005",
                            "min": 0,
                            "max": 1,
                            "localizedNames": {
                                "en": "Diastolic"
                            },
                            "localizedDescriptions": {
                                "en": "Minimum systemic arterial blood pressure - measured in the diastolic or relaxation phase of the heart cycle."
                            },
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.blood_pressure.v2]/data[at0001]/events[at0006]/data[at0003]/items[at0005]/value",
                            "inputs": [
                                {
                                    "suffix": "magnitude",
                                    "type": "DECIMAL",
                                    "validation": {
                                        "range": {
                                            "minOp": ">=",
                                            "min": 0.0,
                                            "maxOp": "<",
                                            "max": 1000.0
                                        },
                                        "precision": {
                                            "minOp": ">=",
                                            "min": 0,
                                            "maxOp": "<=",
                                            "max": 0
                                        }
                                    }
                                },
                                {
                                    "suffix": "unit",
                                    "type": "CODED_TEXT",
                                    "list": [
                                        {
                                            "value": "mm[Hg]",
                                            "label": "mm[Hg]",
                                            "localizedLabels": {
                                                "en": "mmHg"
                                            },
                                            "validation": {
                                                "range": {
                                                    "minOp": ">=",
                                                    "min": 0.0,
                                                    "maxOp": "<",
                                                    "max": 1000.0
                                                },
                                                "precision": {
                                                    "minOp": ">=",
                                                    "min": 0,
                                                    "maxOp": "<=",
                                                    "max": 0
                                                }
                                            }
                                        }
                                    ]
                                }
                            ],
                            "termBindings": {
                                "SNOMED-CT": {
                                    "value": "[SNOMED-CT(2003)::271650006]",
                                    "terminologyId": "SNOMED-CT"
                                }
                            }
                        },
                        {
                            "id": "time",
                            "name": "Time",
                            "rmType": "DV_DATE_TIME",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.blood_pressure.v2]/data[at0001]/events[at0006]/time",
                            "inputs": [
                                {
                                    "type": "DATETIME"
                                }
                            ],
                            "inContext": true
                        },
                        {
                            "id": "language",
                            "name": "Language",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.blood_pressure.v2]/language",
                            "inContext": true
                        },
                        {
                            "id": "encoding",
                            "name": "Encoding",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.blood_pressure.v2]/encoding",
                            "inContext": true
                        },
                        {
                            "id": "subject",
                            "name": "Subject",
                            "rmType": "PARTY_PROXY",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.blood_pressure.v2]/subject",
                            "inputs": [
                                {
                                    "suffix": "id",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_scheme",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_namespace",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "name",
                                    "type": "TEXT"
                                }
                            ],
                            "inContext": true
                        }
                    ],
                    "termBindings": {
                        "SNOMED-CT": {
                            "value": "[SNOMED-CT(2003)::364090009]",
                            "terminologyId": "SNOMED-CT"
                        }
                    }
                },
                {
                    "id": "height_length",
                    "name": "Height/Length",
                    "localizedName": "Height/Length",
                    "rmType": "OBSERVATION",
                    "nodeId": "openEHR-EHR-OBSERVATION.height.v2",
                    "min": 0,
                    "max": 1,
                    "localizedNames": {
                        "en": "Height/Length"
                    },
                    "localizedDescriptions": {
                        "en": "Height, or body length, is measured from crown of head to sole of foot."
                    },
                    "annotations": {
                        "comment": "Height is measured with the individual in a standing position and body length in a recumbent position."
                    },
                    "aqlPath": "/content[openEHR-EHR-OBSERVATION.height.v2]",
                    "children": [
                        {
                            "id": "height_length",
                            "name": "Height/Length",
                            "localizedName": "Height/Length",
                            "rmType": "DV_QUANTITY",
                            "nodeId": "at0004",
                            "min": 1,
                            "max": 1,
                            "localizedNames": {
                                "en": "Height/Length"
                            },
                            "localizedDescriptions": {
                                "en": "The length of the body from crown of head to sole of foot."
                            },
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.height.v2]/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value",
                            "inputs": [
                                {
                                    "suffix": "magnitude",
                                    "type": "DECIMAL",
                                    "validation": {
                                        "range": {
                                            "minOp": ">=",
                                            "min": 0.0,
                                            "maxOp": "<=",
                                            "max": 1000.0
                                        }
                                    }
                                },
                                {
                                    "suffix": "unit",
                                    "type": "CODED_TEXT",
                                    "list": [
                                        {
                                            "value": "cm",
                                            "label": "cm",
                                            "validation": {
                                                "range": {
                                                    "minOp": ">=",
                                                    "min": 0.0,
                                                    "maxOp": "<=",
                                                    "max": 1000.0
                                                }
                                            }
                                        }
                                    ]
                                }
                            ]
                        },
                        {
                            "id": "time",
                            "name": "Time",
                            "rmType": "DV_DATE_TIME",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.height.v2]/data[at0001]/events[at0002]/time",
                            "inputs": [
                                {
                                    "type": "DATETIME"
                                }
                            ],
                            "inContext": true
                        },
                        {
                            "id": "language",
                            "name": "Language",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.height.v2]/language",
                            "inContext": true
                        },
                        {
                            "id": "encoding",
                            "name": "Encoding",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.height.v2]/encoding",
                            "inContext": true
                        },
                        {
                            "id": "subject",
                            "name": "Subject",
                            "rmType": "PARTY_PROXY",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.height.v2]/subject",
                            "inputs": [
                                {
                                    "suffix": "id",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_scheme",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_namespace",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "name",
                                    "type": "TEXT"
                                }
                            ],
                            "inContext": true
                        }
                    ]
                },
                {
                    "id": "body_weight",
                    "name": "Body weight",
                    "localizedName": "Body weight",
                    "rmType": "OBSERVATION",
                    "nodeId": "openEHR-EHR-OBSERVATION.body_weight.v2",
                    "min": 0,
                    "max": 1,
                    "localizedNames": {
                        "en": "Body weight"
                    },
                    "localizedDescriptions": {
                        "en": "Measurement of the body weight of an individual."
                    },
                    "aqlPath": "/content[openEHR-EHR-OBSERVATION.body_weight.v2]",
                    "children": [
                        {
                            "id": "weight",
                            "name": "Weight",
                            "localizedName": "Weight",
                            "rmType": "DV_QUANTITY",
                            "nodeId": "at0004",
                            "min": 1,
                            "max": 1,
                            "localizedNames": {
                                "en": "Weight"
                            },
                            "localizedDescriptions": {
                                "en": "The weight of the individual."
                            },
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.body_weight.v2]/data[at0002]/events[at0003]/data[at0001]/items[at0004]/value",
                            "inputs": [
                                {
                                    "suffix": "magnitude",
                                    "type": "DECIMAL",
                                    "validation": {
                                        "range": {
                                            "minOp": ">=",
                                            "min": 0.0,
                                            "maxOp": "<=",
                                            "max": 1000.0
                                        }
                                    }
                                },
                                {
                                    "suffix": "unit",
                                    "type": "CODED_TEXT",
                                    "list": [
                                        {
                                            "value": "kg",
                                            "label": "kg",
                                            "validation": {
                                                "range": {
                                                    "minOp": ">=",
                                                    "min": 0.0,
                                                    "maxOp": "<=",
                                                    "max": 1000.0
                                                }
                                            }
                                        }
                                    ]
                                }
                            ]
                        },
                        {
                            "id": "time",
                            "name": "Time",
                            "rmType": "DV_DATE_TIME",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.body_weight.v2]/data[at0002]/events[at0003]/time",
                            "inputs": [
                                {
                                    "type": "DATETIME"
                                }
                            ],
                            "inContext": true
                        },
                        {
                            "id": "language",
                            "name": "Language",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.body_weight.v2]/language",
                            "inContext": true
                        },
                        {
                            "id": "encoding",
                            "name": "Encoding",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.body_weight.v2]/encoding",
                            "inContext": true
                        },
                        {
                            "id": "subject",
                            "name": "Subject",
                            "rmType": "PARTY_PROXY",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.body_weight.v2]/subject",
                            "inputs": [
                                {
                                    "suffix": "id",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_scheme",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_namespace",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "name",
                                    "type": "TEXT"
                                }
                            ],
                            "inContext": true
                        }
                    ]
                },
                {
                    "id": "blood_glucose",
                    "name": "Blood glucose",
                    "localizedName": "Blood glucose",
                    "rmType": "OBSERVATION",
                    "nodeId": "openEHR-EHR-OBSERVATION.laboratory_test_result.v1",
                    "min": 0,
                    "max": 1,
                    "localizedNames": {
                        "en": "Blood glucose"
                    },
                    "localizedDescriptions": {
                        "en": "The result, including findings and the laboratory's interpretation, of an investigation performed on specimens collected from an individual or related to that individual."
                    },
                    "aqlPath": "/content[openEHR-EHR-OBSERVATION.laboratory_test_result.v1,'Blood glucose']",
                    "children": [
                        {
                            "id": "test_name",
                            "name": "Test name",
                            "localizedName": "Test name",
                            "rmType": "DV_CODED_TEXT",
                            "nodeId": "at0005",
                            "min": 1,
                            "max": 1,
                            "localizedNames": {
                                "en": "Test name"
                            },
                            "localizedDescriptions": {
                                "en": "Name of the laboratory investigation performed on the specimen(s)."
                            },
                            "annotations": {
                                "comment": "A test result may be for a single analyte, or a group of items, including panel tests. It is strongly recommended that 'Test name' be coded with a terminology, for example LOINC or SNOMED CT. For example: 'Glucose', 'Urea and Electrolytes', 'Swab', 'Cortisol (am)', 'Potassium in perspiration' or 'Melanoma histopathology'. The name may sometimes include specimen type and patient state, for example 'Fasting blood glucose' or include other information, as 'Potassium (PNA blood gas)'."
                            },
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.laboratory_test_result.v1,'Blood glucose']/data[at0001]/events[at0002]/data[at0003]/items[at0005]/value",
                            "inputs": [
                                {
                                    "suffix": "code",
                                    "type": "CODED_TEXT",
                                    "list": [
                                        {
                                            "value": "14743-9",
                                            "label": "Glucose [Moles/volume] in Capillary blood by Glucometer",
                                            "localizedLabels": {
                                                "en": ""
                                            }
                                        }
                                    ],
                                    "defaultValue": "14743-9",
                                    "terminology": "LOINC"
                                }
                            ]
                        },
                        {
                            "id": "laboratory_analyte_result",
                            "name": "Laboratory analyte result",
                            "localizedName": "Laboratory analyte result",
                            "rmType": "CLUSTER",
                            "nodeId": "openEHR-EHR-CLUSTER.laboratory_test_analyte.v1",
                            "min": 0,
                            "max": 1,
                            "localizedNames": {
                                "en": "Laboratory analyte result"
                            },
                            "localizedDescriptions": {
                                "en": "The result of a laboratory test for a single analyte value."
                            },
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.laboratory_test_result.v1,'Blood glucose']/data[at0001]/events[at0002]/data[at0003]/items[openEHR-EHR-CLUSTER.laboratory_test_analyte.v1]",
                            "children": [
                                {
                                    "id": "analyte_name",
                                    "name": "Analyte name",
                                    "localizedName": "Analyte name",
                                    "rmType": "DV_CODED_TEXT",
                                    "nodeId": "at0024",
                                    "min": 0,
                                    "max": 1,
                                    "localizedNames": {
                                        "en": "Analyte name"
                                    },
                                    "localizedDescriptions": {
                                        "en": "The name of the analyte result."
                                    },
                                    "annotations": {
                                        "comment": "The value for this element is normally supplied in a specialisation, in a template or at run-time to reflect the actual analyte. For example: 'Serum sodium', 'Haemoglobin'. Coding with an external terminology is strongly recommended, such as LOINC, NPU, SNOMED CT, or local lab terminologies.",
                                        "hl7v2_mapping": "OBX.3",
                                        "fhir_mapping": "Observation.code"
                                    },
                                    "aqlPath": "/content[openEHR-EHR-OBSERVATION.laboratory_test_result.v1,'Blood glucose']/data[at0001]/events[at0002]/data[at0003]/items[openEHR-EHR-CLUSTER.laboratory_test_analyte.v1]/items[at0024]/value",
                                    "inputs": [
                                        {
                                            "suffix": "code",
                                            "type": "CODED_TEXT",
                                            "list": [
                                                {
                                                    "value": "14743-9",
                                                    "label": "Glucose [Moles/volume] in Capillary blood by Glucometer",
                                                    "localizedLabels": {
                                                        "en": ""
                                                    }
                                                }
                                            ],
                                            "defaultValue": "14743-9",
                                            "terminology": "LOINC"
                                        }
                                    ]
                                },
                                {
                                    "id": "analyte_result",
                                    "name": "Analyte result",
                                    "localizedName": "Analyte result",
                                    "rmType": "DV_QUANTITY",
                                    "nodeId": "at0001",
                                    "min": 0,
                                    "max": 1,
                                    "localizedNames": {
                                        "en": "Analyte result"
                                    },
                                    "localizedDescriptions": {
                                        "en": "The value of the analyte result."
                                    },
                                    "annotations": {
                                        "comment": "For example '7.3 mmol/l', 'Raised'. The 'Any' data type will need to be constrained to an appropriate data type in a specialisation, a template or at run-time to reflect the actual analyte result. The Quantity data type has reference model attributes that include flags for normal/abnormal, reference ranges and approximations - see https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_quantity_class for more details.",
                                        "hl7v2_mapping": "OBX.2, OBX.5, OBX.6, OBX.7, OBX.8",
                                        "fhir_mapping": "Observation.value[x]"
                                    },
                                    "aqlPath": "/content[openEHR-EHR-OBSERVATION.laboratory_test_result.v1,'Blood glucose']/data[at0001]/events[at0002]/data[at0003]/items[openEHR-EHR-CLUSTER.laboratory_test_analyte.v1]/items[at0001]/value",
                                    "inputs": [
                                        {
                                            "suffix": "magnitude",
                                            "type": "DECIMAL"
                                        },
                                        {
                                            "suffix": "unit",
                                            "type": "CODED_TEXT",
                                            "list": [
                                                {
                                                    "value": "mmol/L",
                                                    "label": "mmol/L"
                                                }
                                            ]
                                        }
                                    ]
                                }
                            ]
                        },
                        {
                            "id": "time",
                            "name": "Time",
                            "rmType": "DV_DATE_TIME",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.laboratory_test_result.v1,'Blood glucose']/data[at0001]/events[at0002]/time",
                            "inputs": [
                                {
                                    "type": "DATETIME"
                                }
                            ],
                            "inContext": true
                        },
                        {
                            "id": "language",
                            "name": "Language",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.laboratory_test_result.v1,'Blood glucose']/language",
                            "inContext": true
                        },
                        {
                            "id": "encoding",
                            "name": "Encoding",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.laboratory_test_result.v1,'Blood glucose']/encoding",
                            "inContext": true
                        },
                        {
                            "id": "subject",
                            "name": "Subject",
                            "rmType": "PARTY_PROXY",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.laboratory_test_result.v1,'Blood glucose']/subject",
                            "inputs": [
                                {
                                    "suffix": "id",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_scheme",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_namespace",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "name",
                                    "type": "TEXT"
                                }
                            ],
                            "inContext": true
                        }
                    ]
                },
                {
                    "id": "pulse_oximetry",
                    "name": "Pulse oximetry",
                    "localizedName": "Pulse oximetry",
                    "rmType": "OBSERVATION",
                    "nodeId": "openEHR-EHR-OBSERVATION.pulse_oximetry.v1",
                    "min": 0,
                    "max": 1,
                    "localizedNames": {
                        "en": "Pulse oximetry"
                    },
                    "localizedDescriptions": {
                        "en": "Blood oxygen and related measurements, measured by pulse oximetry or pulse CO-oximetry."
                    },
                    "aqlPath": "/content[openEHR-EHR-OBSERVATION.pulse_oximetry.v1]",
                    "children": [
                        {
                            "id": "spo",
                            "name": "SpO₂",
                            "localizedName": "SpO₂",
                            "rmType": "DV_PROPORTION",
                            "nodeId": "at0006",
                            "min": 0,
                            "max": 1,
                            "localizedNames": {
                                "en": "SpO₂"
                            },
                            "localizedDescriptions": {
                                "en": "The saturation of oxygen in the peripheral blood, measured via pulse oximetry."
                            },
                            "annotations": {
                                "comment": "SpO₂ is defined as the percentage of oxyhaemoglobin (HbO₂) to the total concentration of haemoglobin (HbO₂ + deoxyhaemoglobin) in peripheral blood."
                            },
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.pulse_oximetry.v1]/data[at0001]/events[at0002]/data[at0003]/items[at0006]/value",
                            "proportionTypes": [
                                "percent"
                            ],
                            "inputs": [
                                {
                                    "suffix": "numerator",
                                    "type": "DECIMAL",
                                    "validation": {
                                        "range": {
                                            "minOp": ">=",
                                            "min": 0.0,
                                            "maxOp": "<=",
                                            "max": 100.0
                                        }
                                    },
                                    "defaultValue": 0.0
                                },
                                {
                                    "suffix": "denominator",
                                    "type": "DECIMAL",
                                    "validation": {
                                        "range": {
                                            "minOp": ">=",
                                            "min": 100.0,
                                            "maxOp": "<=",
                                            "max": 100.0
                                        }
                                    }
                                }
                            ],
                            "termBindings": {
                                "SNOMED-CT": {
                                    "value": "[SNOMED-CT::431314004]",
                                    "terminologyId": "SNOMED-CT"
                                },
                                "LOINC": {
                                    "value": "[LOINC::59408-5]",
                                    "terminologyId": "LOINC"
                                }
                            }
                        },
                        {
                            "id": "time",
                            "name": "Time",
                            "rmType": "DV_DATE_TIME",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.pulse_oximetry.v1]/data[at0001]/events[at0002]/time",
                            "inputs": [
                                {
                                    "type": "DATETIME"
                                }
                            ],
                            "inContext": true
                        },
                        {
                            "id": "language",
                            "name": "Language",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.pulse_oximetry.v1]/language",
                            "inContext": true
                        },
                        {
                            "id": "encoding",
                            "name": "Encoding",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.pulse_oximetry.v1]/encoding",
                            "inContext": true
                        },
                        {
                            "id": "subject",
                            "name": "Subject",
                            "rmType": "PARTY_PROXY",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.pulse_oximetry.v1]/subject",
                            "inputs": [
                                {
                                    "suffix": "id",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_scheme",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_namespace",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "name",
                                    "type": "TEXT"
                                }
                            ],
                            "inContext": true
                        }
                    ]
                },
                {
                    "id": "body_temperature",
                    "name": "Body temperature",
                    "localizedName": "Body temperature",
                    "rmType": "OBSERVATION",
                    "nodeId": "openEHR-EHR-OBSERVATION.body_temperature.v2",
                    "min": 0,
                    "max": 1,
                    "localizedNames": {
                        "en": "Body temperature"
                    },
                    "localizedDescriptions": {
                        "en": "A measurement of the body temperature, which is a surrogate for the core body temperature of the individual."
                    },
                    "aqlPath": "/content[openEHR-EHR-OBSERVATION.body_temperature.v2]",
                    "children": [
                        {
                            "id": "temperature",
                            "name": "Temperature",
                            "localizedName": "Temperature",
                            "rmType": "DV_QUANTITY",
                            "nodeId": "at0004",
                            "min": 1,
                            "max": 1,
                            "localizedNames": {
                                "en": "Temperature"
                            },
                            "localizedDescriptions": {
                                "en": "The measured body temperature (as a surrogate for the core of the body)."
                            },
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.body_temperature.v2]/data[at0002]/events[at0003]/data[at0001]/items[at0004]/value",
                            "inputs": [
                                {
                                    "suffix": "magnitude",
                                    "type": "DECIMAL",
                                    "validation": {
                                        "range": {
                                            "minOp": ">=",
                                            "min": 30.0,
                                            "maxOp": "<",
                                            "max": 200.0
                                        },
                                        "precision": {
                                            "minOp": ">=",
                                            "min": 1,
                                            "maxOp": "<=",
                                            "max": 1
                                        }
                                    }
                                },
                                {
                                    "suffix": "unit",
                                    "type": "CODED_TEXT",
                                    "list": [
                                        {
                                            "value": "[degF]",
                                            "label": "[degF]",
                                            "localizedLabels": {
                                                "en": "°F"
                                            },
                                            "validation": {
                                                "range": {
                                                    "minOp": ">=",
                                                    "min": 30.0,
                                                    "maxOp": "<",
                                                    "max": 200.0
                                                },
                                                "precision": {
                                                    "minOp": ">=",
                                                    "min": 1,
                                                    "maxOp": "<=",
                                                    "max": 1
                                                }
                                            }
                                        }
                                    ]
                                }
                            ],
                            "termBindings": {
                                "LNC205": {
                                    "value": "[LNC205::8310-5]",
                                    "terminologyId": "LNC205"
                                },
                                "SNOMED-CT": {
                                    "value": "[SNOMED-CT::386725007]",
                                    "terminologyId": "SNOMED-CT"
                                }
                            }
                        },
                        {
                            "id": "time",
                            "name": "Time",
                            "rmType": "DV_DATE_TIME",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.body_temperature.v2]/data[at0002]/events[at0003]/time",
                            "inputs": [
                                {
                                    "type": "DATETIME"
                                }
                            ],
                            "inContext": true
                        },
                        {
                            "id": "language",
                            "name": "Language",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.body_temperature.v2]/language",
                            "inContext": true
                        },
                        {
                            "id": "encoding",
                            "name": "Encoding",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.body_temperature.v2]/encoding",
                            "inContext": true
                        },
                        {
                            "id": "subject",
                            "name": "Subject",
                            "rmType": "PARTY_PROXY",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-OBSERVATION.body_temperature.v2]/subject",
                            "inputs": [
                                {
                                    "suffix": "id",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_scheme",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_namespace",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "name",
                                    "type": "TEXT"
                                }
                            ],
                            "inContext": true
                        }
                    ]
                },
                {
                    "id": "clinical_synopsis",
                    "name": "Clinical synopsis",
                    "localizedName": "Clinical synopsis",
                    "rmType": "EVALUATION",
                    "nodeId": "openEHR-EHR-EVALUATION.clinical_synopsis.v1",
                    "min": 0,
                    "max": 1,
                    "localizedNames": {
                        "en": "Clinical synopsis"
                    },
                    "localizedDescriptions": {
                        "en": "Narrative summary or overview about a patient, specifically from the perspective of a healthcare provider, and with or without associated interpretations."
                    },
                    "aqlPath": "/content[openEHR-EHR-EVALUATION.clinical_synopsis.v1]",
                    "children": [
                        {
                            "id": "notes",
                            "name": "Notes",
                            "localizedName": "Notes",
                            "rmType": "DV_TEXT",
                            "nodeId": "at0002",
                            "min": 1,
                            "max": 1,
                            "localizedNames": {
                                "en": "Notes"
                            },
                            "localizedDescriptions": {
                                "en": "The summary, assessment, conclusions or evaluation of the clinical findings."
                            },
                            "aqlPath": "/content[openEHR-EHR-EVALUATION.clinical_synopsis.v1]/data[at0001]/items[at0002,'Notes']/value",
                            "inputs": [
                                {
                                    "type": "TEXT"
                                }
                            ]
                        },
                        {
                            "id": "language",
                            "name": "Language",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-EVALUATION.clinical_synopsis.v1]/language",
                            "inContext": true
                        },
                        {
                            "id": "encoding",
                            "name": "Encoding",
                            "rmType": "CODE_PHRASE",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-EVALUATION.clinical_synopsis.v1]/encoding",
                            "inContext": true
                        },
                        {
                            "id": "subject",
                            "name": "Subject",
                            "rmType": "PARTY_PROXY",
                            "min": 0,
                            "max": 1,
                            "aqlPath": "/content[openEHR-EHR-EVALUATION.clinical_synopsis.v1]/subject",
                            "inputs": [
                                {
                                    "suffix": "id",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_scheme",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "id_namespace",
                                    "type": "TEXT"
                                },
                                {
                                    "suffix": "name",
                                    "type": "TEXT"
                                }
                            ],
                            "inContext": true
                        }
                    ]
                },
                {
                    "id": "category",
                    "rmType": "DV_CODED_TEXT",
                    "nodeId": "",
                    "min": 1,
                    "max": 1,
                    "aqlPath": "/category",
                    "inputs": [
                        {
                            "suffix": "code",
                            "type": "CODED_TEXT",
                            "list": [
                                {
                                    "value": "433",
                                    "label": "event",
                                    "localizedLabels": {
                                        "en": "event"
                                    }
                                }
                            ],
                            "terminology": "openehr"
                        }
                    ],
                    "inContext": true
                },
                {
                    "id": "language",
                    "name": "Language",
                    "rmType": "CODE_PHRASE",
                    "min": 0,
                    "max": 1,
                    "aqlPath": "/language",
                    "inContext": true
                },
                {
                    "id": "territory",
                    "name": "Territory",
                    "rmType": "CODE_PHRASE",
                    "min": 0,
                    "max": 1,
                    "aqlPath": "/territory",
                    "inContext": true
                },
                {
                    "id": "composer",
                    "name": "Composer",
                    "rmType": "PARTY_PROXY",
                    "min": 0,
                    "max": 1,
                    "aqlPath": "/composer",
                    "inputs": [
                        {
                            "suffix": "id",
                            "type": "TEXT"
                        },
                        {
                            "suffix": "id_scheme",
                            "type": "TEXT"
                        },
                        {
                            "suffix": "id_namespace",
                            "type": "TEXT"
                        },
                        {
                            "suffix": "name",
                            "type": "TEXT"
                        }
                    ],
                    "inContext": true
                }
            ]
        }
    }
}
```





