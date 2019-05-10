---
title: Overview
keywords: homepage
sidebar: overview_sidebar
permalink: developing.html
toc: false
---

{% include important.html content="This site is under development by NHS Digital, It is advised not to develop against these specifications until a formal announcement has been made." %}

## Provider Demonstrator ##
To support development, a <a href='http://appointments.directoryofservices.nhs.uk:443/poc/index'>synthetic Provider system</a> has been created. This allows development to proceed without being tightly coupled to the timescales of another system supplier.

## Validating resources ##
FHIR resources used in this specification can be valiated against their profiles <a href='https://data.developer.nhs.uk/ccri/term/validate'>using this site</a>, alternatively the resources can be POSTed to: https://data.developer.nhs.uk/ccri-fhir/STU3/[ResourceType]/$validate (making sure to set [ResourceType] to the appropriate type of Resource) using a REST client (such as <a href='https://www.getpostman.com/'>POSTman</a>).

## JWT utilities ##
Once a JWT has been created, there are a couple of useful public resources for decoding them, <a href='https://jwt.io/'>jwt.io</a> is useful, however <a href='http://jwt.ms/'>Jwt.ms</a>Is slightly more user friendly.