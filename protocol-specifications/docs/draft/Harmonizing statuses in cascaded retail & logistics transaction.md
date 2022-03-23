# Harmonizing statuses in cascaded retail & logistics transaction



## 1. Overview

A cascaded transaction in ONDC involves interaction between a retail seller (or buyer app) and the logistics provider who are both participants in the network. To ensure seamless handover of the Order from the retail seller (or buyer app) to the logistics provider and final delivery to the buyer would require harmonisation of different information between all of them.

This note proposes mapping most widely communicated information, between the retail seller app and logistics provider, to the protocol schema.



## 2. Mapping

The proposed mapping of the most widely communicated information, between the retail seller app and logistics provider, to the protocol schema is shown below:

| **Protocol Schema Attribute**                   | API                       | To be used for                                               |
| ----------------------------------------------- | ------------------------- | ------------------------------------------------------------ |
| Order.fulfillment.start.instructions.name       | /update                   | Status for pickup e.g. "Ready for pickup" (by retail seller) |
| Order.fulfillment.start.instructions.short_desc | /on_confirm or /on_update | Pick up confirmation code (PCC) (by logistics provider)      |
| Order.fulfillment.start.instructions.long_desc  | /update                   | Pickup instructions e.g. reverse QC instructions for return (by retail seller) |
| Order.fulfillment.start.instructions.images     | /on_confirm or /on_update | Pickup confirmation image e.g. QR code / Packing Slip etc. that can be provided as an image (by logistics provider) |
| Order.fulfillment.end.instructions.name         | /on_update                | Status for delivery e.g. "Delivered" (by logistics agent)    |
| Order.fulfillment.end.instructions.short_desc   | /on_confirm or /on_update | Delivery confirmation code (DCC) e.g. Article number / AWB / Shipment / Consignment / Order nos / OTP (by logistics provider) |
| Order.fulfillment.end.instructions.long_desc    | /init or /update          | Drop instructions (by retail seller)                         |
| Order.fulfillment.start.instructions.images     | /on_update                | Delivery confirmation image e.g. photo (by logistics agent)  |

