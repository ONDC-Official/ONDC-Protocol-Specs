# Returns and Replacements (Draft)



## 1. Overview

The objective of this note is to propose a process flow for returns & replacements in ONDC. Returns & replacements can be processed by Buyer & Seller Apps, using existing protocol APIs. The detailed process flow and sequence diagrams for returns & replacements is defined below.

A confirmed order, in ONDC, can include multiple items. An item, in an order, may be returnable or replaceable. Not all items in an order may be returnable or replaceable. A buyer can only choose to return / replace one or more items that were defined as returnable or replaceable.

This note proposes a detailed process flow for the following:

- **Return** - for one or more items in an order. A part return will be defined as return of one or more items in an order but not return of all items in the order. A full return will be defined as the return of all items in the order.
- **Replacement** - for one or more items in an order. A part replacement will be defined as replacement of one or more items in an order but not replacement of all items in the order. A full replacement will be defined as the replacement of all items in the order. Note that a replacement can also be handled by part or full return of an order and creating a new order with the items that are being partly or fully replaced.

This note does not address the refund issues associated with return. This will be addressed separately in the Payment & Settlement protocol document.



## 2. Initiation of Returns & Replacements

Buyer can initiate a return or replacement from a feature in the Buyer App, from which the order was confirmed. 

Irrespective of how the return is initiated, return & replacement results in an amendment to the order confirmed earlier



## 3. Process Flow - Returns

A return will follow the following steps:

- Buyer initiates the return from the buyer app. To initiate the return, the buyer selects one or more items, from an order, that were identified as returnable;
- After selecting the items, the buyer triggers the return through a feature in the buyer app;
- Buyer app creates an **/update** request with **update_target** = “item” and **order.items** including only the items & quantity for return (for part return, this will include one or more items that are being returned and for full return, the items array will be null);
- Buyer app signs the auth header for the /update request and sends the request to the seller app endpoint (in case the order was from multiple seller apps, there will be a separate /update request for each seller app);
- On receiving the **/update** request, the seller app initiates a request for a logistics provider to pick up the order items from the buyer;
- Seller App selects a logistics provider and confirms the order with the logistics provider.
  - In this case, **Fulfillment.start.location** will have the location for pickup (buyer location) and **Fulfillment.start.instructions** will have instructions for reverse QC for the logistics agent (note that the right to inspect and accept the returnable items is generally discretionary and would have been specified at the time of the original confirmed order, as part of policy terms & conditions in the /on_init);
  - **Fulfillment.end.location** will have the drop location (seller location) and **Fulfillment.end.instructions** will have instructions for the logistics agent for the drop;
- Seller App returns the updated order and fulfillment details in **/on_update**. In case the Seller App does not accept the **/update** request for return, they can also return a policy error (code 50002) as specified [here](https://github.com/beckn/protocol-specifications/blob/core-0.9.4-draft/docs/protocol-drafts/BECKN-RFC-005-Error-Codes-Draft-01.md);



### **Sequence Diagram**


![Visualising ONDC (3)](https://user-images.githubusercontent.com/95357304/152783922-26983c93-5a75-4a24-afe7-a0bd18f89ff5.jpg)



## 4. Process Flow - Replacement

A replacement will follow the following steps:

- Buyer initiates the replacement from the buyer app. To initiate the replacement, the buyer selects one or more items, from an order, that were identified as replaceable;
- After selecting the items, the buyer triggers the replacement through a feature in the buyer app;
- Buyer app creates an **/update** request with **update_target** = “item” and **order.items** including only the items & quantity for replacement (for part replacement, this will include one or more items that are being replaced and for full replacement, the items array will be null);
- Buyer app signs the auth header for the /update request and sends the request to the seller app endpoint (in case the order was from multiple seller apps, there will be a separate /update request for each seller app);
- On receiving the **/update** request, the seller app initiates a request for logistics providers for 2 fulfillments:
  - Pick up the replacement order items from the seller and deliver to buyer;
  - Pick up the order items, to be replaced, from the buyer and deliver to seller;
- Seller App selects a logistics provider and confirms the order with the logistics provider.
  - For the 1st fulfillment:
    - **Fulfillment.start.location** will have the location for pickup (seller location);
    - **Fulfillment.end.location** will have the drop location (buyer location) and 
    - **Fulfillment.end.instructions** will have instructions for the logistics agent for the drop;
  - For the 2nd fulfillment:
    - **Fulfillment.start.location** will have the location for pickup (buyer location) and **Fulfillment.start.instructions** will have instructions for reverse QC for the logistics agent (note that the right to inspect and accept the returnable items is generally discretionary and would have been specified at the time of the original confirmed order, as part of policy terms & conditions in the /on_init);
    - **Fulfillment.end.location** will have the drop location (seller location) and **Fulfillment.end.instructions** will have instructions for the logistics agent for the drop;
- Seller App returns the updated order and fulfillment details in **/on_update**. In case the Seller App does not accept the **/update** request for return, they can also return a policy error (code 50002) as specified [here](https://github.com/beckn/protocol-specifications/blob/core-0.9.4-draft/docs/protocol-drafts/BECKN-RFC-005-Error-Codes-Draft-01.md);



### Sequence Diagram


![Visualising ONDC (4)](https://user-images.githubusercontent.com/95357304/152783949-db3430bb-d7fe-4313-b760-4efff273094e.jpg)




