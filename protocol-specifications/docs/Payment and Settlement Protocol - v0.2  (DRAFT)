# Payment and Settlement Protocol - v0.2 (DRAFT)


## 1. Overview

The objective of this note is to propose a payment & settlement protocol for initiating discussion with the ONDC community. ONDC will support a diverse set of Buyer App and Seller App entities, in different domains, each of whom may have their own policies for payment collection by themselves and by the counterparty.

Payment collection could be at either of these points[^1]:

- Buyer App[^2]
- Seller App
- Logistics Agent - only for Cash on Delivery (COD)

Every transaction in ONDC will be a bilateral transaction between a Buyer App and Seller App. Payment & Settlement, between different participating entities, will happen using existing payment technology (e.g. payment gateway, UPI, wallets, etc.) and will be outside the network. However, the terms & conditions, for such Payment & Settlement, and the promise or proof of payment will be transmitted using the Beckn protocol[^3].

For the 1st two collection points defined above, there are 4 possible scenarios for payment collection in ONDC:

![Visualising ONDC (5)](https://user-images.githubusercontent.com/95357304/152774144-21361590-15c1-44c9-984a-d09e6ef9d649.jpg)

This note addresses scenarios 1 and 3. Scenarios 2 & 4 will be covered at a later date.Payment & Settlement in ONDC will be guided by ONDC network policies which will evolve in consultation with the participant community. Payment & Settlement in ONDC will be guided by the following network policies:

- Participants in the ONDC network **will not need to enter into bilateral or multilateral agreements, with other participants, for settlement**. Instead, all participants need to adhere to a Network Level Agreement with ONDC that among others will also define payment & settlement outside the network through the transmission of payment & settlement terms, in a transaction on the network, between participants;

- Buyer app will maintain a return fund pool, using the withholding amount from a transaction[^4], that is withheld until the return window closes;

  

## 2. Role of participants in a transaction

The table below lists the different participants in a transaction, covering all possible payment & settlement scenarios, and hence could be involved in the payment & settlement process. Every participant in the payment & settlement process may not be a ONDC network participant (all such participants who are also ONDC network participants are color-coded).

The key participants in a transaction are:

- **Buyer** - orders items using Buyer App;
- **Buyer App** - entity that provides the Buyer Application, using which the buyer can purchase items using ONDC;
- **Seller App** - entity that provides the Seller Application which is a storefront for the sellers, for publishing their catalog and processing & fulfilling orders;
- **Seller** - any merchant or store that maintains items and processes & fulfills orders for these items through a Seller App;

| Participant Types | Buyer | Buyer App                                                    | Seller App                                                   | Seller                                                       | Logistics Agent                                              |
| :---------------- | ----- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Buyer             | N/A   | 1. Buyer **pays** Buyer App using their payment gateway or other online / offline mechanism. Buyer app may even provide a custom payment plan for the buyer;<br />2. Buyer App settles with Seller App and other participants as applicable (e.g. Logistics Provider) as per their settlement terms after deducting the withholding amount (from the Seller App);<br />3. After the return window closes, Buyer App settles the withholding amount with Seller App (in case of no return); | 1. Buyer **pays** Seller App using the payment terms transmitted as part of the protocol;<br />2. Seller App settles with the other participants in the transaction as per their settlement terms (also includes withholding amount for buyer app);<br />3. After the return window closes, Buyer App settles withholding amount with the Seller App (in case of no return); | N/A                                                          | **Scenario 1** - <br />Logistics Services procured by Seller App:<br />1. Buyer **pays** the Logistics agent post fulfilment of the order (only applicable for Cash on Delivery);<br />2. Logistics Agent settles with their Provider as per their settlement terms;<br />3. Logistics Provider settles with Seller App (after deducting the amount for logistics services), as per their settlement terms;<br />4. Seller App settles with Buyer App and also transfers the withholding amount, as per their settlement terms;<br />5. After the return window closes, Buyer App settles withholding amount with the Seller App (in case of no return);<br /><br />**Scenario 2** - Logistics Services procured by Buyer App:<br />1. Buyer **pays** the Logistics agent post fulfilment of the order (only applicable for Cash on Delivery);<br />2. Logistics Agent settles with their Provider as per their settlement terms;<br />3. Logistics Provider settles with Buyer App (after deducting the amount for logistics services), as per their settlement terms;<br />4. Buyer App settles with Seller App after deducting the withholding amount, as per their settlement terms;<br />5. After the return window closes, Buyer App settles withholding amount with the Seller App (in case of no return); |
| Buyer App         | N/A   | N/A                                                          | Discussed above                                              | N/A                                                          | N/A                                                          |
| Seller App        | N/A   | Discussed above                                              | Discussed above (e.g. between retail seller app and logistics seller app) | Seller App **settles** with Seller as per their mutual terms & conditions | N/A                                                          |
| Seller            | N/A   | N/A                                                          | N/A                                                          | N/A                                                          | N/A                                                          |
| Logistics         | N/A   | Discussed above                                              | Discussed above                                              |                                                              | N/A                                                          |



## 3. Payment & Settlement Protocol

Payment & Settlement, between different participating entities, will happen using existing payment technology (e.g. payment gateway, UPI, wallets, etc.) and will be outside the network. However, the terms & conditions, for such Payment & Settlement, and the promise or proof of payment will be transmitted as part of the transaction.

Payment & Settlement terms & conditions will be negotiated as part of the order lifecycle and is defined in detail in **Section 5 - Communication of Payment and Settlement T&C.** This section defines key variables that will be a part of these terms and conditions.

1. Every item that is sold on ONDC will have 2 prices - Order price, Declared price.
   - Declared Price = Price quoted by Seller App (inclusive of appropriate taxes);
   - Order Price = Declared Price +/- Buyer App’s addition or subtraction (+/- % of Declared Price);
2. An order in ONDC can have 1 or more items. Every order generated in ONDC will have 2 values - Order value, Declared value;
   - Declared value (DV) = Σ(Declared Price);
   - Order value (OV) = DV +/- Buyer App’s addition or subtraction (+/- % of DV);
   - It is assumed that offers and add-ons are included in the DV;
3. An order may also include logistics services and the quote for such will be included in the Order value, through the seller app or buyer app, based on how it was procured (i.e. by the Seller App or the Buyer App). In this case, the payment for logistics services will be initiated by the Seller App or Buyer App that procured such services;

Terms & conditions for payment & settlement, that will be mutually negotiated between the participants, as part of the transaction, include:

- **Withholding amount** - this will be a % of (Declared Value) as quoted by the Seller App (note this excludes the prices quoted by Logistics service provider);

- **Return window** - this is the **number of calendar days from order fulfillment**, after which withholding amount will have to be settled, in case of no returns;

- **Settlement window** - this is **number of calendar days from order fulfillment**, within which settlement with counterparty has to be completed;

  

## 4. Payment & Settlement Process Flow

This section elaborates the process flows for Payment & Settlement along with appropriate examples, for different scenarios, in ONDC:

### I. Scenario 1 - Buyer App collect payments, Seller App agrees

The process flow for payment & settlement will be as follows for a Buyer App and a single Seller App. In case of multiple Seller Apps, the same process will be repeated for each Seller App.

- Buyer App asserts the right to **collect payment** for the Order. Buyer App asserts this right, by also specifying the **withholding amount** as a % of Declared Value;
- Seller App can negotiate on the **withholding amount** with the Buyer App. When the Seller App agrees with the Buyer App payment terms, it specifies its terms (**return_window**, **settlement window)** and also the settlement mechanism;
- Buyer App collects payment from Buyer and settles with Seller App as per its settlement terms & conditions, after deducting the withholding amount (i.e. OV - withholding amount as % of DV). In case logistics services were procured through the Buyer App, the Buyer App also settles with the logistics provider, based on its settlement terms & conditions;
- In case logistics services were procured through the Seller App, the Seller App settles with the logistics provider based on its settlement terms & conditions;
- After the **return window** closes, Buyer App transfers the withholding amount to Seller App as per its settlement terms & conditions in case of no returns (settlement window will be applied from the day after closure of return window);

![Visualising ONDC (2)](https://user-images.githubusercontent.com/95357304/152773903-81a2961c-8a3b-4a10-b67f-9a6f68d36ba6.jpg)


**Example**

Buyer purchases 10 kg of Atta through a Buyer App and declared value for this item by a Seller App is ₹600. Buyer App adds a markup of 5%, hence total Order value is ₹630. Seller App also provides free delivery through logistics provider;

During confirmation of order, buyer app & seller app negotiate the payment & settlement terms & conditions as follows (assume the order fulfillment date is 7th Feb 2022):

1. Buyer App asserts right to collect payment by specifying **withholding amount** of 10%;

2. Seller App agrees to the **withholding amount** and specifies its **return window** of 7 days and its settlement terms (includes **settlement window** of 2 days and the payee bank account details);

3. Buyer App collects ₹630 from buyer;

4. Buyer App deducts **withholding amount** of ₹60 and pays Seller App ₹540 by 9th Feb 2022;

5. Buyer App transfers withholding amount of ₹60 to Seller App by 16th Feb 2022 (on closure of return window on 14th Feb, assuming no returns);

   

### II. Scenario 2: Seller App collects payment

The process flow for payment & settlement will be as follows for a Buyer App and a single Seller App. In case of multiple Seller Apps, the same process will be repeated for each Seller App.

- Buyer App refuses to assert first right to collect payment. Buyer App however specifies the **withholding amount** as a % of Declared Value and its settlement terms (**settlement window** and payment mechanism);
- Seller App can negotiate on the **withholding amount** with the Buyer App. When the Seller App agrees with the Buyer App terms, it specifies its terms (**return window**, **settlement window**) and also its settlement mechanism;
- Seller App collects payment from Buyer and settles with Buyer App as per its settlement terms & conditions, after also adding the withholding amount ((i.e. (OV - DV) + withholding amount as % of DV). In case logistics services were procured through the Buyer App, the Seller App also adds the cost of logistics services;
- In case logistics services were procured through the Seller App, the Seller App settles with the logistics provider based on its settlement terms & conditions;
- After the **return window** closes, Buyer App transfers the withholding amount to Seller App as per its settlement terms & conditions (settlement window will be applied from the day after closure of return window);

![Visualising ONDC (1)](https://user-images.githubusercontent.com/95357304/152774024-199cd5c8-e462-470b-a864-96362d780e80.jpg)


**Example**

Buyer purchases 5 kg of Atta through a Buyer App and declared value for this item by a Seller App is ₹300. Buyer App adds a markup of 2.5% on this. Seller App also provides home delivery through a logistics provider which charges fees of ₹30. Hence, total order value is ₹300 + 0.025 * ₹300 + ₹30 = ₹337.5;

During confirmation of order, buyer app & seller app negotiate the payment & settlement terms & conditions as follows (assume the order fulfillment date is 7th Feb 2022):

1. Buyer App refuses to assert right to collect payment, but specifies - **withholding amount** = 5%, **settlement window** of 2 days and their payee bank account details;
2. Seller App agrees to the **withholding amount** and specifies its terms ( **return window** of 7 days) and its settlement terms - **settlement window** of 2 days and the payee bank account details;
3. Seller App collects ₹337.5 from buyer;
4. Seller App pays Buyer App ₹7.5 (markup of 2.5%) + ₹15 (withholding amount) by 9th Feb 2022;
5. Seller App pays Logistics Provider ₹30 as per their settlement terms & conditions;
6. Buyer App transfers withholding amount of ₹15 to Seller App by 16th Feb 2022 (on closure of return window on 14th Feb, assuming no returns);

### III. Cash on Delivery Payment

The process flow for payment & settlement will be as follows for a Buyer App, a single Seller App and a single Logistics Provider. In case of multiple Seller Apps, it is assumed the order will be fulfilled by the same or different logistics providers, but the order through a Seller App will be fulfilled by a single logistics provider. Hence, in the case of multiple Seller Apps with a single or multiple logistics providers, the same process will be repeated for each Logistics Provider.

- As part of the order confirmation process, Buyer App and Seller App would have negotiated their terms & conditions such as the following - **withholding amount**, **settlement window** (for Buyer App) and **return window**, **settlement window** (for Seller App). Additionally, the logistics provider would have negotiated their settlement terms & conditions with the Seller App or Buyer App, through which their services was procured;

- Logistics Agent collects payment from Buyer and settles with Logistics Provider (as per their terms & conditions). Settlement window with Seller App and Buyer App opens from the date of fulfillment of order;

- If logistics services were procured through the Seller App:

  - Logistics Provider settles with Seller App, after deducting their charges from the collected amount, as per their settlement terms & conditions;
  - Seller App settles with the Buyer App as per its settlement terms & conditions (i.e. (OV - DV) + withholding amount as % of DV);
  - After the **return window** closes, Buyer App transfers the withholding amount to Seller App as per its settlement terms & conditions (settlement window will be applied from the day after closure of return window);

- If logistics services were procured through the Buyer App:

  - Logistics Provider settles with Buyer App, after deducting their charges from the collected amount, as per their settlement terms & conditions;
  - Buyer App settles with the Seller App as per its settlement terms & conditions, after deducting the withholding amount;
  - After the **return window** closes, Buyer App transfers the withholding amount to Seller App as per its settlement terms & conditions (settlement window will be applied from the day after closure of return window);

![Visualising ONDC](https://user-images.githubusercontent.com/95357304/152774574-a8c5cdb1-07e2-4261-a04a-4fd97dfeecdb.jpg)


  **Example**

  Buyer purchases 5 kg of Atta through a Buyer App and declared value for this item by a Seller App is ₹400. Buyer App adds a markup of 5% on this. Seller App also provides home delivery through a logistics provider which charges fees of ₹30. Hence, total order value is ₹400 + 0.05 * ₹400 + ₹30 = ₹450;

  During confirmation of order, buyer app & seller app negotiate the payment & settlement terms & conditions as follows (assume the order fulfillment date is 7th Feb 2022):

  1. Buyer App specifies - **withholding amount** = 10%, **settlement window** of 3 days and payee account details;
  2. Seller App agrees to the **withholding amount** & **settlement window** and specifies its **return window** of 7 days, settlement terms (**settlement window** of 3 days for withholding amount and payee account details);
  3. Seller App agrees to settlement window of 1 day with Logistics Provider and specifies its payee account details;
  4. Logistics Agent collects ₹450 from buyer and settles with their Logistics Provider;
  5. Logistics Provider pays Seller App ₹420 (after deducting their charges of ₹30) by 8th Feb 2022;
  6. Seller App pays Buyer App ₹60 (markup of 5% or ₹20 + withholding amount of 10% or ₹40) by 10th Feb;
  7. Buyer App transfers withholding amount of ₹40 to Seller App by 17th Feb 2022 (on closure of return window on 14th Feb, assuming no returns);

  ### IV. Returns

  Returns can be initiated by the Buyer through the Buyer App. Buyer App will refund the Buyer, as per their mutual terms & conditions, and adjust from the withholding amount for the Seller App.

  ### V. Cancellation

  In case payment has already been collected, order cancellation will result in a refund by the entity that collected the payment.

  

## 5. Communication of Payment & Settlement T&C

Payment & Settlement terms will be communicated using the Payment object. This section shows the Payment[^5] object at different steps for each of the process flows presented earlier:

1. **Buyer App Collects Payment**

   **/init**: Buyer App asserts the right to collect payment. It also specifies the following - withholding amount (as % of DV):

   ```
   {
   
   ​			“collected_by”: “BAP”,
   
   ​			“withholding_amount”: “10”
   
   }
   ```

   /**on-init**: Seller App can negotiate with the Buyer App on the withholding amount. Assuming that Seller App has agreed to Buyer App terms above, they also provide their settlement terms & conditions.

   ```
   {
   
   ​			“collected_by”: “BAP”,
   
   ​			“withholding_amount”: “10”,
   
   ​			“return_window”: “7”,
   
   ​			"uri": 
   
   ​			"[payto](https://datatracker.ietf.org/doc/html/rfc8905)://bank/987654321?amount=$currency:$value&ifsc=$ifsc&message=hello",
   
   ​			"tl_method": "PAYTO",
   
   ​			"type": "POST_FULFILLMENT",
   
   ​			"params": {
   
   ​				 			"ifsc": "HDFC0000009",
   
   ​							 "value": "200",
   
   ​							 "currency": "INR"
   
   },
   
   ​			"time": {
   
   ​				 		"range": {
   
   ​										 "end": "2022-02-10 00:00:00"
   
   ​						 }
   
   ​		}
   
   }
   ```

   **TBD - communicating promise to pay, proof of payment from Buyer App to Seller App, settlement of withholding amount.**

   

2. **Seller App Collects Payment**

   **/init**: Buyer App specifies the following - withholding amount (as % of DV), settlement terms (e.g. settlement window is 2 days after fulfillment which translates to Feb 9th 2022):

   ```
   {
   
    			“withholding_amount”: “10”,
   
   ​			"uri": 
   
   ​			"[payto](https://datatracker.ietf.org/doc/html/rfc8905)://bank/987654321?amount=$currency:$value&ifsc=$ifsc&message=hello",
   
   ​			"tl_method": "PAYTO",
   
   ​			"type": "POST_FULFILLMENT",
   
   ​			"params": { 
   
   ​							"ifsc": "SBIN0000575", 
   
   ​							"value": "200", 
   
   ​							"currency": "INR" 
   
   ​			},
   
   ​			"time": { 
   
   ​						"range": { 
   
   ​										"end": "2022-02-09 00:00:00" 
   
   ​						}
   
   ​			}
   
   }
   ```

   /**on-init**: The Seller App can request immediate payment through the UPI endpoint:

   ```
   {
   
   ​			“withholding_amount”: “10”,
   
   ​			“return_window”: “7”,
   
   ​			"uri": "payto://upi/example@upi?amount=$currency:$value&message=hello",
   
   ​			"tl_method": "UPI",
   
   ​			"type": "ON-ORDER",
   
   ​			"params": { 
   
   ​							"value": "200", 
   
   ​							"currency": "INR"
   
   ​			},
   
   ​			"time": { 
   
   ​						"range": { 
   
   ​										"end": "2022-02-10 00:00:00"
   
   ​						}
   
   ​			}
   
   }
   ```

   Alternatively, Seller App can request immediate payment through a Payment Gateway endpoint:

   ```
   {
   
   ​			“withholding_amount”: “10”,
   
   ​			“return_window”: “7”,
   
   ​			"uri": "https://pay.example.com?amount=$value&cur=$currency",
   
   ​			"tl_method": "HTTP/POST",
   
   ​			"type": "ON-ORDER",
   
   ​			"params": { 
   
   ​							"value": "200", 
   
   ​							"currency": "INR"
   
   ​			},
   
   ​			"time": { 
   
   ​						"range": { 
   
   ​										"end": "2022-02-10 00:00:00"
   
   ​						}
   
   ​			}
   
   }
   ```

   **TBD - communicating promise to pay, proof of payment from Buyer App to Seller App, settlement of withholding amount.**

   

   3. **Cash on Delivery Payment**  - this is identified by the following Payment object:

      ```
      {
      
      ​		"params": { 
      
      ​							"value": "200", 
      
      ​							"currency": "INR"
      
      ​			},
      
      }
      ```

      **TBD - communicating promise to pay, proof of payment from Buyer App to Seller App, settlement with Buyer App, Seller App, settlement of withholding amount.**


[^1]:[ONDC Commercial Model - as proposed in Community Call](https://docs.google.com/presentation/d/1j46hAtkeD48m9zxQLCsaPyDxVB7U9DnGVftKjhl4bR0/edit#slide=id.g109473468db_0_423)

[^2]:ONDC is logically partitioned into multiple domains, and each domain may have multiple Buyer App(s) and Seller App(s). While Logistics is also an independent domain, with its own Buyer App(s) and Seller App(s), for the purpose of this document Buyer App & Seller App refer to the Retail domain

[^3]: Payments on Beckn-enabled Networks](https://github.com/beckn/protocol-specifications/blob/master/docs/protocol-drafts/BECKN-RFC-002-Payments-On-Beckn-Enabled-Networks.md)

[^4]:[ONDC Commercial Model - as proposed in Community Call(https://docs.google.com/presentation/d/1XFgYVj2USXagyQBOKI24G_gojmuGljP5OxIW7klGb4w/edit#slide=id.g108fcf0e428_4_57)

[^5]:Some of the attributes being mentioned here may not be a part of the protocol for now. This will be updated in the core protocol or ONDC schema policy shortly. This section will accordingly be revised.





