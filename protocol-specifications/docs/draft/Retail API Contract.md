# Retail API Contract

## Revision History

|Version|Date|Changes|
|---|---|:---|
|0.9.3|30th Oct 2022|a. Updated version nos in this revision history to make the final version of this doc (0.9.3) match with the version of API contract;|
|0.9.26|8th Oct 2022|a. Updated order.state to title case, “Delivered” is either “Completed” or “Delivered”;|
|0.9.25|1st Oct 2022|a. Added keys for “veg”, “non-veg” in /on_search;<br>b. Added enums for order.state;|
|0.9.24|23rd Sep 2022|a. Added items.fulfillment_id, fulfillment.id in /init, /on_init, /confirm, /on_confirm, /on_status;<br>b. Changed “@ondc/org/buyer_app_finder_fee_amount” value to string in /search, /on_init, /confirm, /on_confirm, /on_status to comply with spec;<br>c. Added /on_cancel API contract;<br>d. Removed the following keys as they’re not used - “@ondc/org/withholding_amount”, “@ondc/org/return_window”, “@ondc/org/settlement_basis”, “@ondc/org/settlement_window” from /on_init, /confirm, /on_confirm, /on_status;|
|0.9.23|23rd Jul 2022|a. Support for fulfillments array in Order (changed Order.fulfillment to Order.fulfillments). This requires changes in contract for /select, /on_select, /init, /on_init, /confirm, /on_confirm, /on_status changed|
|0.9.22|22nd Jul 2022|a. Added Order.items.price to /select and /on_select|
|0.9.21|15th Jul 2022|a. Linked generic error handling capability spec to /on_select, /on_init, /on_confirm|
|0.9.2|14th Jul 2022|a. Updated ONDC namespaced attributes<br>@ondc/org/ondc-return_window to @ondc/org/return_window,@ondc/org/ondc-withholding_amount to @ondc/org/withholding_amount, @ondc/org/ondc-settlement_basis to @ondc/org/settlement_basis, @ondc/org/ondc-settlement_details to @ondc/org/settlement_details to comply with the spec|
|0.9.1|14th Jul 2022|a. email optional in Order.fulfillment.end.contact and Order.fulfillment.start.contact in /confirm and /on_confirm|
|0.9.0|10th Jul 2022|a. Updated contracts for /search, /init, /confirm, /status, /track, /cancel|
|0.8|1st Jul 2022|a. Updated /select contract - removed provider_location and added provider.locations|
|0.7|15th Jun 2022|a. Added /search contract|
|0.6|11th Jun 2022|a. Added Fulfillment.”@ondc/org/provider_name” to on_select|
|0.5|9th Jun 2022|a. Removed fulfillment struct from on_search;<br>b. Added select contract;<br>c .Modified on_select contract to remove fulfillment start & end times and add TAT;<br>d. Removed fulfillment.start.location.gps and fulfillment.start.location.address.area_code;|
|0.4|7th Jul 2022|a. Added ttl to order.quote for on_select;|
|0.3|29th May 2022|a. Updated catalog mandatory requirements:<br>bpp/providers.Provider.<br>ttl is mandatory;<br>items.”@ondc/org/time_to_ship” is mandatory only for category_id “F&B”;<br>items.descriptor.images is optional for category_id “F&B”;<br>items.”@ondc/org/return_window” is mandatory only if items.”@ondc/org/returnable” is “true”;<br>items.related, items.recommended not mandatory;<br>fulfillments.id, fulfillments.type is mandatory;<br>locations.area_code is mandatory;<br>b. Added on_select contract;|
|0.2|24th May 2022|a. Updated “@ondc/org/item-id” to “@ondc/org/item_id” and “@ondc/org/item-qty” to “@ondc/org/item_quantity” in quote.breakup in on_init, on_confirm, on_status|
|0.1|20th May 2022|a. Contract for the following APIs - on_search, on_init, on_confirm, on_status|

## 1. SwaggerHub Links
Updated link is [here](https://app.swaggerhub.com/organizations/ONDC)

## 2. Catalog mandatory requirements
Mandatory requirements for catalog for ONDC buyer & seller apps is defined [here](https://docs.google.com/spreadsheets/d/1BNPOgcJzKglZzj1bpx-KkjvWBpH-R50AXbdC1AKJm1g/edit#gid=175789867)

## 3. /search payload
- a. Search by city - Mandatory fields listed below with sample values
```
{
  "context":
  {
    "domain":"nic2004:52110",
    "action":"search",
    "country":"IND",
    "city":"std:011",
    "core_version":"0.9.3",
    "bap_id":"ondc.paytm.com",
    "bap_uri":"https://ondc-staging.paytm.com/retail",
    "transaction_id":"3df395a9-c196-4678-a4d1-5eaf4f7df8dc",
    "message_id":"1655281254861",
    "timestamp":"2022-06-15T08:20:54.861Z",
    “ttl”: “PT30S”
  },
  "message":
  {
    "intent":
    {
      "fulfillment":
      {
        "type":"Delivery"
      },
      "payment":
      {
        "@ondc/org/buyer_app_finder_fee_type":"percent",
        "@ondc/org/buyer_app_finder_fee_amount":”3”
      }
    }
  }
}
```

## 4. /on_search payload
- a.	Mandatory fields listed below with sample values
```{
    "context":
    {
        "domain": "nic2004:52110",
        "country": "IND",
        "city": "std:011",
        "action": "on_search",
        "core_version": "0.9.3",
        "bap_id": "ondc.paytm.com",
        "bap_uri": "https://ondc.paytm.com/retail",
        "bpp_id": "abc.com/ondc/18275",
        "bpp_uri": "https://abc.com/ondc/seller",
        "transaction_id": "3df395a9-c196-4678-a4d1-5eaf4f7df8dc",
        "message_id": "1655281254861",
        "timestamp":"2022-06-15T08:20:55.861Z",
        "ttl":"PT30S"
    },
    "message":
    {
        "catalog":
        {
            "bpp/descriptor":
            {
                "name": "ABC store",
                "symbol": "https://abc.com/images/18275/18275-1-shop-img",
                "short_desc": "Online eCommerce Store",
                "long_desc": "Online eCommerce Store",
                "images":
                [
                    "https://abc.com/images/18275/18275-1-shop-img"
                ]
            },
            "bpp/providers":
            [
                {
                    "id": "18275-ONDC-1",
                    "descriptor":
                    {
                        "name": "ABC store",
                        "symbol": "https://abc.com/images/18275/18275-1-shop-img",
                        "short_desc": "ABC store",
                        "long_desc": "ABC store_",
                        "images":
                        [
                            "https://abc.com/images/18275/18275-1-shop-img"
                        ]
                    },
                    "@ondc/org/fssai_license_no": "ABCDEFGH",
                    "ttl": "P1D",
                    "locations":
                    [
                        {
                            "id": "abc-store-location-id-1",
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
                            "id": "18275-ONDC-1-1",
                            "descriptor":
                            {
                                "name": "CLINIC PLUS SHAMPOO 100 ML",
                    "symbol": "https://abc.com/images/18275/18275_ONDC_1650967420207.png",
             	       "short_desc": "CLINIC PLUS SHAMPOO 100 ML",
                        	       "long_desc": "CLINIC PLUS SHAMPOO 100 ML",
                                "images":
                                [
                                    "https://abc.com/images/18275/18275_ONDC_1650967420207.png"
                                ]
                            },
                            "price":
                            {
                                "currency": "INR",
                                "value": "170.0",
                                "maximum_value": "180.0"
                            },
                            "category_id": "Packaged Commodity",
                            "fulfillment_id": "1",
                            "location_id": "abc-store-location-id-1",
                            "@ondc/org/returnable": "true",
                            "@ondc/org/cancellable": "true",
                            "@ondc/org/return_window": "P7D",
                            "@ondc/org/seller_pickup_return": "false",
                            "@ondc/org/time_to_ship": "PT45M",
                            "@ondc/org/available_on_cod": "false",
                            "@ondc/org/contact_details_consumer_care": "Ramesh, Koramangala, Bengaluru, ramesh@abc.com, 9876543210",
                            "@ondc/org/statutory_reqs_packaged_commodities":
                            {
                                "manufacturer_or_packer_name": "ABC company",
                                "manufacturer_or_packer_address": "123, xyz street, Bengaluru",
                                "common_or_generic_name_of_commodity": "Clinic Plus Shampoo",
                                "net_quantity_or_measure_of_commodity_in_pkg": "100",
                                "month_year_of_manufacture_packing_import": "08/2022"
                            },
                            "@ondc/org/statutory_reqs_prepackaged_food":
                            {
                                "nutritional_info": "Energy(KCal)-(per 100kg) 420, (per serving 50g)250; Protein(g)-(per 100kg) 12, (per serving 50g) 6",
                                "additives_info": "Preservatives, Artificial Colours"
                            },
                            "@ondc/org/mandatory_reqs_veggies_fruits":
                            {
                                "net_quantity": "100g"
                            }
                            "tags":
                            {
                                "veg": "yes",
                                "non_veg": "no"
                            }
                         }
                    ]
                }
            ]
        }
    }
}
```

- b.	Validations
  - “bpp/providers”.”@ondc/org/fssai_license_no” is mandatory for category_id “F&B”;
  - items.images is optional for category_id “F&B”;
  - items.”@ondc/org/time_to_ship” is mandatory;
  - items.”@ondc/org/return_window” is mandatory only if items.”@ondc/org/returnable” is “true”;
  - Following are mandatory only for category_id “Packaged Commodities” (this can also be available through the back image of the product, in item.descriptor.images, which meets all statutory requirements): items.”@ondc/org/statutory_reqs_packaged_commodities”.
    - manufacturer_or_packer_name
    - manufacturer_or_packer_address
    - common_or_generic_name_of_commodity
    - net_quantity_or_measure_of_commodity_in_pkg
    - Month_year_of_manufacture_packing_import
  - Following are mandatory only for category_id “Packaged Foods” (this can also be available through the back image of the product, in item.descriptor.images, which meets all statutory requirements): items.”@ondc/org/statutory_reqs_prepackaged_food”.
    - nutritional_info
    - additives_info
  - Following are mandatory only for category_id “Fruits and Vegetables”: items.”@ondc/org/mandatory_reqs_veggies_fruits”.
    - net_quantity

## 5. /select payload
- a. Mandatory fields listed below with sample values
```
{
    "context":
    {
        "domain": "nic2004:52110",
        "action": "select",
        "core_version": "0.9.3",
        "bap_id": "ondc.paytm.com",
        "bap_uri": "https://ondc.paytm.com/retail/on_select",
        "bpp_id": "ondc.gofrugal.com/ondc/18275",
        "bpp_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
        "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
        "message_id": "1652092268191",
        "city": "std:080",
        "country": "IND",
        "timestamp": "2022-05-09T10:31:08.201Z",
        "ttl": "PT30S"
    },
    "message":
    {
        "order":
        {
            "provider":
                {
                "id": "18275-ONDC-1-11094",
                "locations":
                [
                   {
                
                "id": "18275-ONDC-1-11094"
                   }
                ]
            },
            "items":
            [
                {
                    "id": "18275-ONDC-1-9",
                    “location_id”: “abc-store-location-id-1",
                    "quantity":
                    {
                        "count": 1
                    }
                }
            ],
            "fulfillments":
            [
                {
                    "end":
                    {
                        "location" :
                        {
                          "gps" : "12.4535445,77.9283792",
                          “address”:
                          {
                            “area_code”: “560001”
        	          	    }
                        }
                    }
                }
            ]
        }
    }
}
```

## 6. /on_select payload
- a.	Mandatory fields listed below with sample values
```
{
    "context": {
        "domain": "nic2004:52110",
        "action": "on_select",
        "core_version": "0.9.3",
        "bap_id": "ondc.paytm.com",
        "bap_uri": "https://ondc.paytm.com/retail/on_select",
        "bpp_id": "ondc.gofrugal.com/ondc/18275",
        "bpp_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
        "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
        "message_id": "1652092268191",
        "city": "std:080",
        "country": "IND",
        "timestamp": "2022-05-09T10:32:08.201Z",
        "ttl": "PT30S"
    },
    "message": {
        "order": {
            "provider": {
                "id": "18275-ONDC-1-11094"
            },
            "items":
            [{
                    "fulfillment_id": "Fulfilment1",
                    "id": "18275-ONDC-1-9",
                }
            ],
            "fulfillments":
            [{
                    “id”: “Fulfillment1”,
                    “ @ ondc / org / provider_name”: “Loadshare”,
                    "tracking": false,
                    “ @ ondc / org / category”: ”Immediate Delivery”,
                    “ @ ondc / org / TAT”: ”PT45M”,
                    “state”: {
                        “descriptor”: {
                            “name”: “Serviceable ”
                        }
                    }
                }
            ],
            "quote": {
                "price": {
                    "currency": "INR",
                    "value": "6.9"
                },
                "breakup":
                [{
                        “ @ ondc / org / item_id”: “18275 - ONDC - 1 - 9”,
                        “ @ ondc / org / item_quantity”: {
                            "count": 1
                        }
                        "title": "SENSODYNE SENSITIVE TOOTH BRUSH",
                        “ @ ondc / org / title_type”: “item”,
                        "price": {
                            "currency": "INR",
                            "value": "5.0"
                        }
                    }, {
                        “ @ ondc / org / item_id”: “Fulfillment1”,
                        "title": "Delivery charges",
                        “ @ ondc / org / title_type”: “delivery”,
                        "price": {
                            "currency": "INR",
                            "value": "0.5"
                        }
                    }, {
                        “ @ ondc / org / item_id”: “Fulfillment1”,
                        "title": "Packing charges",
                        “ @ ondc / org / title_type”: “packing”,
                        "price": {
                            "currency": "INR",
                            "value": "0.5"
                        }
                    }, {
                        "title": "Tax",
                        “ @ ondc / org / title_type”: “tax”,
                        "price": {
                            "currency": "INR",
                            "value": "0.9"
                        }
                    }
                ],
                “ttl”: “P1D”
            }
        }
    }
}
```
- b. Validations
  - Serviceability check for pickup, drop off pin codes;
  - Logistics rate calculations
  - For products ordered:
    - quote.price.value = Σ (quote.breakup.price.value);
    - quote.breakup should have 1 entry for each “@ondc/org/item-id”;
  If any of the above validations in a or b fails, buyer app will return “NACK”;

- c. Error Messages<br>To communicate error messages related to inventory unavailability for a particular item or agent unavailability or price being updated, please follow the spec as defined here

## 7. /init payload
- a. Mandatory fields listed below with sample values
```
{
"context":
{
  "domain": "nic2004:52110",
  "action": "init",
  "core_version": "0.9.3",
  "bap_id": "ondc.paytm.com",
  "bap_uri": "https://ondc.paytm.com/retail/on_init",
  "bpp_id": "ondc.gofrugal.com/ondc/18275",
  "bpp_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
  "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
  "message_id": "1652092268192",
  "city": "std:080",
  "country": "IND",
  "timestamp": "2022-05-09T10:31:08.201Z",
  "ttl": "PT30S"
},
"message":
{
  "order":
  {
    "provider":
    {
      "id": "18275-ONDC-1-11094",
      "locations": [
        {
          "id": "18275-ONDC-1-11094"
        }
      ]
    },
    "items":
    [
      {
        "id": "18275-ONDC-1-9",
        "fulfillment_id": "Fulfillment1",
        "quantity":
        {
          "count": 1
        }
      }
    ],
    "billing":
    {
      "name": "Ekansh Katiyar",
      "address":
      {
        "door": "B005 aaspire heights",
        "name": "33rd Cross Road, Vinayaka Layout",
        "locality": "33rd Cross Road, Vinayaka Layout",
        "city": "Bengaluru",
        "state": "Karnataka",
        "country": "IND",
        "area_code": "560037"
      },
      "email": "ekansh@gmail.com"
      "phone": "9886098860",
      "created_at": "2022-05-09T10:31:08.367Z",
      "updated_at": "2022-05-09T10:31:08.367Z"
    },
    "fulfillments":
    [
    	{
			"id": "Fulfillment1",
			"type": "Delivery",
			"provider_id": "18275-ONDC-1-11094",
			"tracking": false,
			"end":
			{
				"location":
				{
				  "gps": "12.9492953,77.7019878",
				  "address":
				  {
					"door": "B005 aaspire heights",
					"name": "3rd Cross Road, Vinayaka Layout",
					"locality": "3rd Cross Road, Vinayaka Layout",
					"city": "Bengaluru",
					"state": "Karnataka",
					"country": "IND",
					"area_code": "560037"
				  }
				}
				"contact":
				{
					"phone": "9886098860"
				},
			},
		}
    ]
  }
}
```

## 8. /on_init payload
- a. Mandatory fields listed below with sample values
```
{
    "context": {
        "domain": "nic2004:52110",
        "action": "on_init",
        "core_version": "0.9.3",
        "bap_id": "ondc.paytm.com",
        "bap_uri": "https://ondc.paytm.com/retail/on_init",
        "bpp_id": "ondc.gofrugal.com/ondc/18275",
        "bpp_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
        "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
        "message_id": "1652092268192",
        "city": "std:080",
        "country": "IND",
        "timestamp": "2022-05-09T10:31:09.201Z"
    },
    "message": {
        "order": {
            "provider": {
                "id": "18275-ONDC-1-11094"
            },
            "provider_location": {
                "id": "18275-ONDC-1-11094"
            }
            "items":
            [{
                    "id": "18275-ONDC-1-9",
                    "fulfillment_id": "Fulfillment1",
                    "quantity": {
                        "count": 1
                    }
                }
            ],
            "billing": {
                "name": "Ekansh Katiyar",
                "address": {
                    "door": "B005 aaspire heights",
                    "name": "33rd Cross Road, Vinayaka Layout",
                    "locality": "33rd Cross Road, Vinayaka Layout",
                    "city": "Bengaluru",
                    "state": "Karnataka",
                    "country": "IND",
                    "area_code": "560037"
                },
                "email": "ekansh.katiyar@gmail.com",
                "phone": "9886098860",
                "created_at": "2022-05-09T10:31:08.367Z",
                "updated_at": "2022-05-09T10:31:08.367Z"
            },
            "fulfillments":
            [{
                    "id": "Fulfillment1",
                    "type": "Delivery",
                    "provider_id": "ondc.gofrugal.com/ondc/18275",
                    "tracking": false,
                    "end": {
                        "location": {
                            "gps": "12.9492953,77.7019878",
                            "address": {
                                "door": "B005 aaspire heights",
                                "name": "3rd Cross Road, Vinayaka Layout",
                                "locality": "3rd Cross Road, Vinayaka Layout",
                                "city": "Bengaluru",
                                "state": "Karnataka",
                                "country": "IND",
                                "area_code": "560037"
                            }
                        },
                        "contact": {
                            "phone": "9886098860"
                        }
                    }
                    ],
                    "quote": {
                        "price": {
                            "currency": "INR",
                            "value": "5.0"
                        },
                        "breakup":
                        [{
                                “ @ ondc / org / item_id”: “18275 - ONDC - 1 - 9”,
                                “ @ ondc / org / item_quantity”: {
                                    "count": 1
                                }
                                "title": "SENSODYNE SENSITIVE TOOTH BRUSH",
                                “ @ ondc / org / title_type”: “item”,
                                "price": {
                                    "currency": "INR",
                                    "value": "5.0"
                                }
                            }, {
                                "title": "Delivery charges",
                                “ @ ondc / org / title_type”: “delivery”,
                                "price": {
                                    "currency": "INR",
                                    "value": "0.0"
                                }
                            }
                        ]
                    },
                    "payment": {
                        "@ondc/org/buyer_app_finder_fee_type": “Percent”,
                        "@ondc/org/buyer_app_finder_fee_amount": “3”,
                        "@ondc/org/settlement_details":
                        [{
                                "settlement_counterparty": "seller-app",
                                "settlement_phase": "sale-amount",
                                "settlement_type": "upi",
                                "upi_address": "gft@oksbi",
                                "settlement_bank_account_no": "XXXXXXXXXX",
                                "settlement_ifsc_code": "XXXXXXXXX",
                                “beneficiary_name”: “xxxxx”,
                                “bank_name”: “xxxx”,
                                “branch_name”: “xxxx”
                            }
                        ]
                    }
			}
		}
	}
```
- b. Validations
  - quote.price.value = Σ (quote.breakup.price.value);
  - quote.breakup should have 1 entry for each “@ondc/org/item-id”;
- c. If any of the above validations in a or b fails, buyer app will return “NACK”;
- d. Error Messages<br>To communicate error messages related to inventory unavailability for a particular item or agent unavailability or price being updated, please follow the spec as defined here

## 9. /confirm payload
a.	Mandatory fields listed below with sample values
```
{
  "context":
  {
    "domain": "nic2004:52110",
    "action": "confirm",
    "core_version": "0.9.3",
    "bap_id": "ondc.paytm.com",
    "bap_uri": "https://ondc.paytm.com/retail/on_init",
    "bpp_id": "ondc.gofrugal.com/ondc/18275",
    "bpp_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
    "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
    "message_id": "1652092268193",
    "city": "std:080",
    "country": "IND",
    "timestamp": "2022-05-09T10:31:10.201Z",
    "ttl": "PT30S"
  },
  "message":
  {
    "order":
    {
      "id": "0799f385-5043-4848-8433-4643ad511a14",
      "state": "Created ",
      "provider":
      {
        "id": "18275-ONDC-1-11094",
        "locations":
        [
          {
            "id": "GFFBRTFR1649830006"
          }
        ]
      },
      "items":
      [
        {
          "id": "18275-ONDC-1-9",
          "fulfillment_id": "Fulfillment1",
          "quantity":
          {
            "count": 1
          }
        }
      ],
      "billing":
      {
        "name": "Ekansh Katiyar",
        "address":
        {
          "door": "B005 aaspire heights",
          "name": "3rd Cross Road, Vinayaka Layout",
          "locality": "3rd Cross Road, Vinayaka Layout",
          "city": "Bengaluru",
          "state": "Karnataka",
          "country": "IND",
          "area_code": "560037"
        },
        "phone": "9886098860",
        "email": "ekansh.katiyar@gmail.com"
      },
      "fulfillments":
      [
 	 {
        "id": "Fulfillment1",
        "type": "Delivery",
        "tracking": false,
        "provider_id": "ondc.gofrugal.com/ondc/18275",
        "end":
        {
          "contact":
          {
            "email": "aditya@dataorc.in",
            "phone": "8793771717"
          },
          "location":
          {
            "gps": "12.9492953,77.7019878",
            "address":
            {
              "door": "B005 aaspire heights",
              "name": "33rd Cross Road, Vinayaka Layout",
              "locality": "33rd Cross Road, Vinayaka Layout",
              "city": "Bengaluru",
              "state": "Karnataka",
              "country": "IND",
              "area_code": "560037"
            }
          }
 }
      ],
      "quote":
      {
        "price":
        {
          "currency": "INR",
          "value": "5.0"
        },
        "breakup":
        [
          {
            “@ondc/org/item_id”: “18275-ONDC-1-9”,
            “@ondc/org/item_quantity”:
            {
              "count": 1
            }
            "title": "SENSODYNE SENSITIVE TOOTH BRUSH",
            “@ondc/org/title_type”: “item”,
            "price":
            {
              "currency": "INR",
              "value": "5.0"
            }
          },
          {
            "title": "Delivery charges",
            “@ondc/org/title_type”: “delivery”,
            "price":
            {
              "currency": "INR",
              "value": "0.0"
            }
          },
          {
            "title": "Packing charges",
            “@ondc/org/title_type”: “packing”,
            "price":
            {
              "currency": "INR",
              "value": "0.0"
            }
          },
          {
            "title": "Tax",
            “@ondc/org/title_type”: “tax”,
            "price":
            {
              "currency": "INR",
              "value": "0.0"
            }
          }
        ]
      },
      "payment":
      {
        "uri": "https://ondc.transaction.com/payment",
        "tl_method": "http/get",
        "params":
        {
          "currency": "INR",
          "transaction_id": "20220510111212800110168115448993937",
          "transaction_status": "XXXX",
          "amount": "5.0"
        },
        "status": "PAID",
        "type": "ON-ORDER",
        "collected_by": "BAP",
        "@ondc/org/buyer_app_finder_fee_type": “Percent”,
        "@ondc/org/buyer_app_finder_fee_amount": “3”,
        "@ondc/org/settlement_details":
        [
      	{
         		 "settlement_counterparty": "seller-app",
 "settlement_phase": "sale-amount",
 "settlement_type": "upi",
              "upi_address": "gft@oksbi",
              "settlement_bank_account_no": "XXXXXXXXXX",
              "settlement_ifsc_code": "XXXXXXXXX",
              “beneficiary_name”: “xxxxx”,
              “bank_name”: “xxxx”,
              “branch_name”: “xxxx”
        	}
        ]
      },
      "created_at": "2022-05-10T18:01:53.000Z",
      "updated_at": "2022-05-10T18:02:19.000Z",
      }
  }
```

## 10. /on_confirm payload
- a. Mandatory fields listed below with sample values
```
{
    "context": {
        "domain": "nic2004:52110",
        "action": "on_confirm",
        "core_version": "0.9.3",
        "bap_id": "ondc.paytm.com",
        "bap_uri": "https://ondc.paytm.com/retail/on_init",
        "bpp_id": "ondc.gofrugal.com/ondc/18275",
        "bpp_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
        "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
        "message_id": "1652092268193",
        "city": "std:080",
        "country": "IND",
        "timestamp": "2022-05-09T10:31:11.201Z"
    },
    "message": {
        "order": {
            "id": "0799f385-5043-4848-8433-4643ad511a14",
            “state”: “CREATED”,
            "provider": {
                "id": "18275-ONDC-1-11094",
                “locations”:
                [{
                        “id”: “GFFBRTFR1649830006 "
                        	}
                        ]
                        },
                        " items ":
                        [{
                        " id ": " 18275 - ONDC - 1 - 9 ",
                        " fulfillment_id ": " Fulfillment1 ",
                        " quantity ":{
                        " count ": 1
                        }
                        }
                        ],
                        " billing ":{
                        " name ": " Ekansh Katiyar ",
                        " address ":{
                        " door ": " B005 aaspire heights ",
                        " name ": " 3rd Cross Road,
                        Vinayaka Layout ",
                        " locality ": " 3rd Cross Road,
                        Vinayaka Layout ",
                        " city ": " Bengaluru ",
                        " state ": " Karnataka ",
                        " country ": " IND ",
                        " area_code ": " 560037 "
                        },
                        " email ": " ekansh.katiyar @ gmail.com ",
                        " phone ": " 9886098860 ",
                        " created_at ": " 2022 - 05 - 09T10: 31: 08.367Z ",
                        " updated_at ": " 2022 - 05 - 09T10: 31: 08.367Z "
                        },
                        " fulfillments ":
                        [
                             {
                        " id ": " Fulfillment1 ",
                        " type ": " Delivery ",
                        " tracking ": false,
                        " start ":{
                        " location ":{
                        " id ": " GFFBRTFR1649830006 ",
                        " descriptor ":{
                        " name ": " GFFBRTFR1649830006 "
                        },
                        " gps ": " 12.956399,
                        77.636803 "
                        },
                        " time ":{
                        " range ":{
                        " start ": " 2022 - 05 - 10T20: 02: 19.609Z ",
                        " end ": " 2022 - 05 - 10T20: 32: 19.609Z "
                        }
                        },
                        " instructions ":{
                        " name ": " Status for pickup ",
                        " short_desc ": " Pickup Confirmation Code "
                        },
                        " contact ":{
                        " phone ": " 9876543210 ",
                        " email ": " ondc @ gmail.com "
                        }
                        },
                        " end ":{
                        " location ":{
                        " gps ": " 12.9492953,
                        77.7019878 ",
                        " address ":{
                        " door ": " B005 aaspire heights ",
                        " name ": " 33rd Cross Road,
                        Vinayaka Layout ",
                        " locality ": " 33rd Cross Road,
                        Vinayaka Layout ",
                        " city ": " Bengaluru ",
                        " state ": " Karnataka ",
                        " country ": " IND ",
                        " area_code ": " 560037 "
                        }
                        },
                        " time ":{
                        " range ":{
                        " start ": " 2022 - 05 - 10T20: 02: 19.609Z ",
                        " end ": " 2022 - 05 - 10T20: 32: 19.609Z "
                        }
                        },
                        " instructions ":{
                        " name ": " Status for drop ",
                        " short_desc ": " Delivery Confirmation Code "
                        },
                        " contact ":{
                        " phone ": " 9886098860 "
                        }
                        }
                             },
                        ],
                        " quote ":{
                        " price ":{
                        " currency ": " INR ",
                        " value ": " 5.0 "
                        },
                        " breakup ":
                        [
                             {
                        “@ondc/org/item_id”: “18275-ONDC-1-9”,
                        “@ondc/org/item_quantity”:
                         {
                        " count ": 1
                         }
                        " title ": " SENSODYNE SENSITIVE TOOTH BRUSH ",
                        “@ondc/org/title_type”: “item”,
                        " price ":{
                        " currency ": " INR ",
                        " value ": " 5.0 "
                        }
                             },
                             {
                        " title ": " Delivery charges ",
                        “@ondc/org/title_type”: “delivery”,
                        " price ":{
                        " currency ": " INR ",
                        " value ": " 0.0 "
                        }
                             },
                             {
                        " title ": " Packing charges ",
                        “@ondc/org/title_type”: “packing”,
                        " price ":{
                        " currency ": " INR ",
                        " value ": " 0.0 "
                        }
                             },
                             {
                        " title ": " Tax ",
                        “@ondc/org/title_type”: “tax”,
                        " price ":{
                        " currency ": " INR ",
                        " value ": " 0.0 "
                        }
                             }
                        ]
                        },
                        " payment ":{
                        " uri ": " https: //ondc.transaction.com/payment",
                        "tl_method": "http/get",
                        "params": {
                            "currency": "INR",
                            "transaction_id": "20220510111212800110168115448993937",
                            "amount": "5.0"
                        },
                        "status": "PAID",
                        "type": "ON-ORDER",
                        "collected_by": "BAP",
                        "@ondc/org/buyer_app_finder_fee_type": “Percent”,
                        "@ondc/org/buyer_app_finder_fee_amount": “3”,
                        "@ondc/org/settlement_details":
                        [{
                                "settlement_counterparty": "seller-app",
                                "settlement_phase": "sale-amount",
                                "settlement_type": "upi",
                                "upi_address": "gft@oksbi",
                                "settlement_bank_account_no": "XXXXXXXXXX",
                                "settlement_ifsc_code": "XXXXXXXXX",
                                “beneficiary_name”: “xxxxx”,
                                “bank_name”: “xxxx”,
                                “branch_name”: “xxxx”
                            }
                        ]
                    },
                    "created_at": "2022-05-10T18:01:53.000Z",
                    "updated_at": "2022-05-10T18:02:19.000Z",
                }
            }
```

- b. Validations
  - Buyer app will create the order id which will be unique across the network;
  - Confirm and on_confirm requests are idempotent;
  - In response to confirm request from buyer app, seller app sends on_confirm response with the same order_id;
  - Buyer app sends NACK for on_confirm for any of the following:
    - if the order_id is different (from confirm);
    - order object has changed (items and / or quantity is different, fulfillment type is different);
    - there is an error in the order e.g. missing message body, no order.state;
    - if the order is cancelled by the buyer before the buyer app receives ACK for /confirm;
    - In this case, there will be no further retries and Seller app and / or Buyer app should cancel the order;
  - Buyer app sends ACK for on_confirm if the order_id is the same (as confirm) and order object is same;
  - If the Buyer app sends NACK which is lost, the Seller app can't consume NACK. If the seller app now sends unsolicited /on_status, buyer app will send NACK with appropriate error codes;
    - If on_confirm order.state="Cancelled" or on_confirm returns a business error:
    - Buyer App will send ACK and update order.state to ‘Cancelled’;
    - No further retries in this case;
  - If the Buyer app does not receive on_confirm response within the ttl defined for confirm, it will initiate retry, with a maximum of 3 retries within the ttl outer limit defined (30s);

- c. Error Messages<br>To communicate error messages related to inventory unavailability for a particular item or agent unavailability or price being updated, please follow the spec as defined here

## 11. /status payload
- a. Mandatory fields listed below with sample values
```
{
  "context":
  {
    "domain": "nic2004:52110",
    "action": "status",
    "core_version": "0.9.3",
    "bap_id": "ondc.paytm.com",
    "bap_uri": "https://ondc.paytm.com/retail/status",
    "bpp_id": "ondc.gofrugal.com/ondc/18275",
    "bpp_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
    "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
    "message_id": "1652092268194",
    "city": "std:080",
    "country": "IND",
    "timestamp": "2022-05-09T10:31:12.201Z",
    "ttl": "PT30S"
  },
  "message":
  {
     "order_id": "0799f385-5043-4848-8433-4643ad511a14"
  }
}
```

## 12. /on_status payload
- a. Mandatory fields listed below with sample values
```
{
    "context": {
        "domain": "nic2004:52110",
        "action": "on_status",
        "core_version": "0.9.3",
        "bap_id": "ondc.paytm.com",
        "bap_uri": "https://ondc.paytm.com/retail/on_status",
        "bpp_id": "ondc.gofrugal.com/ondc/18275",
        "bpp_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
        "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
        "message_id": "1652092268194",
        "city": "std:080",
        "country": "IND",
        "timestamp": "2022-05-09T10:31:13.201Z",
        "ttl": "PT30S"
    },
    "message": {
        "order": {
            "id": "0799f385-5043-4848-8433-4643ad511a14",
            “state”: “DELIVERED”,
            "provider": {
                "id": "18275-ONDC-1-11094",
                “locations”:
                [{
                        “id”: “GFFBRTFR1649830006 "
                        	}
                        ]
                        },
                        " items ":
                        [{
                        " id ": " 18275 - ONDC - 1 - 9 ",
                        " fulfillment_id ": " Fulfillment1 ",
                        " quantity ":{
                        " count ": 1
                        }
                        }
                        ],
                        " billing ":{
                        " name ": " Ekansh Katiyar ",
                        " address ":{
                        " door ": " B005 aaspire heights ",
                        " name ": " 33rd Cross Road,
                        Vinayaka Layout ",
                        " locality ": " 33rd Cross Road,
                        Vinayaka Layout ",
                        " city ": " Bengaluru ",
                        " state ": " Karnataka ",
                        " country ": " IND ",
                        " area_code ": " 560037 "
                        },
                        " email ": " ekansh.katiyar @ gmail.com ",
                        " phone ": " 9886098860 ",
                        " created_at ": " 2022 - 05 - 09T10: 31: 08.367Z ",
                        " updated_at ": " 2022 - 05 - 09T10: 31: 08.367Z "
                        },
                        " fulfillments ":
                        [
                             {
                        " id ": " Fulfillment1 ",
                        " type ": " Delivery ",
                        " tracking ": false,
                        " state ":{
                        " descriptor ":{
                        " name ": " Order delivered ",
                        " code ": " Delivered - package "
                        }
                        },
                        " start ":{
                        " location ":{
                        " descriptor ":{
                        " name ": " GFT Digi store ",
                        " images ":
                        [
                        " https: //gf-ig-integration-qa-c0nn3c7.s3.amazonaws.com/images/5458
                        /5458_OrderEasy_3_1639793053567.png"
                        ]
                        },
                        "gps": "12.956399,77.636803"
                        },
                        "contact":
                    {
                        "phone": "9876543210",
                        "email": "ondc@gmail.com"
                        }
                        },
                        "end":
                    {
                        "location":
                    {
                        "gps": "12.9492953,77.7019878",
                        "address":
                    {
                        "door": "B005 aaspire heights",
                        "name": "33rd Cross Road, Vinayaka Layout",
                        "locality": "33rd Cross Road, Vinayaka Layout",
                        "city": "Bengaluru",
                        "state": "Karnataka",
                        "country": "IND",
                        "area_code": "560037"
                        }
                        },
                        "contact":
                    {
                        "phone": "9886098860"
                        }
                        }
                             }
                        ],
                        "quote":
                    {
                        "price":
                    {
                        "currency": "INR",
                        "value": "5.0"
                        },
                        "breakup":
                        [
                             {
                        “@ondc/org/item_id”: “18275-ONDC-1-9”,
                        “@ondc/org/item_quantity”:
                         {
                        "count": 1
                         },
                        "title": "SENSODYNE SENSITIVE TOOTH BRUSH",
                        “@ondc/org/title_type”: “item”,
                        "price":
                    {
                        "currency": "INR",
                        "value": "5.0"
                        }
                             },
                             {
                        "title": "Delivery charges",
                        “@ondc/org/title_type”: “delivery”,
                        "price":
                    {
                        "currency": "INR",
                        "value": "0.0"
                        }
                             },
                             {
                        "title": "Packing charges",
                        “@ondc/org/title_type”: “packing”,
                        "price":
                    {
                        "currency": "INR",
                        "value": "0.0"
                        }
                             },
                             {
                        "title": "Tax",
                        “@ondc/org/title_type”: “tax”,
                        "price":
                    {
                        "currency": "INR",
                        "value": "0.0"
                        }
                             }
                        ]
                        },
                        "payment":
                    {
                        "uri": "https:/ / ondc.transaction.com / payment ",
                        " tl_method ": " http / get ",
                        " params ":{
                        " currency ": " INR ",
                        " transaction_id ": " 20220510111212800110168115448993937 ",
                        " transaction_status ": " XXXX ",
                        " amount ": " 5.0 "
                        },
                        " status ": " PAID ",
                        " type ": " ON - ORDER ",
                        " collected_by ": " BAP ",
                        " @ ondc / org / buyer_app_finder_fee_type ": “Percent”,
                        " @ ondc / org / buyer_app_finder_fee_amount ": “3”,
                        " @ ondc / org / settlement_details ":
                        [
                             {
                        " settlement_counterparty ": " seller - app ",
                        " settlement_phase ": " sale - amount ",
                        “settlement_reference”: “XXXX”,
                        “settlement_status”: “PAID”,
                        “settlement_timestamp”: “2022-05-10T18:01:53.000Z",
                        "settlement_type": "upi",
                        "upi_address": "gft@oksbi",
                        "settlement_bank_account_no": "XXXXXXXXXX",
                        "settlement_ifsc_code": "XXXXXXXXX",
                        “beneficiary_name”: “xxxxx”,
                        “bank_name”: “xxxx”,
                        “branch_name”: “xxxx”
                    }
                ]
            },
            "created_at": "2022-05-10T18:01:53.000Z",
            "updated_at": "2022-05-10T18:02:19.000Z",
        }
    }
}
```

## 13. /cancel payload
- a. Mandatory fields listed below with sample values (note: cancellation reason codes are defined here)
```
{
  "context":
  {
    "domain": "nic2004:52110",
    "country": "IND",
    "city": "std:080",
    "action": "cancel",
    "core_version": "0.9.3",
    "bap_id": "ondc.paytm.com",
    "bap_uri": "https://ondc.paytm.com/retail/on_status",
    "bpp_id": "ondc.gofrugal.com/ondc/18275",
    "bpp_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
    "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
    "message_id": "1652092268195",
    "timestamp": "2022-05-09T10:31:14.201Z",
                 "ttl": "PT30S"
  },
  "message":
  {
    "order_id": "0799f385-5043-4848-8433-4643ad511a14",
    "cancellation_reason_id": "004"
  }
}
```
- Note: /cancel should only be used for Order cancellation and can be initiated only if Order.state is “Created”. 

## 14. /on_cancel payload
- a. Mandatory attributes listed below with sample values
```
{
  "context":
  {
    "domain": "nic2004:52110",
    "country": "IND",
    "city": "std:080",
    "action": "on_cancel",
    "core_version": "0.9.3",
    "bap_id": "ondc.paytm.com",
    "bap_uri": "https://ondc.paytm.com/retail",
    "bpp_id": "ondc.gofrugal.com/ondc/18275",
    "bpp_uri": "https://ondc.gofrugal.com/ondc/seller/adaptor",
    "transaction_id": "9fdb667c-76c6-456a-9742-ba9caa5eb765",
    "message_id": "1652092268195",
    "timestamp": "2022-05-09T10:31:15.201Z",
    "ttl": "PT30S"
  },
  "message":
  {
    "order":
    {
      “id”: "0799f385-5043-4848-8433-4643ad511a14",
      “state”: "Cancelled"
      “tags”:
      {
"cancellation_reason_id": "004"
      }
    }
  }
}
```
- Note (for seller app):
  - a. If Order.state is not “Created”, return NACK in /cancel;
  - b. If cancellation_reason_id is not valid, return NACK in /cancel;
