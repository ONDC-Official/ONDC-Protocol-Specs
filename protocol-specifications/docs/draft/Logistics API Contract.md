# Logistics API Contract

## Revision History

|Version|Date|Changes|
|---|---|:---|
|0.9.3|30th Oct 2022|a. Updated version nos in this revision history to make the final version of this doc (0.93) match with the version of API contract|
|0.9.25|8th Oct 2022|a. Updated order state enums in /on_confirm as per Swagger spec 1.0.14 (July 23rd, 2022)|
|0.9.24|1st Oct 2022|a. Added enums for Order.state|
|0.9.23|24th Sep 2022|a. Fixes for the following to comply with swagger spec:<br>/search - “@ondc/org/payload_details”.weight.value to number;<br>/confirm - “@ondc/org/linked_order”.items.quantity.measure.value to number;<br>/search & /confirm - “@ondc/org/linked_order”.order.weight.value to number;<br>“@ondc/org/payload_details”.dimensions.length.value to number; “@ondc/org/payload_details”.dimensions.breadth.value to number;<br>“@ondc/org/payload_details”.dimensions.height.value to number;|
|0.9.22|24th Jul 2022|a. Adding contract for RTO, return flows (/cancel, /on_cancel, /update, /on_update)<br>b. Support for fulfillments array in Order (changed Order.fulfillment to Order.fulfillments) and necessary changes in contract for /select, /on_select, /init, /on_init, /confirm, /on_confirm, /on_status to enable this<br>c. Changes to /on_status - fulfillment state, delivery confirmation image|
|0.9.21|15th Jul 2022|a. Updated following attrs in /init to comply with specs:<br>@ondc/org/cod-settlement_window,<br>@ondc/org/cod-settlement_details|
|0.9.2|13th Jul 2022|a. Agent optional for hyperlocal in /on_confirm<br>b. Added Order.fulfillment.start.person.name (optional), Order.fulfillment.end.person.name (mandatory) in /confirm<br>c. Updated /on_status|
|0.9.1|11th Jul 2022|a. Fixed syntax error - added “}” to /on_confirm to comply with spec (provider & item at same level)|
|0.9|4th Jul 2022|a. Order payload details mandatory only for inter-city shipments in /search<br>b. provider.locations mandatory only if package has to be dropped off at the logistics provider location<br>c. Contract for status and on_status, track and on_track|
|0.8|3rd Jul 2022|a. Changed items.name and provider.name in “@ondc/org/linked_order” to descriptor.name|
|0.7|19th Jun 2022|a. Added response to comments on this doc<br>b. Modified overview|
|0.6|15th Jun 2022|a. Added order.id to /confirm and /on_confirm|
|0.5|14th Jun 2022|a. Added /on_confirm contracts|
|0.4|13th Jun 2022|a. Added /confirm contracts|
|0.3|12th Jun 2022|a. Added /init and /on_init contracts|
|0.2|7t Jun 2022|a. Updated Contract for Logistics Buyer and Seller|
|0.1|10th May 2022|a. Initial version - harmonization of information between logistics buyer & seller;<br>b. Logistics flow for express & standard delivery|


## 1. Overview

A cascaded transaction in ONDC involves interaction between a retail seller (or buyer app), that also acts as the logistics buyer, and the logistics seller (provider) who are both participants in the network. To ensure seamless handover of the Order from the logistics buyer to the logistics provider and final delivery to the buyer would require harmonisation of information between all of them.<br><br>This note defines the transaction contract between the logistics buyer and logistics seller (provider) and applies to hyperlocal as well as e-commerce logistics.

## 2. Logistics Flows

Logistics Providers for ONDC will support the categories as defined in the protocol definition. Logistics Buyer can specify the type of delivery by specifying the appropriate category.

The logistics flow for RTO is defined below.

1. **RTO**

- Logistics provider will create a fulfilment type of "RTO" for every forward shipment or CoD order in /on_confirm;
- Logistics agent initiates cancellation of order through unsolicited /on_cancel & provides the reason code and AWB no. The cancellation reason code will identify whether the RTO fulfillment should be initiated (list of cancellation reason codes is [here](https://docs.google.com/document/d/1M-lZSduYMFKIk1V6d8QLt-j-16-rVzYVdPn0pmbkclk/edit));
- If the RTO fulfillment is being initiated, the logistics provider also updates the RTO fulfillment start date & time, order.state to "RTO initiated" and fulfilment state to "Order is cancelled";
- After the RTO fulfillment is completed, logistics provider updates the fulfillment end date & time, order.state to "RTO delivered" through /on_status;
- Logistics buyer updates the settlement trail for the logistics provider using Order.payment."@ondc/org/cod-settlement_details", if payment is due to the logistics provider from the logistics buyer based on the reason for cancellation;
- RTO process flow for logistics provider is detailed [here](https://docs.google.com/document/d/1M-lZSduYMFKIk1V6d8QLt-j-16-rVzYVdPn0pmbkclk/edit);

The logistics flows for F&B and Grocery are defined below.

2. **F&B**

Logistics Buyer specifies "@ondc/org/time_to_ship" for Catalog Item using appropriate Item attribute:

- Retail Buyer App shows fulfillment promise based on the above item attribute;
- In /init API, Logistics Buyer (retail seller) searches for " **Immediate Delivery**" option by specifying the following in Search Intent:
  - Intent.category.id = " **Immediate Delivery**";
  - Intent.fulfillment.start.location;
  - Intent."@ondc/org/payload_details" = weight, dimensions of package (optional);
- Logistics Provider responds with ACK & appropriate logistics options (if the location is serviceable) in the /on_search callback;
- Logistics Provider responds with ACK & empty message body (if the location is not serviceable) in the /on_search callback;
- Logistics Provider responds with ACK & appropriate [error codes](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/blob/master/protocol-specifications/docs/draft/Error%20Codes.md) if location is not serviceable and / or agent is not available in the /on_init callback;
- Logistics Buyer confirms the logistics order after receiving /confirm request from retail buyer app:
  - Logistics Provider responds with ACK & provides fulfillment.start.time and fulfillment.end.time (if logistics agent is available) and other information as appropriate, in the /on_confirm callback (as defined in 2 above);
  - Logistics provider responds with ACK & appropriate error codes if logistics agent is not available, in the /on_confirm callback;
  - All F&B orders, will by default be marked as "ready to ship", through the tag Order.fulfillment.tags ("@ondc/org/ready_to_ship: "Yes"), the order will be picked up by the logistics provider;

3. **Grocery**

Logistics Buyer specifies "@ondc/org/time_to_ship" for Catalog Item using appropriate Item attribute:

- Retail Buyer App shows fulfillment promise based on the above item attribute;
- In **/select** API, Logistics Buyer searches for " **Same Day Delivery **" or "**Next Day Delivery**" option by specifying the following in Search Intent:
  - Intent.category.id = " **Same Day Delivery**" or " **Next Day Delivery**";
  - Intent.fulfillment.start.location;
  - Intent."@ondc/org/payload_details" = weight, dimensions of package (optional);
- Logistics Provider responds with ACK & appropriate logistics options (if the location is serviceable), in the /on_search callback;
- Logistics Provider responds with ACK & empty message body (if the location is not serviceable) in the /on_search callback;
- Logistics Provider responds with ACK & appropriate [error codes](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/blob/master/protocol-specifications/docs/draft/Error%20Codes.md) if location is not serviceable and / or agent is not available, in the /on\_init callback;
- Logistics Buyer confirms the logistics order after receiving /confirm request from retail buyer app and specifies the following:
  - Order.fulfillment.start.time (based on retail order timestamp as received from buyer app + time to ship) - this is only an estimate;
  - Other information as appropriate (as defined in 2 above)
- Logistics Provider responds with ACK & fulfillment.end.time and other information as appropriate (as defined in 2 above), in the /on_confirm callback;
- Logistics provider responds with ACK & appropriate error codes if fulfillment.start.time is not acceptable to them, in the /on_confirm callback;
- When Logistics Buyer marks the order as "ready to ship" through the tag Order.fulfillment tags ("@ondc/org/ready_to_ship - pass "Yes"), the order will be picked up by the logistics provider;

## 3. /search payload (for serviceability check, rate calculation) - mandatory attributes listed below with sample values
```
{
   "context":
   {
       "domain": "nic2004:60232",
       "country": "IND",
       "city": "std:080",
       "action": "search",
       "core_version": "0.9.3",
       "bap_id": "ondc.gofrugal.com/ondc/18275",
       "bap_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
       "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
       "message_id": "1651742565654",
       "timestamp": "2022-06-13T07:22:45.363Z",
       "ttl": "PT30S"
   },
   "message":
   {
       "intent" :
       {
           "category":
           {
                 "id" : "Immediate Delivery"
           },
           "fulfillment":
           {
               "type" : “CoD”,
               "start" :
  {
                   "location" :
      {
                       "gps" : "12.4535445,77.9283792",
	          “address”:
	          {
		 “area_code”: “560041”
	          }
                   },
               },
               "end" : {
                   "location" : {
                       "gps" : "12.4535445,77.9283792",
	          “address”:
	          {
		 “area_code”: “560001”
	          }
                   }
               }
           },
           "payment":
           {
	  “@ondc/org/collection_amount” : “30000”
           },
           "@ondc/org/payload_details":
           {
                   "weight" :
                   {
                       "unit" : "Kilogram",
                       "value" : 10
                   },
                   "dimensions" :
                   {
          "length" :
          {
               "unit" : "meter",
               "value" : 1
          },
          "breadth" :
          {
               "unit" : "meter",
               "value" : 1
          },
          "height" :
          {
               "unit" : "meter",
               "value" : 1
          },
                   },
                   "category" : “Mobile Phone”,
                   "value" :
                   {
                      “currency”: “INR”,
                      “value”: “50000"
                   }
           }
       }
   }
}
```

## 4. /on_search payload - mandatory attributes listed below with sample values
```
{
   "context":
   {
       "domain": "nic2004:60232",
       "country": "IND",
       "city": "std:080",
       "action": "on_search",
       "core_version": "0.9.3",
       "bap_id": "ondc.gofrugal.com/ondc/18275",
       "bap_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
       "bpp_id": "shiprocket.com/ondc/18275",
       "bpp_uri": "https://shiprocket.com/ondc",
       "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
       "message_id": "1651742565654",
       "timestamp": "2022-06-13T07:22:46.363Z",
       "ttl": "PT30S"
   },
   "message":
    {
       "Catalog":
       {
            "bpp/descriptor":
            {
                "name": "Shiprocket Inc",
            },
            "bpp/providers":
            [
                {
                    "id": "18275-Provider-1",
                    "descriptor":
                    {
                        "name": "Loadshare",
                        "short_desc": "Loadshare",
                        "long_desc": "Loadshare"
                    },
                   "categories":
                   [
	         {
                        “id”: “Immediate Delivery”,
                        “time”:
                        {
                            "duration": "PT45M"
                        }
	         }
                   ],
                   "locations":
                    [
                        {
                            "id": "18275-Location-1",
                            "gps": "12.967555,77.749666",
                            "address":
                            {
                              "street": "Jayanagar 4th Block",
                              "city": "Bengaluru",
                              "area_code": "560076",
                              "state": "KA"
                            }
                        }
                    ],
                    "items":
                    [
                        {
                            "id": "18275-Item-1",
                            "category_id": "Immediate Delivery",
                            "descriptor":
                            {
                                "name": "Immediate Delivery",
         	      "short_desc": "Immediate Delivery for F&B",
                        	      "long_desc": "Immediate Delivery for F&B",
                            },
                            "price":
                            {
                                "currency": "INR",
                                "value": "50.0"
                            }
                    ]
                }
            ]
       }
    }
}

```

## 5. /init payload - mandatory attributes listed below with sample values
```
{
  "context":
  {
      "domain": "nic2004:60232",
      "country": "IND",
      "city": "std:080",
      "action": "init",
      "core_version": "0.9.3",
      "bap_id": "ondc.gofrugal.com/ondc/18275",
      "bap_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
      "bpp_id": "shiprocket.com/ondc/18275",
      "bpp_uri": "https://shiprocket.com/ondc",
      "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
      "message_id": "1651742565655",
      "timestamp": "2022-06-13T07:22:47.363Z",
       "ttl": "PT30S"
  },
  "message":
  {
    "order": 
    {
      "provider":
      {
        "id": "18275-Provider-1",
        “locations”:
        [
          {
            “id”: “18275-Location-1”
          }
        ]
      },
      "items":
      [
        {
          "id": "18275-Item-1",
          "category_id": "Immediate Delivery"
        }
      ],
      "fulfillments":
      [
      	{
     “id”: “Fulfillment1”,
     "type": "CoD",
     "tracking": true,
     "start":
     {
          "location":
          {
            "gps": "12.4535445,77.9283792"
            "address":
            {
              "name": "Fritoburger",
              "building": "12 Restaurant Tower",
              "locality": "JP Nagar 24th Main",
              "city": "Bengaluru",
              "state": "Karnataka",
              "country": "India",
              "area_code": "560041"
            }
          }
           "contact":
           {
             "phone": "98860 98860",
             "email": "abcd.efgh@gmail.com"
           }
     },
     "end":
     {
          "location":
          {
            "gps": "12.4535445,77.9283792",
            "address":
            {
              "name": "D 000",
              "building": "Prestige Towers",
              "locality": "Bannerghatta Road",
              "city": "Bengaluru",
              "state": "Karnataka",
              "country": "India",
              "area_code": "560076"
            }
          }
          "contact":
          {
            "phone": "98860 98860",
            "email": "abcd.efgh@gmail.com"
          }
     }
}
      ],
      "billing":
      {
        "name": "XXXX YYYYY",
        "address":
        {
          "name": "D000, Prestige Towers",
          "locality": "Bannerghatta Road",
          "city": "Bengaluru",
          "state": "Karnataka",
          "country": "India",
          "area_code": "560076"
        }
        "phone": "98860 98860",
        "email": "abcd.efgh@gmail.com"
      },
      "payment":
      {
        "type": "POST-FULFILLMENT",
        "@ondc/org/cod-settlement_window": "P2D",
        "@ondc/org/cod-settlement_details":
        [
          {
            "settlement_counterparty": "buyer-app",
            "settlement_type": "upi",
            "upi_address": "gft@oksbi",
            "settlement_bank_account_no": "XXXXXXXXXX",
            "settlement_ifsc_code": "XXXXXXXXX"
          }
        ]
      }
    }
  }
}
```

## 6. /on_init payload - mandatory attributes listed below with sample values
```
{
   "context":
   {
       "domain": "nic2004:60232",
       "country": "IND",
       "city": "std:080",
       "action": "on_init",
       "core_version": "0.9.3",
       "bap_id": "ondc.gofrugal.com/ondc/18275",
       "bap_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
       "bpp_id": "shiprocket.com/ondc/18275",
       "bpp_uri": "https://shiprocket.com/ondc",
       "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
       "message_id": "1651742565655",
       "timestamp": "2022-06-13T07:22:48.363Z",
       "ttl": "PT30S"
   },
   "message":
   {
     "order": 
     {
       "provider":
       {
         "id": "18275-Provider-1"
       },
       “provider_location”:
       {
         “id”: “18275-Location-1”
       },
       "items":
       [
         {
           "id": "18275-Item-1"
         }
       ],
       "quote":
       {
        "price":
        {
          "currency": "INR",
          "value": "7.0"
        },
        "breakup":
        [
          {
            “@ondc/org/item_id”: “18275-Item-1”,
            “@ondc/org/title_type”: “Delivery Charge”,
            "price":
            {
              "currency": "INR",
              "value": "5.0"
            }
          },
          {
            "title": "RTO charges",
            “@ondc/org/title_type”: “RTO Charge”,
            "price":
            {
              "currency": "INR",
              "value": "2.0"
            }
          }
        ]
      }
    }
  }
}
```

**Validations**
- quote.price.value = Σ (quote.breakup.price.value);
- Quote.breakup should have 1 entry for each "@ondc/org/item-id";
- If any of the above validations or mandatory checks in (d) above fail, buyer app will return "NACK";

## 7. /confirm payload (for manifest creation) - mandatory attributes listed below with sample values
```
{
  "context":
  {
      "domain": "nic2004:60232",
      "country": "IND",
      "city": "std:080",
      "action": "confirm",
      "core_version": "0.9.3",
      "bap_id": "ondc.gofrugal.com/ondc/18275",
      "bap_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
      "bpp_id": "shiprocket.com/ondc/18275",
      "bpp_uri": "https://shiprocket.com/ondc",
      "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
      "message_id": "1651742565656",
      "timestamp": "2022-06-13T07:22:49.363Z",
      "ttl": "PT30S"
  },
  "message":
  {
    "order": 
    {
     “id”: "0799f385-5043-4848-8433-4643ad511a14",
      "provider":
      {
        "id": "18275-Provider-1",
        “locations”:
        [
          {
            “id”: “18275-Location-1”
          }
        ]
      },
      "items":
      [
          {
            "id": "18275-Item-1",
            "category_id": "Same Day Delivery"
          }
      ],
      "fulfillments":
      [
      	{
        “id”: “Fulfillment1”,
        "type": "CoD",
        "@ondc/org/awb_no": "1227262193237777",
        "tracking": false,
        "start":
        {
          "person":
          {
            "name": "Ramu"
          },
          "location":
          {
            "gps": "12.4535445,77.9283792",
            "address":
            {
              "name": "Fritoburger",
              "building": "12 Restaurant Tower",
              "locality": "JP Nagar 24th Main",
              "city": "Bengaluru",
              "state": "Karnataka",
              "country": "India",
              "area_code": "560041"
            }
          }
          "contact":
          {
            "phone": "98860 98860",
            "email": "abcd.efgh@gmail.com"
          }
          "instructions":
          {
            "short_desc": "XYZ1",
            "long_desc": "QR code will be attached to package",
            "images"
            [
              "URL or data string as per spec"
            ]
          }
        },
        "end":
        {
          "person":
          {
            "name": "Ramu"
          },
          "location":
          {
            "gps": "12.4535445,77.9283792",
            "address":
            {
              "name": "D 000",
              "building": "Prestige Towers",
              "locality": "Bannerghatta Road",
              "city": "Bengaluru",
              "state": "Karnataka",
              "country": "India",
              "area_code": "560076"
            }
          },
          "contact":
          {
            "phone": "98860 98860",
            "email": "abcd.efgh@gmail.com"
          },
          "instructions":
          {
            "short_desc": "XYZ2",
            "long_desc": "drop package at security gate"
          }
        }
        "tags":
         "@ondc/org/order_ready_to_ship": “Yes”
      }
      ],
      "billing":
      {
        "tax_number": "29AAACU1901H1ZK"
      },
      "payment":
      {
        "@ondc/org/collection_amount": "30000"
      }
      "@ondc/org/linked_order"
      {
        "items":
        [
          "category_id": "Grocery",
          "descriptor":
          {
             “name”: "Atta"
          },
          "quantity":
          {
            "count": "2"
            "measure":
            {
              "unit": "Kilogram",
              "value": 5
            }
          },
          "price":
          {
            "currency": "INR",
            "value": "300"
          },
        ],
        "provider":
        {
          “descriptor”:
          {
            "name": "Aadishwar Store"
          },
          "address":
          {
            "name": "KHB Towers",
            "street": "6th Block",
            "locality": "Koramangala",
            "city": "Bengaluru",
            "state": "Karnataka",
            "area_code": "560070"
          }
        },
        "order":
        {
          "id": "ABCDEFGH",
          "weight":
          {
            "unit": "Kilogram",
            "value": 10
          }
        }
      }
    }
  }
}
```

## 8. /on_confirm payload (for returning the fulfillment slot, agent details, vehicle details, E-way bill sr no & EBN expiry date, as applicable, for inter-state shipments) - mandatory attributes listed below with sample values**
```
{
  "context":
  {
      "domain": "nic2004:60232",
      "country": "IND",
      "city": "std:080",
      "action": "on_confirm",
      "core_version": "0.9.3",
      "bap_id": "ondc.gofrugal.com/ondc/18275",
      "bap_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
      "bpp_id": "shiprocket.com/ondc/18275",
      "bpp_uri": "https://shiprocket.com/ondc",
      "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
      "message_id": "1651742565656",
      "timestamp": "2022-06-13T07:22:50.363Z",
      "ttl": "PT30S"
  },
  "message":
  {
    "order":
    {
      “id”: "0799f385-5043-4848-8433-4643ad511a14",
      “state”: "Created",
      "provider":
      {
        "id": "18275-Provider-1",
     
        "locations":
        [
          {
            "id": "18275-Location-1",
          }
        ]
      },
      "items":
      [
        {
          "id": "18275-Item-1",
          "category_id": "Same Day Delivery"
        }
      ],
      "fulfillments":
      [
        {
          “id”: “Fulfillment1”,
          "type": "CoD",
          "@ondc/org/awb_no": "1227262193237777",
          "tracking": false,
          "start":
          {
            "time":
            {
              "range":
              {
                "start": "2022-06-14T10:00:00.000Z",
                "end": "2022-06-14T10:30:00.000Z"
              }
            }
          },
          "end":
          {
            "time":
            {
              "range":
              {
                "start": "2022-06-14T12:00:00.000Z",
                "end": "2022-06-14T12:30:00.000Z"
              }
            }
          },
          "agent":
          {
            "name": "Ramu",
            "phone": "9886098860"
          },
          "vehicle":
          {
            "category": "mini-truck",
            "size": "small",
            "registration": "2020"
          },
          "@ondc/org/ewaybillno": "EBN1",
          "@ondc/org/ebnexpirydate": "2022-06-30T12:00:00.000Z"
        },
        {
          “id”: “Fulfillment1-RTO”,
          "type": "RTO",
          "start":
          {
            "time":
            {
              "range":
              {
                "start": "2022-06-14T10:00:00.000Z",
                "end": "2022-06-14T10:30:00.000Z"
              }
            }
          },
          "agent":
          {
            "name": "Ramu",
            "phone": "9886098860"
          }
        }
      ]
    }
  }
}
```

## 9. /status payload (for getting the status of logistics order) - mandatory attributes listed below with sample values
```
{
      "context":
      {
          "domain": "nic2004:60232",
          "country": "IND",
          "city": "std:080",
          "action": "status",
          "core_version": "0.9.3",
          "bap_id": "ondc.gofrugal.com/ondc/18275",
          "bap_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
          "bpp_id": "shiprocket.com/ondc/18275",
          "bpp_uri": "https://shiprocket.com/ondc",
          "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
          "message_id": "1651742565657",
          "timestamp": "2022-06-13T07:22:51.363Z"
      },
      "message":
      {
         "order_id": "a912e-244c-4954-b74d-5be678940ec4_87"
      }
}

```
**Note**: transaction_id has to be the same as in /confirm (for the order)

## 10. /on_status payload (for returning the status of logistics order) - mandatory attributes listed below with sample values)
```
  {
    "context": 
    {
      "domain": "nic2004:60232",
      "country": "IND",
      "city": "std:080",
      "action": "on_status",
      "core_version": "0.9.3",
      "bap_id": "ondc.gofrugal.com/ondc/18275",
      "bap_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
      "bpp_id": "shiprocket.com/ondc/18275",
      "bpp_uri": "https://shiprocket.com/ondc",
      "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
      "message_id": "1651742565657",
      "timestamp": "2022-06-13T07:22:52.363Z",
      "ttl": "PT30S"
    },
    "message":
    {
      "order":
      {
        “id”: "0799f385-5043-4848-8433-4643ad511a14",
            “state”: "Shipped",
        "provider":
            {
          "id": "18275-Provider-1",
          "items":
          [
            {
                  "id": "18275-Item-1",
      "category_id": "Same Day Delivery"
                    }
                ],
          "locations":
                [
                    {
              "id": "18275-Location-1",
                    }
                ]
          },
           "fulfillments":
           [
           {
              “type": "CoD",
          "@ondc/org/awb_no": "1227262193237777",
          “state”:
          {
            “descriptor”:
            {
        “code”: “Agent reached pickup”
            }
          },
          "tracking": false,
          "start":
                  {
            "time":
                      {
              "range":
                        {
                              "start": "2022-06-14T10:00:00.000Z",
                  "end": "2022-06-14T10:30:00.000Z"
                        }
                      }
                  },
          "end":
          {
            "time":
            {
              "range":
              {
                "start": "2022-06-14T12:00:00.000Z",
                "end": "2022-06-14T12:30:00.000Z"
                          }
            },
            "instructions":
            {
              "images":
              [
                  "https://abc.com/images/18275/18275_ONDC_1650967420207.png"
              ]
            }
          },
          "agent":
          {
            "name": "Ramu",
            "phone": "9886098860"
          },
          "vehicle":
          {
            "category": "mini-truck",
            "size": "small",
            "registration": "2020"
          },
          "@ondc/org/ewaybillno": "EBN1",
          "@ondc/org/ebnexpirydate": "2022-06-30T12:00:00.000Z"
       }
           ]
      }
    }
  }
```

## 11. /update payload (mandatory attributes listed below with sample values)
```
{
  "context":
  {
    "domain": "nic2004:60232",
    "country": "IND",
    "city": "std:080",
    "action": "update",
    "core_version": "0.9.3",
    "bap_id": "ondc.gofrugal.com/ondc/18275",
    "bap_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
    "bpp_id": "shiprocket.com/ondc/18275",
    "bpp_uri": "https://shiprocket.com/ondc",
    "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
    "message_id": "1651742565658",
    "timestamp": "2022-06-13T07:22:53.363Z",
    "ttl": "PT30S"
  },
  "message":
  {
    "update_target": "fulfillment",
    "order":
    {
      "id": "0799f385-5043-4848-8433-4643ad511a14",
      “state”: “Created”,
      "fulfillments":
      [
        {
          “id”: “Fulfillment1”,
          "type": "Prepaid",
          "tags":
          {
            "@ondc/org/order_ready_to_ship": "yes"
          }
        }
      ]
    }
  }
}
```

## 12. /on_update payload
```
{
  "context":
  {
    "domain": "nic2004:60232",
    "country": "IND",
    "city": "std:080",
    "action": "on_update",
    "core_version": "0.9.3",
    "bap_id": "ondc.gofrugal.com/ondc/18275",
    "bap_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
    "bpp_id": "shiprocket.com/ondc/18275",
    "bpp_uri": "https://shiprocket.com/ondc",
    "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
    "message_id": "1651742565658",
    "timestamp": "2022-06-13T07:22:54.363Z",
    "ttl": "PT30S"
  },
  "message":
  {
    "order":
    {
      "id": "0799f385-5043-4848-8433-4643ad511a14",
      “state”: “Created”,
      "fulfillments":
      [
        {
          “id”: “Fulfillment1”,
          "type": "Prepaid",
          "start":
          {
            "time":
            {
              "range":
              {
                "start": "2022-06-14T10:00:00.000Z",
                "end": "2022-06-14T10:30:00.000Z"
              }
            }
          },
          "agent":
          {
            "name": "Ramu",
            "phone": "9886098860"
          }
        }
      ]
    }
  }
}
```

## 13. /cancel payload (mandatory attributes listed below with sample values (note: cancellation reason codes are defined [here](https://docs.google.com/document/d/1M-lZSduYMFKIk1V6d8QLt-j-16-rVzYVdPn0pmbkclk/edit))
```
{
  "context":
  {
    "domain": "nic2004:60232",
    "country": "IND",
    "city": "std:080",
    "action": "cancel",
    "core_version": "0.9.3",
    "bap_id": "ondc.gofrugal.com/ondc/18275",
    "bap_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
    "bpp_id": "shiprocket.com/ondc/18275",
    "bpp_uri": "https://shiprocket.com/ondc",
    "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
    "message_id": "1651742565659",
    "timestamp": "2022-06-13T07:22:55.363Z",
    "ttl": "PT30S"
  },
  "message":
  {
    "order_id": "a912e-244c-4954-b74d-5be678940ec4_87",
    "cancellation_reason_id": "004"
  }
}
```
## 14. /on_cancel payload (mandatory attributes listed below with sample values (note: cancellation flow & reason codes are defined [here](https://docs.google.com/document/d/1M-lZSduYMFKIk1V6d8QLt-j-16-rVzYVdPn0pmbkclk/edit))
```
{
  "context":
  {
    "domain": "nic2004:60232",
    "country": "IND",
    "city": "std:080",
    "action": "on_cancel",
    "core_version": "0.9.3",
    "bap_id": "ondc.gofrugal.com/ondc/18275",
    "bap_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
    "bpp_id": "shiprocket.com/ondc/18275",
    "bpp_uri": "https://shiprocket.com/ondc",
    "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
    "message_id": "1651742565659",
    "timestamp": "2022-06-13T07:22:56.363Z",
    "ttl": "PT30S"
  },
  "message":
  {
    "order":
    {
      “id”: "0799f385-5043-4848-8433-4643ad511a14",
      “state”: "RTO initiated",
      "fulfillments":
      [
        {
          “id”: “Fulfillment1-RTO”,
          "type": "RTO",
          "start":
          {
            "time":
            {
              "range":
              {
                "start": "2022-06-14T10:00:00.000Z",
                "end": "2022-06-14T10:30:00.000Z"
              }
            }
          },
          "agent":
          {
            "name": "Ramu",
            "phone": "9886098860"
          },
          "tags":
          {
            "cancellation_reason_id": "013",
            "AWB no": "1227262193237777"
          }
        }
      ]
    }
  }
}
```
