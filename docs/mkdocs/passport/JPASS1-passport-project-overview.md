# Overview of the Passport-JDP project

This section covers some examples that are very specific to the JMoHW Passport project. 

The proposed JMoHW Data Platform (JDP) will be based on an openEHR CDR, which is simulated for this project by a sandbox environment provided by the Apperta UK non-profit, and based on the Better CDR product. This is free to use for demonstration purposes and dummy patient data.

The initial CDR dataset is aligned to the needs of Non-communicable disease management, including the data handled by Passport.

As well as the Passport app demonstrator, a very simple web-based prototype app will be built to simulate a clinician-facing app, making use of the same CDR and able to access/visualise the data submitted to and received from the Passport app.

!!! important "Better Ehrscape API vs. openCDR API usage"

   Most of the calls are based on the 'canonical openEHR REST API which we wil refer to as the 'openCDR' API, other than calls to the /composition resource.
   Calls to the /composition resource are currently set to  make use of the Better Ehrscape API, as the openCDR API does not currently support the `FLAT JSON` data formats that we will be using. This support is under development e.g by EhrBase and will require only a minor change when implemented.

The demonstrator use-cases for Passport are ...

### 1. Retrieve key sets of read-only data from the CDR 

Access a patient record from the JDP sandbox via the openEHR query mechanism (AQL).

The initial proposed queries are

- Current allergies
- Current and recent medications
- Immunisations
- Current problems/diagnoses
- Hospitalisations
- Recent additions to the CDR record (date, name of clinical author, name of document etc).

Final exact details to be discussed with JMoHW  but the principles will not change.
  
#### Technical Tasks

 1. Retrieve the patient JDP `ehrId` from their `subjectId` - [Retrieving the patient's ehrID](JPASS2-retrieving-an-ehrid.md) 
 2. run an AQL query to find a list of current Allergies and display the queried data - [Querying JDP Allergies](JPASS3-querying-allergies.md).
 3. run an AQL query to find a list of current Medications (if it exists) and display the queried data  - [Querying JDP Medication](JPASS4-querying-medications.md).
4.  run an AQL query to find a list of Hospitalizations and display the queried data) - [Querying JDP Hospitalization](JPASS5-querying-hospitalisations.md).
  5. run an AQL query to find a list of Immunization and display the queried data) - [Querying JDP Immunization](JPASS6-querying-immunizations.md).
6.  run an AQL query to find a list of recently created Compositions and display the queried data) - [Querying JDP recent Compositions](JPASS7-querying-compositions.md).
7.  run an AQL query to find a list of Diagnoses and display the queried data) - [Querying JDP Diagnoses](JPASS8-querying-diagnoses.md).

### 2. Work with Patient-derived Observations data

Allow patient to enter some simple observational data via their PHR app (Passport) and have it stored in the JDP-CDR. Also display all recent Observation data (however committed) via a query to the JDP-CDR.


#### Technical Tasks

1. Retrieve the patient JDP `ehrId` from their SubjectId - [Retrieving the patient's ehrID](JPASS2-retrieving-an-ehrid.md). 
2. Commit a composition to the JDP-CDR via a composition (FLAT format) - [Committing patient Observations](JPASS9-committing-observation-data.md).
3. Retrieve that composition - [retrieving patient Observations](JPASS10-retrieving-observation-data.md).
5. Query **all** recent Observation data, including that recorded on the JDP by other applications.

!!! important "Better Ehrscape API vs. openEHR REST API"

   For now, we will make use of the Better Ehrscape API for the /composition calls, rather than the openCDR API, even though the latter is what will be used by the actual JDP-CDR. 

   The primary reason is that, right now, Better support some simplified data formats which are easier for third-party vendors to work with. We know that ehrBase is actively working on support for these formats, and an early release is imminent, so this advice may change.

   We will supply some documentation to show how, once committed, the same data can be accessed and queried via the openEHR REST API.






