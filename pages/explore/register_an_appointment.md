---
title: Register an appointment
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: register_an_appointment.html
summary: "Details the register an Appointment interaction"
---

{% include important.html content="Site under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Use case ##

A system wishes to register a <a href='book_an_appointment.html'>previously booked Appointment</a> in the Registry.
**NB The registry has not yet been delivered.**

## Security ##

- Utilises a JSON Web Token (JWT) to transmit Consumer system identity and authorisation details.
- Utilises TLS Mutual Authentication for system level authentication.

## Request Body ##

The request body is sent using an http `POST` method.

The body is a valid Appointment resource which conforms to <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>the relevant profile</a>

The Appointment resource MUST NOT include the following data items:

| Name | Description |
|---|---|
| id | The identity of the appointment will be assigned by the Providing system at the point of booking, and MUST NOT be included in the request body. |
| contained | No additional resources should be contained in the appointment |
| end | The appointment end time is not required. |
| supportingInformation | No supporting Information should be included in the registry. |
| description | No description should be included in the registry. |
| slot | The details of the slot are irrelevant for the purposes of the registry. |


The Appointment resource MUST include the following data items:

| Name | Value | Description |
|---|---|---|
| status | `booked` | Indicates that the Appointment is being created in an already booked state. |
| start | instant | A full timestamp in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601) of when the Appointment starts |
| created | instant | When the appointment is being booked in <a href='http://hl7.org/fhir/STU3/datatypes.html#instant'>FHIR instant</a> format (ISO 8601). This detail will optionally be used to filter and/or to expire registered appointments. |
| participant | reference | A <a href='https://nhsconnect.github.io/fhir-policy/national-services.html#FHIR-NAT-01'>national service reference</a> to the Patient for whom this Appointment is being booked, for example: `https://demographics.spineservices.nhs.uk/Patient/1234567890` where the Patient's NHS Number is 1234567890|

## Response ##

### Success ###
Where the request succeeded, the response MUST include a status of `201` **Created**.
The response MUST include a Location header giving the absolute URL of the created Appointment. This URL WILL remain stable, and the resource WILL support RESTful updates using a PUT request to this URL.
The response body WILL include the created Appointment, this resource WILL include the newly assigned id of the resource.

### Failure ###
- If the request fails because of a business rule, the response WILL include a status of `422` **Unprocessable Entity** <a href='http://hl7.org/fhir/STU3/http.html#2.21.0.10.1'>as described here</a>.
This will be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because the request body failed validation against the relevant profiles, the response WILL include a status of `422` **Unprocessable Entity** <a href='http://hl7.org/fhir/STU3/http.html#2.21.0.10.1'>as described here</a>.
This WILL be accompanied by an OperationOutcome resource providing additional detail.
- If the request fails because either no valid JWT is supplied or the supplied JWT failed validation, the response WILL include a status of `403` **Forbidden**.
This WILL be accompanied by an OperationOutcome resource providing additional detail.

- If the request fails because the request body was simply invalid, the response WILL include a status of `400` **Bad Request**.
- If the request fails because of a server error, the response WILL include a status of `500` **Internal Server Error**.

Failure responses with a `4xx` status SHOULD NOT be retried without taking steps to address the underlying cause of the failure.

Failure responses with a `500` status MAY be retried.

## Sample request body ##

```xml
<Appointment xmlns="http://hl7.org/fhir">
    <meta>
        <profile value="https://profile.url/"></profile>
    </meta>
    <identifier>
        <system value="urn:ietf:rfc:3986"></system>
        <value value="http://test.nhs.uk/test3"></value>
    </identifier>
    <status value="booked"></status>
    <start value="2019-02-01T10:51:23.620+00:00"></start>
    <created value="2019-02-01T10:51:23+00:00"></created>
    <participant>
        <actor>
            <identifier>
                <use value="official"></use>
                <system value="https://fhir.nhs.uk/Id/nhs-number"></system>
                <value value="1234554321"></value>
            </identifier>
        </actor>
    </participant>
</Appointment>
```