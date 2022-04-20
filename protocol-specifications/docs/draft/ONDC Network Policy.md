# ONDC Network Policy (draft)



## 1. Overview

ONDC will define network policies that will govern the operation of the network, for each of the following:

- Registration of Participants
- Data Privacy and Security
- [Payment and Settlement Protocol](https://docs.google.com/document/d/1iqLdayk488ekEzKrEs-yn6gVrevbxBkILBe5j4oIxMY/edit)
- Use-cases for different domains
- Open Data Protocol
- Transaction Authentication & Non-repudiation
- Grievances & Disputes
- Frauds

A subset of the above network policies have been codified, in the form of an “ONDC Protocol” commerce layer on top of the underlying Beckn protocol, and includes the following:

- Baseline configurations (for codification of domains supported, validity of message requests for APIs, codification of city / country / currency / language);
- ONDC Schema Policy - defines an optimal set of mandatory attributes and additional non-standard attributes to meet statutory and other functional requirements. Non-standard attributes are namespaced and prefixed by “./ondc-”;
- Error Codes

ONDC Protocol includes the following:

- ONDC Protocol Core API - this is the core API from which the domain specific APIs (below) are derived. Links are below:
  - [Github](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol/blob/master/protocol-specifications/core/v0/api/core.yaml)
  - [SwaggerHub](https://app.swaggerhub.com/apis/ONDC/ONDC-Protocol-Core/1.0.0-draft)
- ONDC Protocol API for Retail (hyperlocal, F&B) - this is the API for retail (hyperlocal). Links are below:
  - [Github](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol/blob/master/protocol-specifications/core/v0/api/retail-hyperlocal.yaml)
  - [SwaggerHub](https://app.swaggerhub.com/apis/ONDC/ONDC-Protocol-Hyperlocal/1.0.0-draft)
- ONDC Protocol API for Logistics - this is the API for Logistics. Links are below:
  - [Github](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol/blob/master/protocol-specifications/core/v0/api/logistics.yaml)
  - [Swaggerhub](https://app.swaggerhub.com/apis/ONDC/ONDC-Protocol-Logistics/1.0.0-draft)
- ONDC Error Codes. Links are below:
  - [Github](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/blob/master/protocol-specifications/docs/draft/Error%20Codes.md)
- Harmonization of information between retail seller and logistics provider. Links are below:
  - [Github](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/blob/master/protocol-specifications/docs/draft/Harmonizing%20statuses%20in%20cascaded%20retail%20%26%20logistics%20transaction.md)


ONDC Protocol will continue to evolve with the ONDC network. All issues, related to the ONDC Protocol APIs, should be logged to [Github](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol/issues)) 
