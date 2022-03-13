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
  |**Code**|**Message**|**Description**|
  |---|---|---|
  |30000|Invalid request error|Generic invalid request error|
  |30001|Provider not found|When Seller App is unable to find the provider id sent by the Buyer App|
  |30002|Provider location not found|When Seller App is unable to find the provider location id sent by the Buyer App|
  |30003|Provider category not found|When Seller App is unable to find the provider category id sent by the Buyer App|
  |30004|Item not found|When Seller App is unable to find the item id sent by the Buyer App|
  |30005|Category not found|When Seller App is unable to find the category id sent by the Buyer App|
  |30006|Offer not found|When Seller App is unable to find the offer id sent by the Buyer App|
  |30007|Add on not found|When the Seller App is unable to find the add on id sent by the Buyer App|
  |30008|Fulfillment unavailable|When Seller App is unable to find the fulfillment id sent by the Buyer App|
  |30009|Fulfilment provider unavailable|When the Seller App is unable to find fulfilment provider id sent by the Buyer App|
  |30010|Order not found|When the Seller App is unable to find the order id sent by the Buyer App|
  |30011|Invalid cancellation reason|When the Seller App is unable to find the cancellation reason in cancellation_reason_id|
  |30012|Invalid update_target|When the Seller App is unable to find the update_target in the order object|
  |30013|Update inconsistency|When the Seller App finds changes in the order object other than the update_target|
  |30014|Entity to rate not found|When the Seller App is unable to find the entity to rate in id|
  |30015|Invalid rating value|When the Seller App receives an invalid value as the rating value in value|
  |40000|Business Error|Generic business error|
  |40001|Action not applicable|When an API endpoint is not implemented by the Seller App as it is not required for their use cases and a Buyer App calls one of these endpoints|
  |40002|Item quantity unavailable|When the Seller App is unable to select the specified number in order.items[].quantity|
  |40003|Quote unavailable|When the quote sent by the Buyer App is no longer available from the Seller App|
  |40004|Payment not supported|When the payment object sent by the Buyer App is not supported by the Seller App|
  |40005|Tracking not supported|When the Seller App does not support tracking for the order in order_id|
  |40006|Fulfilment agent unavailable|When an agent for fulfilment is not available|
  |50000|Policy Error|Generic Policy Error|
  |50001|Cancellation not possible|When the Seller App is unable to cancel the order due to it's cancellation policy|
  |50002|Updation not possible|When the Seller App is unable to update the order due to it's updation policy|
  |50003|Unsupported rating category|When the Seller App receives an entity to rate which is not supported|
  |50004|Support unavailable|When the Seller App receives an object if for which it does not provide support|

  ## Acknowledgements
  This document has been adapted from the [Error Codes draft document](https://github.com/beckn/protocol-specifications/blob/draft/docs/protocol-drafts/BECKN-RFC-005-Error-Codes-Draft-01.md) from the Beckn Foundation.

*Copyright (c) 2022 ONDC. All rights reserved.*
