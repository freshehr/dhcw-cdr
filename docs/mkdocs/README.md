# Overview

This set of documentation will introduce you to some of the principles behind openEHR and how to start working with the APIs and data formats that the various openEHR CDRs (Clinical Data Repositories) support.

#### Postman 

We strongly recommend that you orientate yourselves using the free Postman tool, as this will let you easily play with the various API calls without writing any code. Once you understand the principles it will be much easier to write code for your own environment, and better still, Postman can often generate code examples in a variety of languages.

To make life easier, we have prepared a `Postman collection` which is essentially a pre-cooked set of openEHR API calls, for which when used with Postman environment files, can be used to point very quickly at an endpoint and get playing.

Click the 'Run Postman Button' to import the Postman 'Apperta C4H openEHR REST APIs' collection and the associated Postman environment..

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/20729-17be3698-fb0b-4f4d-82c2-a128efa9c29c?action=collection%2Ffork&collection-url=entityId%3D20729-17be3698-fb0b-4f4d-82c2-a128efa9c29c%26entityType%3Dcollection%26workspaceId%3Db92254b2-1f5c-4442-8595-21165204e9dc#?env%5BDHCW%20Training%20CDR%5D=W3sia2V5IjoiY2RyUHJvZHVjdCIsInZhbHVlIjoiQmV0dGVyIiwiZW5hYmxlZCI6dHJ1ZSwic2Vzc2lvblZhbHVlIjoiQmV0dGVyIiwic2Vzc2lvbkluZGV4IjowfSx7ImtleSI6Im9wZW5laHJCYXNlVXJsIiwidmFsdWUiOiJodHRwczovL3Jlc3QuZWhyc2NhcGUuY29tL3Jlc3Qvb3BlbmVoci92MSIsImVuYWJsZWQiOnRydWUsInNlc3Npb25WYWx1ZSI6Imh0dHBzOi8vcmVzdC5laHJzY2FwZS5jb20vcmVzdC9vcGVuZWhyL3YxIiwic2Vzc2lvbkluZGV4IjoxfSx7ImtleSI6ImVocnNjYXBlQmFzZVVybCIsInZhbHVlIjoiaHR0cHM6Ly9yZXN0LmVocnNjYXBlLmNvbS9yZXN0L3YxIiwiZW5hYmxlZCI6dHJ1ZSwic2Vzc2lvblZhbHVlIjoiaHR0cHM6Ly9yZXN0LmVocnNjYXBlLmNvbS9yZXN0L3YxIiwic2Vzc2lvbkluZGV4IjoyfSx7ImtleSI6ImFjY291bnROYW1lIiwidmFsdWUiOiJESENXIHRyYWluaW5nIiwiZW5hYmxlZCI6dHJ1ZSwic2Vzc2lvblZhbHVlIjoiREhDVyB0cmFpbmluZyIsInNlc3Npb25JbmRleCI6M30seyJrZXkiOiJVc2VybmFtZSIsInZhbHVlIjoid2FsZXMtdHJhaW5pbmciLCJlbmFibGVkIjp0cnVlLCJzZXNzaW9uVmFsdWUiOiJ3YWxlcy10cmFpbmluZyIsInNlc3Npb25JbmRleCI6NH0seyJrZXkiOiJQYXNzd29yZCIsInZhbHVlIjoid2FsZXMtdHJhaW5pbmciLCJlbmFibGVkIjp0cnVlLCJzZXNzaW9uVmFsdWUiOiJ3YWxlcy10cmFpbmluZyIsInNlc3Npb25JbmRleCI6NX0seyJrZXkiOiJjb21taXR0ZXJOYW1lIiwidmFsdWUiOiJEciBGcmFua2UiLCJlbmFibGVkIjp0cnVlLCJzZXNzaW9uVmFsdWUiOiJEciBGcmFua2UiLCJzZXNzaW9uSW5kZXgiOjZ9LHsia2V5IjoicGF0aWVudE5hbWUiLCJ2YWx1ZSI6Ikl2b3IgQ294IiwiZW5hYmxlZCI6dHJ1ZSwic2Vzc2lvblZhbHVlIjoiSXZvciBDb3giLCJzZXNzaW9uSW5kZXgiOjd9LHsia2V5Ijoic3ViamVjdElkIiwidmFsdWUiOiI5OTk5OTk5MDAwIiwiZW5hYmxlZCI6dHJ1ZSwic2Vzc2lvblZhbHVlIjoiOTk5OTk5OTAwMCIsInNlc3Npb25JbmRleCI6OH0seyJrZXkiOiJuaHNOdW1iZXIiLCJ2YWx1ZSI6Ijk5OTk5OTkwMDAiLCJlbmFibGVkIjp0cnVlLCJzZXNzaW9uVmFsdWUiOiI5OTk5OTk5MDAwIiwic2Vzc2lvbkluZGV4Ijo5fSx7ImtleSI6InN1YmplY3ROYW1lc3BhY2UiLCJ2YWx1ZSI6InVrX25oc19uaHNfbnVtYmVyIiwiZW5hYmxlZCI6dHJ1ZSwic2Vzc2lvblZhbHVlIjoidWtfbmhzX25oc19udW1iZXIiLCJzZXNzaW9uSW5kZXgiOjEwfSx7ImtleSI6ImVocklkIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlLCJzZXNzaW9uVmFsdWUiOiJjZDU4OGUxMS05MjdmLTQwNWEtYmNjMy05YzU1YmQ0YmJkYmUiLCJzZXNzaW9uSW5kZXgiOjExfSx7ImtleSI6InBhcnR5SWQiLCJ2YWx1ZSI6IiIsImVuYWJsZWQiOnRydWUsInNlc3Npb25WYWx1ZSI6IiIsInNlc3Npb25JbmRleCI6MTJ9LHsia2V5IjoidGVtcGxhdGVJZCIsInZhbHVlIjoiIiwiZW5hYmxlZCI6dHJ1ZSwic2Vzc2lvblZhbHVlIjoiTWFsbnV0cml0aW9uIHVuaXZlcnNhbCBzY3JlZW5pbmcgdG9vbCIsInNlc3Npb25JbmRleCI6MTN9LHsia2V5IjoiY29tcG9zaXRpb25JZCIsInZhbHVlIjoiIiwiZW5hYmxlZCI6dHJ1ZSwic2Vzc2lvblZhbHVlIjoiIiwic2Vzc2lvbkluZGV4IjoxNH0seyJrZXkiOiJxdWVyeVN0cmluZyIsInZhbHVlIjoiIiwiZW5hYmxlZCI6dHJ1ZSwidHlwZSI6ImFueSIsInNlc3Npb25WYWx1ZSI6IlNFTEVDVCBjL3VpZC92YWx1ZSBhcyBjb21wb3NpdGlvblVpZCwgYy9jb21wb3Nlci9uYW1lIGFzIGF1dGhvciwgYy9jb250ZXh0L3N0YXJ0X3RpbWUvdmFsdWUgYXMgZGF0ZVJlY29yZGVkLC4uLiIsInNlc3Npb25JbmRleCI6MTV9XQ==)

### Which API to use?

For more information on the various APIs  see [Overview of the different CDR APIs](intro/INT2-overview-rest-apis.md).

To get started we suggest you focus on the Better Ehrscape API. Although this is proprietary to Better and will soon be supplanted by the openEHR REST API, it uses some simplified data formats that can make life much easier. These formats are being adopted by the wider CDR community as part of the openEHR CDR API, so this advice will change, once this is better established.

In other respects the Better API is conceptually very similar and you will not find it hard to adjust to the openEHR REST API, indeed one possibility is to use the standard API for most calls.


### Key content

[Getting started with Postman and the openEHR REST collection](postman/PM1-postman-getting-ready.md)

[Introduction to openEHR](intro/INT1-overview-openehr.md)

[Overview of the different CDR APIs](intro/INT2-overview-rest-apis.md)

[Working with the Better Ehrscape API](ehrscape/ECDR1-authentication.md)

[Working with the openEHR REST API](opencdr/OCDR1-authentication.md)

[Overview of the Jamaica MoHW Passport project](passport/JPASS1-passport-project-overview.md)




