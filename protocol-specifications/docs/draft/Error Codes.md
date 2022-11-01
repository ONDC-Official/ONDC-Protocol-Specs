# ONDC Error Codes (for Buyer App, Seller App, Gateway)

## ID: 
ONDC-RFC-001

## Draft ID
Draft-01

## Title:
ONDC Error Codes

## Category:
Network Policy

## Status:
Protocol Draft

## Published on:
March 13, 2022

## Expires on:
March 12, 2023 or Date of publication of next draft which ever is earlier

## License:
CC-BY-ND

## Authors:
1. Supriyo Ghosh : supriyo@ondc.org

## Introduction
  This document outlines the error codes which must be returned by a Buyer App, Seller App or Gateway. 

  ## Error Codes
  |**Code**|**Type**|**Message**|**Description**|
  |---|---|---|---|
  |10000|Gateway|Bad or Invalid request error|Generic bad or invalid request error|
  |10001|Gateway|Invalid Signature|Cannot verify signature for request|
  |10002|Gateway|Invalid City Code|Valid city code needs to be passed for search|
  |20000|Buyer App|Invalid catalog item|Catalog Item cannot be displayed as it does not meet statutory requirements|
  |20001|Buyer App|Invalid Signature|Cannot verify signature for request|
  |30000|Seller App|Invalid request error|Generic invalid request error|
  |30001|Seller App|Provider not found|When Seller App is unable to find the provider id sent by the Buyer App|
  |30002|Seller App|Provider location not found|When Seller App is unable to find the provider location id sent by the Buyer App|
  |30003|Seller App|Provider category not found|When Seller App is unable to find the provider category id sent by the Buyer App|
  |30004|Seller App|Item not found|When Seller App is unable to find the item id sent by the Buyer App|
  |30005|Seller App|Category not found|When Seller App is unable to find the category id sent by the Buyer App|
  |30006|Seller App|Offer not found|When Seller App is unable to find the offer id sent by the Buyer App|
  |30007|Seller App|Add-on not found|When the Seller App is unable to find the add-on id sent by the Buyer App|
  |30008|Seller App|Location Serviceability Error|Pickup location not serviceable by Logistics Provider|
  |30009|Seller App|Location Serviceability Error|Dropoff location not serviceable by Logistics Provider|
  |30010|Seller App|Location Serviceability Error|Delivery distance exceeds the maximum serviceability distance|
  |30011|Seller App|Order Serviceability Error|Delivery Partners not available|
  |30012|Seller App|Invalid cancellation reason|When the Seller App is unable to find the cancellation reason in cancellation_reason_id|
  |30013|Seller App|Update inconsistency|When the Seller App finds changes in the order object other than the update_target|
  |30014|Seller App|Entity to rate not found|When the Seller App is unable to find the entity to rate in id|
  |30015|Seller App|Invalid rating value|When the Seller App receives an invalid value as the rating value in value|
  |30016|Seller App|Invalid Signature|Cannot verify signature for request|
  |30017|Seller App|Merchant Unavailable|Merchant is currently not taking orders|
  |30018|Seller App|Invalid Order|Order not found|
  |30019|Seller App|Cannot Confirm Order|Seller App is unable to confirm the order|
  |40000|Seller App|Business Error|Generic business error|
  |40001|Seller App|Action not applicable|When an API endpoint is not implemented by the Seller App as it is not required for their use cases and a Buyer App calls one of these endpoints|
  |40002|Seller App|Item quantity unavailable|When the Seller App is unable to select the specified number in order.items[].quantity|
  |40003|Seller App|Quote unavailable|When the quote sent by the Buyer App is no longer available from the Seller App|
  |40004|Seller App|Payment not supported|When the payment object sent by the Buyer App is not supported by the Seller App|
  |40005|Seller App|Tracking not supported|When the Seller App does not support tracking for the order in order_id|
  |40006|Seller App|Fulfilment agent unavailable|When an agent for fulfilment is not available|
  |50000|Seller App|Policy Error|Generic Policy Error|
  |50001|Seller App|Cancellation not possible|When the Seller App is unable to cancel the order due to it's cancellation policy|
  |50002|Seller App|Updation not possible|When the Seller App is unable to update the order due to it's updation policy|
  |50003|Seller App|Unsupported rating category|When the Seller App receives an entity to rate which is not supported|
  |50004|Seller App|Support unavailable|When the Seller App receives an object if for which it does not provide support|
  |60001|Logistics|Location Serviceability Error|Pickup location not serviceable by Logistics Provider|
  |60002|Logistics|Location Serviceability Error|Dropoff location not serviceable by Logistics Provider|
  |60003|Logistics|Location Serviceability Error|Delivery distance exceeds the maximum serviceability distance|
  |60004|Logistics|Order Serviceability Error|Delivery Partners not available|
  |60005|Logistics|Invalid Signature|Cannot verify signature for request|
  |60006|Logistics|Invalid Request|Invalid request, not compliant with API contract|
  |60007|Logistics|Policy Error|Cancellation not possible because of Logistics Provider policy|

  ## Acknowledgements
  This document has been adapted from the [Error Codes draft document](https://github.com/beckn/protocol-specifications/blob/draft/docs/protocol-drafts/BECKN-RFC-005-Error-Codes-Draft-01.md) from the Beckn Foundation.

*Copyright (c) 2022 ONDC. All rights reserved.*
