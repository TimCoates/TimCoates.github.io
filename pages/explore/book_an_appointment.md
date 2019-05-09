---
title: Book an appointment
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: book_an_appointment.html
summary: "Details the Search for free slots functionality required"
---

{% include important.html content="Site under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Use case ##

A consuming system wishes to book an Appointment for a Patient into <a href="search_free_slots.html">a previously retrieved</a> Slot.

## Security ##

- Utilises a JSON Web Token (JWT) to transmit Consumer system identity and authorisation details.
- Is routed through the SSP (Spine secure Proxy)
- Utilises TLS Mutual Authentication for system level authentication.

## Request Body ##

A valid Appointment resource which conforms to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>the relevant profile</a>

The Appointment resource SHALL NOT include the following data items:

| Name | Description |
|---|---|
| id | The identity of the appointment will be assigned by the Providing system at the point of booking, and MUST NOT be included in the request body. |


The Appointment resource SHALL include the following data items:

| Name | Value | Description |
|---|---|---|
| status | `booked` | Indicates that the Appointment is being created in an already booked state. |
| start | instant | A full timestamp in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |
| end | instant |  A full timestamp in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |
| supportingInformation | reference | A reference to a contained resource (see below) which describes an associated document. |
| description | Call reason | Text describing the need for the appointment, to be shown for example in an appointment list |
| slot | reference | A reference to the <a href="search_free_slots.html">a previously retrieved</a> Slot. |
| created | instant | When the appointment is being booked in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |
| participant | reference | A reference to a contained resource (see below) which describes the Patient for whom this Appointment is being booked |



### Contained resources ###

The appointment resource SHALL have two <a href='http://hl7.org/fhir/STU3/references.html#contained'>contained</a> resources. Note that contained resources are given an identifier which is only required to be unique within the scope of the containing resource, and are referenced using that identifier prefixed with a Hash `#` character.

#### Patient ####
A contained Patient resource which conforms to the <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1'>Care Connect Patient profile</a>.
This resource is referenced in the Appointment's participant element, and is used to convey the details of the Patient for whom the Appointment is being booked.
The Patient resource SHALL include the following data items:

| Name | Value | Description |
|---|---|---|
| id | Any | Any identifier, used to reference the resource from the `Appointment.Participant` element |
| identifier | NHS Number | The Patient's NHS Number as defined in the <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1'>Care Connect Patient profile</a> |
| name | Patient's name | Name as retrieved from PDS, including Prefix, Given and Family components |
| telecom | Contact number | The number the Patient can be called back on |
| gender | male \| female \| other \| unknown | The gender as retrieved from PDS |
| birthdate | yyyy-mm-dd | Patient's DOB |
| address | Address | Patient's full address as retrieved from PDS |

#### DocumentReference ####
A contained DocumentReference resource which conforms to <b>TBC</b> profile.
This resource is referenced in the appointment's supportingInformation element, it describes the type and identifier(s) of any supporting information, for example a CDA document which may be transferred separately.
The DocumentReference resource SHALL include the following data items:

| Name | Value | Description |
|---|---|---|
| id | Any | Any identifier, used to reference the resource from the Appointment.supportingInformation element |
| identifier | see below | Identifies the supporting information (i.e. CDA document) |
| identifier.system | `https://tools.ietf.org/html/rfc4122` | Indicates that the associated value is a UUID. |
| identifier.value | [UUID] | The UUID of the associated CDA (XPath: `/ClinicalDocument/id/@root`) |
| status | "current" | Indicates that the associated document is current. No other value is expected. |
| type | A value from `urn:oid:2.16.840.1.113883.2.1.3.2.4.18.17` | Indicates the type of document |
| content | see below | Describes the actual document |
| content.attachment | Describes the actual document |
| content.attachment.contentType | A valid mime type | Indicates the mime type of the document |
| content.attachment.language | `en` | States that the document is in English |

```json
{
    "resourceType": "Appointment",
    "meta": {
        "profile": "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"
    },
    "language": "en",
    "text": "<div>Appointment</div>",
    "contained": [
        {

            "resourceType": "DocumentReference",
            "id": "123",
            "identifier": {
                "system": "https://tools.ietf.org/html/rfc4122",
                "value": "A709A442-3CF4-476E-8377-376500E829C9"
            },
            "status": "current",
            "type": {
                "coding": [
                    {
                        "system": "urn:oid:2.16.840.1.113883.2.1.3.2.4.18.17",
                        "code": "POCD_MT200001GB02",
                        "display": "Integrated Urgent Care Report"
                    }
                ]
            },
            "indexed": "2018-12-20T09:43:41+11:00",
            "content": [
                {
                    "attachment": {
                        "contentType": "application/hl7-v3+xml",
                        "language": "en"
                    }
                }
            ]
        },
        {
            "resourceType": "Patient",
            "meta": {
            "profile": "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1"
        },
            "id": "P1",
              "identifier": [
    {
      "extension": [
        {
          "url": "https://fhir.hl7.org.uk/STU3/StructureDefinition/Extension-CareConnect-NHSNumberVerificationStatus-1",
          "valueCodeableConcept": {
            "coding": [
              {
                "system": "https://fhir.hl7.org.uk/STU3/CodeSystem/CareConnect-NHSNumberVerificationStatus-1",
                "code": "01",
                "display": "Number present and verified"
              }
            ]
          }
        }
      ],
      "use": "official",
      "system": "https://fhir.nhs.uk/Id/nhs-number",
      "value": "9476719931"
    }
  ],
            "name": [
                {
                  "use": "official",
                    "prefix": "Mr",
                    "given": "John",
                    "family": "Smith"
                }
            ],
            "telecom": [
                {
                    "system": "phone",
                    "value": "01234 567 890",
                    "use": "home",
                    "rank": 1
                }
            ],
            "gender": "male",
            "birthDate": "1974-12-25",
            "address": [
                {
                    "use": "home",
                    "text": "123 High Street, Leeds LS1 4HR",
                    "line": [
                        "123 High Street",
                        "Leeds"
                    ],
                    "city": "Leeds",
                    "postalCode": "LS1 4HR"
                }
            ]
        }
    ],
    "status": "booked",
    "start": "2019-01-17T15:00:00.000Z",
    "end": "2019-01-17T15:10:00.000Z",
    "supportingInformation": [
      {
            "reference": "#123"
        }
    ],
    "description": "Reason for calling",
    "slot": [
        {
            "reference": "Slot/slot002"
        }
    ],
    "created": "2019-01-18T14:32:22.579+00:00",

    "participant": [
        {
            "actor": {
                "reference": "#P1",
                "identifier": {
                    "use": "official",
                    "system": "https://fhir.nhs.uk/Id/nhs-number",
                    "value": "1234554321"
                },
                "display": "Peter James Chalmers"
            },
            "status": "accepted"
        }
    ]
}
```