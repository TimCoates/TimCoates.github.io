---
title: Cancel an appointment
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: cancel_an_appointment.html
summary: "Details the Cancel an Appointment interaction"
---

{% include important.html content="Site under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Use case ##

A consuming system wishes to cancel an Appointment which was previously <a href='book_an_appointment.html'>booked</a>.

## Security ##

- Utilises a JSON Web Token (JWT) to transmit Consumer system identity and authorisation details.
- Is routed through the SSP (Spine secure Proxy)
- Utilises TLS Mutual Authentication for system level authentication.

## Request Body ##

The request body is sent using an http `PUT` method.

The body is a valid Appointment resource which conforms to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>the relevant profile</a>. **NB The appointment resource MUST be <a href='get_an_appointment.html'>retrieved from the Provider system</a> in order to ensure that no data is lost.**

The following data items in the <a href='get_an_appointment.html'>retrieved Appointment</a> resource MUST be changed as defined:

| Name | Value | Description |
|---|---|---|
| status | `cancelled` | Indicates that the Appointment is being changed to a `cancelled` state. |
| created | instant | When the appointment is being cancelled in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) |

**No other elements of the Appointment resource may be changed**

- Provider systems SHOULD store previous versions of the resource to defend against any such loss of data.
- Provider systems SHOULD reject cancellation requests where differences (other than those described above) are detected between the original and updated resource.


## Response ##

### Success ###
Where the request succeeded, the response MUST include a status of `200` **OK**.
The response MUST include a Location header giving the absolute URL of the created Appointment. This URL MUST remain stable, and the resource SHOULD support RESTful updates using a PUT request to this URL.
The response body MUST include the updated Appointment, this resource MUST include the newly assigned id of the resource.

### Failure ###
- If the request fails because of a business rule (for example if differences are detected between the existing and updated Appointment), the response MUST include a status of `422` **Unprocessable Entity** <a href='http://hl7.org/fhir/STU3/http.html#2.21.0.10.1'>as described here</a>.
This SHOULD be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because the request body failed validation against the relevant profiles, the response MUST include a status of `422` **Unprocessable Entity** <a href='http://hl7.org/fhir/STU3/http.html#2.21.0.10.1'>as described here</a>.
This SHOULD be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because either no valid JWT is supplied or the supplied JWT failed validation, the response MUST include a status of `403` **Forbidden**.
This SHOULD be accompanied by an OperationOutcome resource providing additional detail.

- If the request fails because the request body was simply invalid, the response MUST include a status of `400` **Bad Request**.
- If the request fails because of a server error, the response MUST include a status of `500` **Internal Server Error**.

Failure responses with a `4xx` status SHOULD NOT be retried without taking steps to address the underlying cause of the failure.

Failure responses with a `500` status MAY be retried.

## Sample request body ##

```json
{
    "resourceType": "Appointment",
    "meta": {
        "profile": "https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1"
    },
    "id": "efea8f22-0c33-4000-b4e7-a28569d65e91",
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
    "status": "cancelled",
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