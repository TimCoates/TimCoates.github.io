---
title: Resources Overview
keywords: getcarerecord, structured, rest, resource
sidebar: foundations_sidebar
permalink: resources_overview.html
summary: "This page provides an overview of the FHIR STU3 Resources that are required to build the required API messaging. Each link will take you to the resource page detail with a link to the StructureDefinitions of each resource."
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Urgent & Emergency Care Appointments API's ##

The “disposition” from the patient interaction (NHS111 call etc) determines the urgency (giving the time bounds). The DOS (Directory of Services) will  have been interrogated and an appropriate service for the Patient identified.

- The time bounds and a specific service identifier are passed to the API
- The provider system searches for free slots within the time period given, for the identified service
- The Patient (or the 111 call handler) will review the free slots & pick one
- The provider system will then be sent the slot details & patient detail and will book an appointment into the slot

<img src="images/UEC-Appointments/UEC_Appointments_Flow.png">

| Resource | Description | Profile |
| --- | --- | --- |
| <a href='appointment.html'>Appointment</a> | The appointment that is booked, linking a specific Patient into a specific Slot | <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Appointment-1'>CareConnect-Appointment-1</a> |
| <a href='slot.html'>Slot</a> | A free time period, into which an appointment with a specific Patient can be booked | <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Slot-1'>CareConnect-Slot-1</a> |
| <a href='schedule.html'>Schedule</a> | A grouping of Slots, used to link them to the HealthcareService which the slots are provided as part of | <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Schedule-1'>CareConnect-Schedule-1</a> |
| <a href='practitioner.html'>Practitioner</a> | A Practitioner which may optionally be assigned to deliver a given Schedule | <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Practitioner-1'>CareConnect-Practitioner-1</a> |
| <a href='practitioner_role.html'>PractitionerRole</a> | A PractitionerRole which may optionally be assigned to the delivery of a given Schedule | <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-PractitionerRole-1'>CareConnect-PractitionerRole-1</a> |
| <a href='healthcare_service.html'>HealthcareService</a> | A HealthcareService which has been selected from the DoS, and delivers one or more Schedules | <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-HealthcareService-1'>CareConnect-HealthcareService-1</a> |
| <a href='organisation.html'>Organization</a> | An Organisation which delivers one or more HealthcareServices | <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Organization-1'>CareConnect-Organization-1</a> |
| <a href='location.html'>Location</a> | A Location at which an Organisation delivers one or more HealthcareServices | <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Location-1'>CareConnect-Location-1</a> |
| <a href='patient.html'>Patient</a> | A Patient for whom an appointment is being booked | <a href='https://fhir.hl7.org.uk/STU3/StructureDefinition/CareConnect-Patient-1'>CareConnect-Patient-1</a> |
| <a href='document_reference.html'>DocumentReference</a> | A DocumentReference which points to a document which gives information supporting the Appointment | <a href='#'>TBC</a> |

