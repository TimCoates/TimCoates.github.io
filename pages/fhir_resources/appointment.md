---
title: Appointment | Urgent & Emergency Care Appointments
keywords: getcarerecord, structured, rest, patient
sidebar: accessrecord_rest_sidebar
permalink: /appointment.html
summary: A booking of a healthcare event among patient(s), practitioner(s), related person(s) and/or device(s) for a specific date/time.
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Introduction ##
This resource is used in two closely linked but very different scenarios.

### Booking ###
It is sent as a POST request body from the Consumer to Provider systems in order to book an appointment.

### Registering ###

{% include custom/fhir.reference.html resource="Appointment" page="CareConnect-Appointment-1" fhirname="Appointment" fhirlink="appointment.html" content="-" userlink="" %}

## Key FHIR Elements for Booking ##

The following FHIR elements are key to this implementation when <a href='book_an_appointment.html'>Booking an appointment</a>:

<table>
<tr>
<th>Element</th>
<th>Cardinality</th>
<th>Description</th>
<th>Example value(s)</th>
</tr>
<tr>
<td>Appointment.status</td><td>1..1</td><td>proposed | pending | booked | arrived | fulfilled | cancelled | noshow | entered-in-error</td><td><a href="https://www.hl7.org/fhir/valueset-appointmentstatus.html" target="_blank">https://www.hl7.org/fhir/valueset-appointmentstatus.html</a></td>
</tr>
<tr>
<td>Appointment.description</td><td>0..1</td><td>Free text field for Reason this appointment is scheduled</td><td>To get the person treated by a surgeon</td>
</tr>
<tr>
<td>Appointment.slot</td><td>1..1</td><td>The slots that this appointment is filling</td><td>Reference (Slot) <a href="https://www.hl7.org/fhir/slot.html" target="_blank">https://www.hl7.org/fhir/slot.html</a></td>
</tr>
<tr>
<td>Appointment.participant.actor (Reference | Patient)</td><td>1..*</td><td>Participants involved in appointment, Person, Location/HealthcareService or Device</td><td>Reference (Patient) <a href="https://www.hl7.org/fhir/patient.html" target="_blank">https://www.hl7.org/fhir/patient.html</a></td>
</tr>
<tr>
<td>Appointment.supportingInformation</td><td>0..1</td><td>Additional information to support the appointment</td><td>Reference (Resource)</td>
</tr>
</table>

## Key FHIR Elements for Registering ##

The following FHIR elements are key to this implementation when <a href='register_an_appointment.html'>Registering an appointment</a>:

<table>
<tr>
<th>Element</th>
<th>Cardinality</th>
<th>Description</th>
<th>Example value(s)</th>
</tr>
<tr>
<td>Appointment.status</td><td>1..1</td><td>proposed | pending | booked | arrived | fulfilled | cancelled | noshow | entered-in-error</td><td><a href="https://www.hl7.org/fhir/valueset-appointmentstatus.html" target="_blank">https://www.hl7.org/fhir/valueset-appointmentstatus.html</a></td>
</tr>
<tr>
<td>Appointment.description</td><td>0..1</td><td>Free text field for Reason this appointment is scheduled</td><td>To get the person treated by a surgeon</td>
</tr>
<tr>
<td>Appointment.slot</td><td>1..1</td><td>The slots that this appointment is filling</td><td>Reference (Slot) <a href="https://www.hl7.org/fhir/slot.html" target="_blank">https://www.hl7.org/fhir/slot.html</a></td>
</tr>
<tr>
<td>Appointment.participant.actor (Reference | Patient)</td><td>1..*</td><td>Participants involved in appointment, Person, Location/HealthcareService or Device</td><td>Reference (Patient) <a href="https://www.hl7.org/fhir/patient.html" target="_blank">https://www.hl7.org/fhir/patient.html</a></td>
</tr>
<tr>
<td>Appointment.supportingInformation</td><td>0..1</td><td>Additional information to support the appointment</td><td>Reference (Resource)</td>
</tr>
</table>

## Sample resources ##

### Booking example ###

### Registering example ###

