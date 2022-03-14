# ONDC: Checklist for Sellers (Under Discussion)

## Overview
Any participant on ONDC becomes a node in the network. A retailer could be onboarded to ONDC as a seller app or as a provider for an aggregator / software vendor, that becomes the seller app on ONDC.

A seller app, on ONDC, publishes a catalog in response to search requests from buyer apps and provides confirmation for each step of the transaction flow, i.e.order creation, fulfilment & post-fulfilment.

An entity that wishes to onboard to ONDC, as a seller app, needs an application that is compliant with Beckn[1] protocol specifications, and could be any of the following:
-   Retail Aggregator – will act as aggregator for multiple retail stores;
    
-   Retail Software Vendor – act as aggregator for multiple retail stores and forward requests from buyers to all the stores while also consolidating the responses from the retail stores back to the buyers;
    
-   Small Retailers – directly onboard to ONDC,using SaaS instances,provided by SaaS vendors;
    
-   Online marketplaces
## Retail Seller Checklist
This checklist is for a retail seller who could either be a seller app on ONDC or a provider for an aggregator which is the seller app for ONDC. This checklist defines what the retail seller needs to do at each step of the transaction flow.
|#| **Transaction flow Step** |**Seller needs to provide**| What does it include?|**Description** 
|--|--|--|--|--|
| 1. |**Discovery**  |**Catalog**| Items, add-ons, offers, fulfilment criteria, payment criteria, validity | 
||||List of stores, mapping of items to stores| Store definition includes name, location (address, GPS coordinates), logo
||||- Items(available in stock)| Item ID, name, description, image, price, additional details (like for mobile phone – RAM, display size, etc.)
||||- Items (pre-order)|Same as above
||||- Related Items| Items in between two items
||||- Recommended Items| Popular/ best seller items
||||- Offers | Description; criteria (based on season, total order value, for specific buyer, from specific seller, 3rd party offer, etc.); validity (for specific Item, Category, time of day, location, etc.); how applying offer affects final quote
||||- Add-ons|
||||- fulfilment criteria| Type (e.g. home delivery,store pickup)|
|||| - payment criteria| Type (cash, online)|
|**2**| **Order** |
||-Order Selection| Order quote for Items, Addons, offers| Break-up of quote + breakup, price of each item, validity of price| Check availability of each item before providing quote for order selection
|| Checkout| **Fulfilment Policy**| Delivery Provider, Start & end location for delivery; Pick-up & drop-off instructions (if applicable); delivery radius; whether own delivery agent or to assign to seller node or use network logistics provider; whether agents available on specific time slots; delivery charges| Check availability of each item at check-out; Additional details (if applicable) e.g. for pharmacy, requires prescription; pre-payment; loyalty program, etc
||| **Payment Policy**| Payment type, Payment provider URI, payment details|  Seller & Seller node should have a settlement protocol; How should cash collected by delivery node be settled with seller node – directly to stores or via the seller node?
||| **Cancellation / Update Policy**| | Under progress – will specify if order can be updated or cancelled before pick-up
|| -Confirm| Confirmed Order| Items / Addons / Offers, Price quote, fulfilment policy, cancellation policy, payment policy
|**3**| **Fulfilment**|
||-Tracking| Status of order| Locator, status| Whether delivery agent provides tracking; Periodic status updates for delivery? Additional details – temperature of delivery agent, whether fully vaccinated?
|**4**| **Post-Fulfilment**|
||-Rating|
||-Support| Contact details of provider| Phone no, email, URL| Phone no for store, customer support for store (if applicable), delivery customer support (if applicable)

*[1]https://github.com/beckn/protocol-specifications/blob/master/core/v0/api/core.yaml*

