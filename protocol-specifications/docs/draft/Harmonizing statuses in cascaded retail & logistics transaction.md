# Harmonizing statuses in cascaded retail & logistics transaction



## 1. Overview

A cascaded transaction in ONDC involves interaction between a retail seller (or buyer app) and the logistics provider who are both participants in the network. To ensure seamless handover of the Order from the retail seller (or buyer app) to the logistics provider and final delivery to the buyer would require harmonisation of different information between all of them.

This note proposes mapping most widely communicated information, between the retail seller app and logistics provider, to the protocol schema.



## 2. Mapping

The proposed mapping of the most widely communicated information, between the retail seller app and logistics provider, to the protocol schema is shown below:

| API                       | Called by          | To be used for                                               | Protocol Schema Mapping                                      |
| ------------------------- | ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| /search                   | Retail Seller App  | CoD collection preference                                    | Intent.payment.collection_preference                         |
| /search                   | Retail Seller App  | Determining serviceability based on weight and / or dimensions of package | Intent.”./ondc-payload_details”.weight<br />Intent.”./ondc-payload_details”.dimensions |
| /search                   | Retail Seller App  | Determine serviceability based on pickup or delivery pin code | Intent.fulfillment.start.location.address.area_code<br />Intent.fulfillment.end.location.address.area_code |
| /order                    | Retail Seller App  | Payload for retail order - items (id, name, quantity, unit price)<br />Seller (name, address);<br />Order (value, weight, dimensions, payment mode & other details) | Order.”./ondc-linked_orders”.items<br />Order.”./ondc-linked_orders”.provider<br />Order.”./ondc-linked_orders”.order |
| /update                   | Retail Seller App  | Status for pickup                                            | Order.fulfillment.start.instructions.name                    |
| /on_confirm or/on_update  | Logistics Provider | Pickup confirmation code (PCC)                               | Order.fulfillment.start.instructions.short_desc              |
| /update                   | Retail Seller App  | Pickup instructions e.g. reverse QC instructions for return  | Order.fulfillment.start.instructions.long_desc               |
| /on_confirm or /on_update | Logistics Provider | Pickup confirmation image e.g. QR code / Packing Slip etc. that can be provided as an image | Order.fulfillment.start.instructions.images                  |
| /on_update                | Logistics Agent    | Status for delivery e.g. "Delivered"                         | Order.fulfillment.end.instructions.name                      |
| /on_confirm or /on_update | Logistics Provider | Delivery confirmation code (DCC) e.g. Article number / AWB / Shipment / Consignment / Order nos / OTP | Order.fulfillment.end.instructions.short_desc                |
| /init or /update          | Retail Seller App  | Drop instructions                                            | Order.fulfillment.end.instructions.long_desc                 |
| /on_update                | Logistics Agent    | Delivery confirmation image e.g. photo                       | Order.fulfillment.end.instructions.images                  |

