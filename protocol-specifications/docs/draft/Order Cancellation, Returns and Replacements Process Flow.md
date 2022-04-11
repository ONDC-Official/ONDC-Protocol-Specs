# Order Cancellation, Returns & Replacements Process Flow - v0.3 (DRAFT)



## 1. Overview

The objective of this note is to propose process flows for Cancellation, Return & Replacement. Cancellation, Return & Replacement can be processed by Buyer & Seller Apps, using existing protocol APIs. The detailed process flows and sequence diagrams are defined below.

This note proposes detailed process flows for the following:

- **Cancellation -** for the full order or for part of the order, i.e. some of the items in the order;
- **Return** - for one or more items in an order. A part return will be defined as return of one or more items in an order but not return of all items in the order. A full return will be defined as the return of all items in the order.
- **Replacement** - for one or more items in an order. A part replacement will be defined as replacement of one or more items in an order but not replacement of all items in the order. A full replacement will be defined as the replacement of all items in the order. Note that a replacement can also be handled by part or full return of an order and creating a new order with the items that are being partly or fully replaced.

The money trail, associated with cancellation, return & replacement, will be addressed separately in the Payment & Settlement protocol document.

## 2. Cancellation

- **Criteria**

  Criteria for order cancellation are defined below:

  - An order is cancellable by default and a non-cancellable order, if any, will be defined as such by the seller through the seller app as part of the order confirmation process;
  - Cancellation is possible any time before acceptance of delivery by buyer;
  - Cancellation may be initiated by
    - Buyer or Buyer App
    - Seller App
    - Logistics Provider
  - This also covers cases where buyer may refuse acceptance on attempt to deliver;

- **Cancellation Reasons**

  Criteria for order cancellation along with cancellation reason codes (cancellation_reason_id, descriptor.code) are defined below:
  
| Code | Reason for Cancellation                                      |
| ---- | ------------------------------------------------------------ |
| 001  | Order delivery delayed or not possible                       |
| 002  | Price of one or more items have changed due to which buyer was asked to make additional payment |
| 003  | One or more items in the Order not available                 |
| 004  | Delivery pin code not serviceable                            |
| 005  | Buyer refused to accept delivery                             |



- **Process flow - Buyer or Buyer App Initiated**

  Buyer can initiate cancellation of order. Alternatively, Buyer App may have a policy to initiate cancellation of order without buyer’s request, due to breach of policy e.g. (shipping time committed + “X” days grace) has lapsed.

  If buyer initiates the cancellation:

  - Buyer requests for order cancellation through the Buyer App, which should accept the cancellation request if the above mentioned criteria are met;
  - Buyer confirms cancellation;

  If buyer app initiates the cancellation, the process flow starts from here:

  - Buyer App sends order cancellation request to Seller App, which should accept the cancellation request if the above mentioned criteria are met. If the Seller App does not accept the cancellation request, they can also return a policy error (code 50001) as defined [here](https://github.com/beckn/protocol-specifications/blob/core-0.9.4-draft/docs/protocol-drafts/BECKN-RFC-005-Error-Codes-Draft-01.md);
  - Order cancellation with the Logistics Provider will be initiated by the entity that confirmed the order with the Logistics Provider, i.e. Buyer App or Seller App;
  - In case cancellation is requested after shipment, but before acceptance of delivery, Logistics Provider may like to recover shipping charges through the Seller / Buyer App which will be as per their mutually agreed upon terms & conditions during order confirmation *(while recovery may be possible for prepaid orders, no recovery is possible for CoD orders.* *Therefore, for now, there will be no provision for recovery, from the Buyer, of shipping charges irrespective of CoD or prepaid order and the buyer will be issued a full refund);*
  - Logistics Provider confirms order cancellation to Seller / Buyer App;
  - Seller App confirms order cancellation to Buyer App and updates the status of the order;
  - Buyer App confirms order cancellation to Buyer;
  - Buyer will be issued full refund, for prepaid order, from the entity that collected the payment (will be covered in detail in “Payment and Settlement Protocol”);
![image6](https://user-images.githubusercontent.com/95357304/158113442-394b2e0b-747d-4204-b924-d957c19219c8.png)
![image7](https://user-images.githubusercontent.com/95357304/158113361-fdb901fc-b220-44ef-b8e3-350368c6a8f7.png)

- **Sequence diagram - Buyer or Buyer App initiated**.

  **Cancellation of Full Order**

  - Cancellation of order is initiated by Retail Buyer App;
  - This assumes that the order with the Logistics Provider was initiated & confirmed by Retail Seller App. In case the order was initiated & confirmed by Retail Buyer App, participants may adapt the sequence diagram appropriately;
![image4](https://user-images.githubusercontent.com/95357304/158114806-809abbe9-1fe4-4b1c-ac8d-fbb586261097.png)

  **Cancellation of Part Order**

  - Cancellation of order is initiated by Retail Buyer App;
  - This assumes that the order with the Logistics Provider was initiated & confirmed by Retail Seller App. In case the order was initiated & confirmed by Retail Buyer App, participants may adapt the sequence diagram appropriately;
  - This sequence diagram follows the same flow as return but without the reverse logistics;
![image12](https://user-images.githubusercontent.com/95357304/158114919-9daed3d4-93ef-49c2-885f-0d24bcf2c9c4.png)

- **Process flow - Seller App Initiated**

  If seller app initiates the cancellation, the process flow starts from here:

  - Seller App sends order cancellation request to Buyer App, which should accept the cancellation request if the above mentioned criteria are met;
  - Order cancellation with the Logistics Provider will be initiated by the entity that confirmed the order with the Logistics Provider, i.e. Buyer App or Seller App;
  - In case cancellation is requested after shipment, but before acceptance of delivery, Logistics Provider may like to recover shipping charges through the Seller / Buyer App which will be as per their mutually agreed upon terms & conditions during order confirmation *(while recovery may be possible for prepaid orders, no recovery is possible for CoD orders.* *Therefore, for now, there will be no provision for recovery, from the Buyer, of shipping charges irrespective of CoD or prepaid order and the buyer will be issued a full refund);*
  - Logistics Provider confirms order cancellation to Seller / Buyer App;
  - Buyer will be issued full refund, for prepaid order, from the entity that collected the payment (will be covered in detail in “Payment and Settlement Protocol”);
![image8](https://user-images.githubusercontent.com/95357304/158113697-73e64425-4bb6-460e-984c-bf581d9898e2.png)

- **Sequence diagram - Seller App initiated**

  **Cancellation of Full Order**

  - Cancellation of order is initiated by Retail Seller App;
  - This assumes that the order with the Logistics Provider was initiated & confirmed by Retail Seller App. In case the order was initiated & confirmed by Retail Buyer App, participants may adapt the sequence diagram appropriately;
![image11](https://user-images.githubusercontent.com/95357304/158115046-507910bb-5813-4270-a16c-1d7da4c929b7.png)

  **Cancellation of Part Order**

  - Cancellation of order is initiated by Retail Seller App;
  - This assumes that the order with the Logistics Provider was initiated & confirmed by Retail Seller App. In case the order was initiated & confirmed by Retail Buyer App, participants may adapt the sequence diagram appropriately;
  - This sequence diagram follows the same flow as return but without the reverse logistics; 
  - ![image9](https://user-images.githubusercontent.com/95357304/158115096-2802165a-1564-4d2b-af8b-f76454bf8d41.png)

- **Process Flow - Logistics Provider Initiated**

  If logistics provider initiates the cancellation, the process flow starts from here:

  - Logistics Provider sends order cancellation request to entity that confirmed the logistics order i.e. Buyer App or Seller App, which should accept the cancellation request if the above mentioned criteria are met;
  - Entity that confirmed the order (Seller App or Buyer App) searches for a new logistics provider, with the same criteria as the previous logistics provider, to deliver the order. In case a new logistics provider cannot be found, the retail order is cancelled;
  - If the retail order is cancelled, Buyer will be issued full refund, for prepaid order, from the entity that collected the payment (will be covered in detail in “Payment and Settlement Protocol”);
![image10](https://user-images.githubusercontent.com/95357304/158115175-b4be2ccc-a672-4e74-aeab-b3505c82ba44.png)

- **Sequence diagram - Logistics Provider Initiated**
![image2](https://user-images.githubusercontent.com/95357304/158115243-364049d1-fd70-4a05-b894-6d40dbea80ce.png)



## **3. Returns & Replacements**

Buyer can initiate a return or replacement from a feature in the Buyer App, from which the order was confirmed. An item, in an order, may be returnable or replaceable. Not all items in an order may be returnable or replaceable. A buyer can only choose to return / replace one or more items that were defined as returnable or replaceable.

Irrespective of how the return is initiated, return & replacement results in an amendment to the order confirmed earlier.

- **Process Flow - Returns**

  A return will follow the following steps:

  - Buyer initiates the return from the buyer app. To initiate the return, the buyer selects one or more items, from an order, that were identified as returnable;
  - After selecting the items, the buyer triggers the return through a feature in the buyer app;
  - Buyer app creates an **/update** request with **update_target** = “item” and **order.items** with the **updated state of the order**, i.e. including only the items & quantity that are not being returned (for part return, this will include one or more items that are not being returned and for full return, the items array will be null);
  - Buyer app signs the auth header for the /update request and sends the request to the seller app endpoint (in case the order was from multiple seller apps, there will be a separate /update request for each seller app);
  - If the Seller App does not accept the update request, they can also return a policy error (code 50002) as defined [here](https://github.com/beckn/protocol-specifications/blob/core-0.9.4-draft/docs/protocol-drafts/BECKN-RFC-005-Error-Codes-Draft-01.md). Otherwise, oOn receiving the **/update** request, the seller app initiates a request for a logistics provider to pick up the order items from the buyer;
  - Seller App selects a logistics provider and confirms the order with the logistics provider.
    - In this case, **Fulfillment.start.location** will have the location for pickup (buyer location) and **Fulfillment.start.instructions** will have instructions for reverse QC for the logistics agent (note that the right to inspect and accept the returnable items is generally discretionary and would have been specified at the time of the original confirmed order, as part of policy terms & conditions in the /on_init);
    - **Fulfillment.end.location** will have the drop location (seller location) and **Fulfillment.end.instructions** will have instructions for the logistics agent for the drop;
  - Seller App returns the updated order and fulfillment details in **/on_update**;

- **Sequence Diagram - Returns**
![image1](https://user-images.githubusercontent.com/95357304/158115367-e9341e88-c6ba-44aa-aa4c-672b1cc2efbe.jpg)

- **Process Flow - Replacements**

  A replacement will follow the following steps:

  - Buyer initiates the replacement from the buyer app. To initiate the replacement, the buyer selects one or more items, from an order, that were identified as replaceable;
  - After selecting the items, the buyer triggers the replacement through a feature in the buyer app;
  - Buyer app creates an **/update** request with **update_target** = “item” and **order.items** with **the updated state of the order**, i.e. including only the items & quantity that are not for replacement (for part replacement, this will include one or more items that are not being replaced and for full replacement, the items array will be null);
  - Buyer app signs the auth header for the /update request and sends the request to the seller app endpoint (in case the order was from multiple seller apps, there will be a separate /update request for each seller app);
  - In case the Seller App does not accept the **/update** request for replacement, they can also return a policy error (code 50002) as specified [here](https://github.com/beckn/protocol-specifications/blob/core-0.9.4-draft/docs/protocol-drafts/BECKN-RFC-005-Error-Codes-Draft-01.md). Otherwise, on receiving the **/update** request, the seller app initiates a request for logistics providers for 2 fulfillments:
    - Pick up the replacement order items from the seller and deliver to buyer;
    - Pick up the order items, to be replaced, from the buyer and deliver to seller;
  - Seller App selects a logistics provider and confirms the order with the logistics provider.
    - For the 1st fulfillment:
    - **Fulfillment.start.location** will have the location for pickup (seller location);
    - **Fulfillment.end.location** will have the drop location (buyer location) and **Fulfillment.end.instructions** will have instructions for the logistics agent for the drop;
  - For the 2nd fulfillment:
    - **Fulfillment.start.location** will have the location for pickup (buyer location) and **Fulfillment.start.instructions** will have instructions for reverse QC for the logistics agent (note that the right to inspect and accept the returnable items is generally discretionary and would have been specified at the time of the original confirmed order, as part of policy terms & conditions in the /on_init);
    - **Fulfillment.end.location** will have the drop location (seller location) and **Fulfillment.end.instructions** will have instructions for the logistics agent for the drop;
  - Seller App returns the updated order and fulfillment details in **/on_update**.

- **Sequence Diagram - Replacements**
![image3](https://user-images.githubusercontent.com/95357304/158115403-3587d307-e777-427a-a0a2-c82b3d4b3d97.jpg)
