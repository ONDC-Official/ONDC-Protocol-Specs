# Payment and Settlement Protocol - v0.6 (DRAFT) 

| Version | Date          | Changes                                                      |
| ------- | ------------- | ------------------------------------------------------------ |
| 0.6     | 24th Mar 2022 | - Changed scenarios for Seller App collecting payment to also update status using unsolicited on_status callback; |
| 0.5     | 22nd Mar 2022 | - Changed "Seller App Settlement details" for scenario 2 to "Buyer App Settlement details" (page 12);<br />- Added for scenario b - "Who collects payment" - if the Seller App asserts its right to collect payment, the buyer aoo should agree in the subsequent init call (page 17); |
| 0.4     | 16th Mar 2022 | - Assertion to collect payment (by Buyer App or Seller App) to be decided during order initialization, whether or not it’s a part of search intent;<br />- Deterministic negotiation protocol for payment & settlement terms;<br />- Withholding amount to be held by entity that collects payment;Updated Technical Specs for different scenarios;<br />- Added scenarios for settlement; |
| 0.3     | 8th Feb 2022  | Incorporate following changes to align with updated ONDC Commercial Policy Release 1:<br />- Willingness to collect payment will be a part of the search intent from the Buyer App;<br />- Settlement time starts from the time the proof of payment is available;<br />- Open Issues - markup for buyer app during initialising checkout of order? refund trail for COD; |
| 0.2     | 7th Feb 2022  | Initial Version                                              |



## 1. Overview

The objective of this note is to propose a payment & settlement protocol for initiating discussion with the ONDC community. ONDC will support a diverse set of Buyer App and Seller App entities, in different domains, each of whom may have their own policies for payment collection by themselves and for settlement with the counterparty. This note proposes a way for different parties, to a transaction, to mutually negotiate their payment & settlement terms & conditions that leads to a deterministic outcome for the order transaction.

Payment collection could be at either of these points:

- Buyer App
- Seller App
- Logistics Agent - only for Cash on Delivery (COD)

Every transaction in ONDC will be a bilateral transaction between a Buyer App and Seller App. Payment & Settlement, between different participating entities, will happen using existing payment technology (e.g. payment gateway, UPI, wallets, etc.) and will be outside the network. However, the terms & conditions, for such Payment & Settlement, and the promise or proof of payment will be transmitted using the underlying protocol.

For the 1st two collection points defined above, there are 4 possible scenarios for payment collection in ONDC:
![image1](https://user-images.githubusercontent.com/95357304/159215694-36f3f15e-4c6f-4254-9070-61130942b6ae.png)
<p align="center">
Figure 1: Collection Flexibility: Possible Scenarios
</p>


ONDC will allow collection flexibility, i.e. allow the Buyer App or Seller App to decide whether to collect payment for an order, as per their collection policies, and have it acceptable to the counterparty. This can include the following scenarios:

- Buyer App asserts its right to collect payment, optionally through the search intent but also during Order initialization. This has to be accepted by the Seller App during Order initialization, irrespective of whether they honoured the assertion during the search intent;
- Buyer App doesn’t assert its right to collect payment, optionally through the search intent but also during order initialization. This has to be accepted by the Seller App, by agreeing to collect payment, during Order initialization, irrespective of whether they honoured the lack of assertion, by the Buyer App, during the search intent by implicitly agreeing to collect payment through their display of the Catalogue;

**While scenarios 1 & 3 above result in agreement between the Buyer App & Seller App, regarding payment collection, scenarios 2 & 4 above result in lack of agreement and the Order transaction failure prior to confirmation**. “Who collects payment” is one of the parameters that will be negotiated between the counterparties to the transaction and the other parameters are defined later in this note. In order to ensure a deterministic outcome for this type of negotiation, a [negotiation protocol](https://docs.google.com/document/d/1H1ZOwpcUkjrKNyeVKJjwqkxCXFEri0C84JkqOsR5CAI/edit) is proposed.

Payment & Settlement in ONDC will be guided by ONDC network policies which will evolve in consultation with the participant community. Payment & Settlement in ONDC will be guided by the following network policies:

- Participants in the ONDC network **will not need to enter into bilateral or multilateral agreements, with other participants, for settlement**. Instead, all participants need to adhere to a Network Level Agreement with ONDC that among others will also define the payment & settlement terms, which includes payment & settlement outside the network through the mutual negotiation & transmission of these terms, alongwith the proof or promise of payment, between participants;
- Whoever collects the payment (i.e. Buyer app or Seller app) could optionally maintain a refund pool, using the withholding amount from a transaction, which could be used to settle refunds in cases of returns, damaged items, etc, and that is withheld until the return window closes;
- It is also possible that Buyer App and Seller App mutually agree to settle for a delivered order after expiry of the return window. In this case, neither the Buyer App nor the Seller App shall be required to maintain a refund pool created from withholding amount and will also eliminate the need for reconciliation of withholding amount and its adjustment;



## **2. Role of participants in a transaction**

The table below lists the different participants in a transaction, covering all possible payment & settlement scenarios, and hence could be involved in the payment & settlement process. Every participant in the payment & settlement process may not be a ONDC network participant (all such participants who are also ONDC network participants are colour-coded).

The key participants in a transaction are:

- **Buyer** - orders items using Buyer App;

- **Buyer App** - entity that provides the Buyer Application, using which the buyer can purchase items using ONDC;

- **Seller App** - entity that provides the Seller Application which is a storefront for the sellers, for publishing their catalogue and processing & fulfilling orders;

- **Seller** - any merchant or store that maintains items and processes & fulfils orders for these items through a Seller App;

  | Participant Types   | Buyer | Buyer App                                                    | Seller App                                                   | Seller                                                       | Logistics Agent - only for Cash on Delivery (CoD)            |
  | ------------------- | ----- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | **Buyer**           | NA    | 1. Buyer **pays** Buyer App using their payment gateway or other online / offline mechanism. Buyer app may even provide a custom payment plan for the buyer;<br />2. Buyer App settles with Seller App and other participants as applicable (e.g. Logistics Provider) as per their settlement terms after deducting the withholding amount (from the Seller App);<br />3. After the return window closes, Buyer App settles the withholding amount with Seller App (in case of no refunds for return, damages, etc.); | 1. Buyer **pays** Seller App using the payment terms transmitted as part of the protocol;<br />2. Seller App settles with the other participants in the transaction as per their settlement terms;<br />3. Seller App settles with Seller as per their mutual terms & conditions. Seller App can also maintain refund pool using withholding amount from the Seller to handle cases like refund for return, damages, etc.; | NA                                                           | **Scenario 1 - Logistics Services procured by Seller App:**<br />1. Buyer **pays** the Logistics agent post fulfilment of the order;<br />2. Logistics Agent remits the collection to the Logistics Provider as per their settlement terms;<br />3. Logistics Provider settles with Seller App (after deducting the amount for logistics services), as per their mutual settlement terms;<br />4. Seller App settles with Buyer App as per their mutual settlement terms;5. Seller App can also maintain refund pool using withholding amount from the Seller to handle cases like refund for return, damages, etc.;<br /><br />**Scenario 2 - Logistics Services procured by Buyer App:**<br />1. Buyer **pays** the Logistics agent post fulfilment of the order;<br />2. Logistics Agent remits the collection to the Logistics Provider as per their mutual settlement terms;<br />3. Logistics Provider settles with Buyer App (after deducting the amount for logistics services), as per their mutual settlement terms;<br />4. Buyer App settles with Seller App after deducting the withholding amount, as per their mutual settlement terms;5. After the return window closes, Buyer App settles withholding amount with the Seller App (in case of no refund for return, damages, etc.); |
  | **Buyer App**       | NA    | NA                                                           | Discussed Above                                              | NA                                                           | NA                                                           |
  | **Seller App**      | NA    | Discussed Above                                              | Discussed above (e.g. between retail seller app and logistics seller app) | Seller App **settles** with Seller as per their mutual terms & conditions | NA                                                           |
  | **Seller**          | NA    | NA                                                           | NA                                                           | NA                                                           | NA                                                           |
  | **Logistics Agent** | NA    | Discussed Above                                              | Discussed Above                                              | NA                                                           | NA                                                           |

  

## **3. Possible use of Refund Pool**

The refund pool, maintained by the Buyer App or Seller App, using the withholding amount, could be used for quick settlement with the buyer, for the following:

- Refund in case of part or full return;
- Refund in case the buyer receives items (e.g. perfume) in a damaged or broken state;



## **4. Commercial Components of Order**

The commercial components of an order shall include the following:

- **Declared Price** (per item) - this is the price declared by the seller-on-record or merchant on the Seller App (GST inclusive) and includes the following:

  - **Buyer App Fee** - this is the finder fee for the Buyer App as a % of the declared price or a fixed amount per order added to the declared price;

  - **Seller App Fee** - this is the fee charged by the Seller App, to the seller-on-record, as a % of the declared price or a fixed amount per order added to the declared price, for listing their catalogue items;

  - **ONDC Fee** - this is ONDC’s per-transaction fee as a % of the declared price;

  - Final Product consideration due to the seller-on-record is (Declared Price) - [(Buyer App Fee + Seller App Fee + ONDC Fee) + (GST on Buyer App Fee, Seller App Fee, ONDC Fee)]

  **Example**

  Declared price (for item) = ₹100

  Buyer App Fee - 3% of declared price = ₹3

  Seller App Fee - 5% of declared price = ₹5

  ONDC Fee - 1.5% of declared price = ₹1.5

  Final consideration due to seller-on-record (assuming 18% GST) is ₹100 - (₹3 + ₹5 + ₹1.5) - 0.18*(₹3 + ₹5 + ₹1.5) = ₹88.79

- The quote for an order will be the cumulative value of the Declared Price for individual items in the Order, price for logistics services and markup / discounts provided by the Buyer App;

- Additional charges such as for logistics services (whether imposed by Seller App or Buyer App), markups or discounts offered by Buyer App, etc. will be separate line items in the Order;

- The cost for logistics services will be included in the quote for the Order, through the seller app or buyer app, based on how it was procured. In this case, the payment for logistics services will be initiated by the Seller App or Buyer App that procured such services;

- Invoice will be generated by the seller-on-record and made available to the buyer as part of the protocol;



## **5. Terms & Conditions**

Payment & Settlement terms & conditions will be decided as part of the Order lifecycle. Some of these terms & conditions will be fixed by one or more parties to the transaction. Other terms & conditions will be mutually negotiated between the parties to the transaction.

1. **Fixed terms & conditions**

| Terms & Conditions    | Fixed by                             | Mandatory? | Default Value                                                | Definition                                                   | Protocol Reference                                           |
| --------------------- | ------------------------------------ | ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Buyer App Finder Fee  | Buyer App (as part of Search Intent) | No         | 0                                                            | Finder fee for the Buyer App as a % of the cumulative declared price or a fixed amount per order added to cumulative declared price | API<br /><br />/search;<br /><br />Schema<br /><br />Intent.payment.”./ondc-buyer_app_finder_fee_type”<br /><br />Intent.payment.”./ondc-buyer_app_finder_fee_amount” |
| Settlement Start Time | Based on type of payment             | No         | For prepaid - timestamp of payment transaction reference; <br />for CoD - timestamp of delivery | For **prepaid**, based on settlement basis - collection (timestamp of payment transaction reference), shipment or delivery;<br />For **cash-on-delivery**, settlement start time is triggered on delivery. | Schema - for:<br /><br />Collection - order.payment.time.timestamp<br /><br />Shipment -order.fulfillment.start.time.timestamp<br /><br />Delivery -order.fulfilment.end.time.timestamp |

2. **Negotiable terms & conditions**

Terms & conditions for payment & settlement, that will be mutually negotiated between the participants, as part of the transaction, include:

| Terms & Conditions    | Fixed by                                                     | Mandatory? | Default Value    | Definition                                                   | Protocol Reference                                           |
| --------------------- | ------------------------------------------------------------ | ---------- | ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Who collects payment? | Who collects the payment - buyer app, seller app (logistics agent always collects for cash on delivery) | Yes        | N/A              | a. Search Intent (optional);<br />b. Order Initialization (mandatory) | a. API - /search;<br /><br />Schema - Intent.payment.collected_by<br /><br />b. API - /init, /on_init;<br /><br />Schema - Order.payment.collected_by |
| Settlement basis      | In case of prepaid payment, whether settlement between counterparties should be on the basis of collection, shipment or delivery | No         | Collection basis |                                                              | Schema -<br /><br />order.payment.”./ondc-settlement_basis”  |
| Witholding Amount     | % of cumulative declared price (by the entity that collects payment) | No         | 0                | Order Initialization                                         | API - /init, /on_init;<br /><br />Schema - order.payment.”./ondc-withholding_amount” |
| Return Window         | Number of calendar days from order delivery, after which withholding amount will have to be settled, in case of no returns | No         | N/A              | Order Initialization                                         | API - /init, /on_init;<br /><br />Schema - order.payment.”./ondc-return_window” |
| Settlement Window     | Number of calendar days from settlement start time, within which settlement with counterparty has to be completed | Yes        | N/A              | Order Initialization                                         | API - /init, /on_init;<br /><br />Schema -order.payment.”./ondc-settlement_window” |



## **6. Payment & Settlement Process Flow**

This section elaborates the process flows for Payment & Settlement, for an order, along with appropriate examples, for different scenarios, in ONDC:



**Scenario 1 - Buyer App collect payments, Seller App agrees**

The process flow for payment & settlement will be as follows for a Buyer App and a single Seller App. In case of multiple Seller Apps, the same process will be repeated for each Seller App.

- Buyer App asserts the right to **collect payment** for the Order. Buyer App asserts this right, by also specifying the **withholding amount** as a % of cumulative declared price, **settlement basis**, **settlement window** and **return window**;
- Seller App agrees to Buyer App asserting to collect payment.
- Seller App & Buyer App can mutually negotiate on the other terms & conditions (**withholding amount, settlement basis, settlement window, return window)** and come to an agreement. When there is agreement, Seller App specifies its settlement mechanism;
- Buyer App collects payment from Buyer and settles with Seller App as per its settlement terms & conditions, after deducting the withholding amount. In case logistics services were procured through the Buyer App, the Buyer App also settles with the logistics provider, based on its settlement terms & conditions;
- In case logistics services were procured through the Seller App, the Seller App settles with the logistics provider based on its settlement terms & conditions;
- After the **return window** closes, Buyer App transfers the withholding amount to Seller App as per its settlement terms & conditions in case of no returns (settlement window will be applied from the day after closure of return window);


![image3](https://user-images.githubusercontent.com/95357304/159215735-b7c744a6-164e-47dd-93ab-12fc4e40912d.png)
<p align="center">
Figure 2: Process Flow: Payment Collection by Buyer App
</p>
**Example**

Buyer purchases 10 kg of Atta through a Buyer App and declared price for this item by a Seller App is ₹630. Buyer App has a fixed finder fee of 5%, included in the declared price. Seller App also provides free delivery through logistics provider;

During confirmation of order, buyer app & seller app negotiate the payment & settlement terms & conditions as follows (assume settlement start time is 21st Mar 2022):

1. Buyer App & Seller App mutually negotiate and agree on the following - **withholding amount** of 10%, **settlement window** of 2 days, **return window** of 7 days;
2. Seller App provides its settlement details (payee bank account no);
3. Buyer App collects ₹630 from buyer;
4. Buyer App deducts **withholding amount** of ₹63, its **finder fee** of ₹31.5 and pays Seller App ₹535.5 by 23rd Mar 2022;
5. Buyer App transfers withholding amount of ₹63 to Seller App by 30th Mar 2022 (on closure of return window on 28th Mar, assuming no returns);



**Scenario 2 - Seller App collects payment**

The process flow for payment & settlement will be as follows for a Buyer App and a single Seller App. In case of multiple Seller Apps, the same process will be repeated for each Seller App.

- Buyer App refuses to assert first right to collect payment and Seller App agrees to collect payment;
- Seller App & Buyer App mutually negotiate & agree on the following - **withholding amount** of 10%, **settlement window** of 2 days, **return window** of 7 days.
- Buyer App provides its settlement details (payee bank account no);
- Seller App collects payment from Buyer and settles with Buyer App, its **Finder Fee**, as per the mutually agreed settlement terms & conditions. In case logistics services were procured through the Buyer App, the Seller App also includes the cost of logistics services;
- In case logistics services were procured through the Seller App, the Seller App settles with the logistics provider based on its settlement terms & conditions;

![image5](https://user-images.githubusercontent.com/95357304/159215784-b1b8fd5b-ff3e-4d07-b956-361d53d3a87f.png)
<p align="center">
Figure 3: Process Flow: Payment Collection by Seller App
</p>

**Example**

Buyer purchases 5 kg of Atta through a Buyer App and declared value for this item by a Seller App is ₹315. Buyer App has a fixed finder fee of 5%, included in the declared price. Seller App also provides home delivery through a logistics provider which charges fees of ₹30. Hence, total order value is ₹315 + ₹30 = ₹345;

During confirmation of order, buyer app & seller app negotiate the payment & settlement terms & conditions as follows (assume settlement start time is 21st Mar 2022):

1. Buyer App refuses to assert right to collect payment and Seller App agrees to collect payment;
2. Seller App & Buyer App mutually negotiate & agree on the following - **settlement window** of 2 days;
3. Buyer App provides its settlement details (payee bank account no);Seller App collects ₹345 from buyer;
4. Seller App pays Buyer App ₹15 (Finder fee) by 23rd Mar 2022;
5. Seller App pays Logistics Provider ₹30 as per their settlement terms & conditions;Settlement of withholding amount will be between the Seller App and the Seller;



**Cash on delivery payment**

Cash on delivery payment can happen only when there is a match between the payment modes offered by the buyer and the modes of collection by the Logistics Agent, e.g. buyer offers payment in Cash or UPI and Logistics Agent accepts these modes of payment collection. In this case, the cash on delivery payment will happen and the order fulfilment is completed.

In case there is a mismatch in the payment modes offered by the buyer and the modes of collection by the Logistics Agent, e.g. buyer offers payment in credit card and Logistics Agent accepts cash or UPI only, it will be up to the Logistics Provider to decide whether to cancel the order and perform a return to origin (RTO).

The process flow for payment & settlement will be as follows for a Buyer App, a single Seller App and a single Logistics Provider. In case of multiple Seller Apps, it is assumed the order will be fulfilled by the same or different logistics providers, but the order through a Seller App will be fulfilled by a single logistics provider. Hence, in the case of multiple Seller Apps with a single or multiple logistics providers, the same process will be repeated for each Logistics Provider.

- As part of the order confirmation process, Buyer App and Seller App will mutually negotiate and agree to the following terms & conditions - **withholding amount** & **return window** (in case logistics services procured by Buyer App), **settlement window**. Additionally, the logistics provider would have negotiated their settlement terms & conditions with the Seller App or Buyer App, through which their services was procured;
- Logistics Agent collects payment from Buyer and settles with Logistics Provider (as per their terms & conditions). Settlement start time, with Seller App and Buyer App, is from the delivery of order;
- If logistics services were procured through the Seller App:
  - Logistics Provider settles with Seller App, after deducting their charges from the collected amount, as per their settlement terms & conditions;
  - Seller App settles with the Buyer App as per its settlement terms & conditions (i.e. **Finder fee**);
- If logistics services were procured through the Buyer App:
  - Logistics Provider settles with Buyer App, after deducting their charges from the collected amount, as per their settlement terms & conditions;
  - Buyer App settles with the Seller App as per its settlement terms & conditions, after deducting its **Finder fee** and **withholding amount**;
  - After the **return window** closes, Buyer App transfers the **withholding amount** to Seller App as per its settlement terms & conditions (settlement window will be applied from the day after closure of return window);


![image4](https://user-images.githubusercontent.com/95357304/159215800-1568eabe-14db-4869-af8e-dda19fb21dcb.png)
<p align="center">
Figure 4: Process Flow: Cash on Delivery Payment
</p>

**Example**

Buyer purchases 5 kg of Atta through a Buyer App and declared value for this item by a Seller App is ₹420. Buyer App has a fixed finder fee of 5%, included in the declared price. Seller App also provides home delivery through a logistics provider which charges fees of ₹30. Hence, total order value is ₹420 + ₹30 = ₹450;

During confirmation of order, buyer app & seller app negotiate the payment & settlement terms & conditions as follows (assume settlement start time starts on 21st Mar 2022):

1. Buyer App & Seller App mutually negotiate and agree on the following - **settlement window** of 3 days;
2. Buyer App provides its settlement details (payee bank account no);
3. Seller App agrees to settlement window of 1 day with Logistics Provider and specifies its payee account details;
4. Logistics Agent collects ₹450 from buyer and settles with their Logistics Provider;
5. Logistics Provider pays Seller App ₹420 (after deducting their charges of ₹30) by 22nd Mar;
6. Seller App pays Buyer App its Finder fee of ₹20 by 24th Mar;Settlement of Withholding Amount will be between the Seller App and the Seller;



**Refunds**

Refunds in cases of returns, cancellations, damaged items, etc. will be as follows:

1. **Cancellation** - refund will be initiated as follows:
   - Prepaid order - by the entity that collected the payment from the buyer (i.e. buyer app or seller app) and the refund money flow will be the exact reverse of the payment money flow;
   - CoD order - since cancellation happens before delivery, no payment would be made and hence, there is no refund;
2. **Return** - refund will be initiated as follows:
   - Prepaid order - by the entity that collected the payment from the buyer (and has the withholding amount) and the refund money flow will be the exact reverse of the payment money flow;
   - CoD order - by the entity that has the withholding amount (i.e. Seller App or Buyer App), and will be the same as the entity that procured the services of the Logistics Provider. If the Seller App initiates the refund, it will transfer the amount to the Buyer App and the refund will be in the form of credit to the buyer account. If the Buyer App initiates the refund, it will credit the buyer account;
3. **Damaged Items** - refund money flow will be the same as Return.



## **7. Technical details for Payment and Settlement**

Payment & Settlement terms will be communicated using the Payment object. This section shows the **Payment** object at different steps for each of the process flows presented earlier. Each of these scenarios only shows the parameters relevant to that specific scenario.

- **Buyer App Finder fee - fixed**

  **/search**: providing its finder fee as part of the search intent. This could be expressed as a fixed amount or a % of the declared price:
<pre>
  {

  “./ondc-buyer_app_finder_fee_type”: “Amount”,

  “./ondc-buyer_app_finder_fee_amount”: “50”

  }

  OR

  {

  “./ondc-buyer_app_finder_fee_type”: “Percent”,

  “./ondc-buyer_app_finder_fee_amount”: “5”

  }
</pre>
- **Who collects payment - negotiable**

​	This scenario shows a single negotiable variable - “Who collects payment?”. Note that multiple variables can be negotiated simultaneously.

**/init**: Buyer App asserts its right to collect payment:
<pre>
{

“collected_by”: “BAP”,

“./ondc-collected_by_status”: “Assert”

}
</pre>
**/on_init**: Seller App agrees:
<pre>
{

“collected_by”: “BAP”,

“./ondc-collected_by_status”: “Agree”

}
</pre>
**/on_init**: If Seller App disagrees:
<pre>
{

“collected_by”: “BAP”,

“./ondc-collected_by_status”: “Disagree”

}
</pre>
**/init**: If Buyer App wishes to withdraw its assertion:
<pre>
{

“collected_by”: “”,

“./ondc-collected_by_status”: “”

}
</pre>
**/on_init**: If Seller App asserts its right to collect payment (note: the Buyer App should agree to this in the subsequent /init call):
<pre>
{

“collected_by”: “BPP”,

“./ondc-collected_by_status”: “Assert”

}
</pre>
**/init**: If Buyer App wishes to terminate the negotiation as it can’t reach an agreement with Seller App:
<pre>
{

“collected_by”: “BAP”,

“./ondc-collected_by_status”: “Terminate”

}
</pre>
**/on_init**: If Seller App wishes to terminate the negotiation as it can’t reach an agreement with Buyer App:
<pre>
{

“collected_by”: “BAP”,

“./ondc-collected_by_status”: “Terminate”

}
</pre>
- **Buyer App collects payment (prepaid) and provides proof of payment**

**/confirm**:
<pre>
{

  “collected_by”: “BAP”,

	“type”: “ON-ORDER”,

​	“status”: “PAID”,

​	“params”: {

​						“transaction_status”: “transaction completion reference number”,	

​						“amount”: “1000”,	

​						“currency”: “INR”

​	}

​	“time”: {	“timestamp”: “2022-03-21 00:00:00”

​	}

}
</pre>
- **Seller App provides UPI payment link for prepaid payment**

Seller App provides UPI payment link, for rendering, by Buyer App:

**/on_init**:
<pre>
{

​	“collected_by”: “BPP”,

​	“uri”: “upi://pay?pa=UPIID@oksbi&amp;pn=NAME&amp;&tr=123456789&amp;cu=INR&amp;am=1000”,

​	“tl_method”: “upi”,

​	“type”: “ON-ORDER”,

​	“status”: “NOT-PAID”,

​	“params”: {	

​						“amount”: “1000”,	

​						“currency”: “INR”

​	}

}
</pre>
Seller App provides proof of payment

**/on_status**:
<pre>
{

​	“collected_by”: “BPP”,

​	“uri”: “upi://pay?pa=UPIID@oksbi&amp;pn=NAME&amp;&tr=123456789&amp;cu=INR&amp;am=1000”,

​	“tl_method”: “upi”,

​	“type”: “ON-ORDER”,

​	“status”: “PAID”,

​	“params”: {	

​						“transaction_status”: “transaction completion reference number”,	

​						“amount”: “1000”,	

​						“currency”: “INR”

​	}

​	“time”: {	“timestamp”: “2022-03-21 00:00:00”

​	}

}
</pre>
- **Seller App provides payment gateway link for prepaid payment**

Seller App provides payment gateway link, for rendering, by Buyer App:
<pre>
**/on_init**:
<pre>
{

​	“collected_by”: “BPP”,

​	“uri”: “https://pay.example.com?transaction_id=$transaction_id&amount=$amount&cur=$currency”,

​	“tl_method”: “http/post”,

​	“type”: “ON-ORDER”,

​	“status”: “NOT-PAID”,

​	“params”: {	

​						“transaction_id”: “transaction identifier”,	

​						“amount”: “1000”,	

​						“currency”: “INR”

​	}

}
</pre>
Seller App provides proof of payment

**/on_status**:
<pre>
{

​	“collected_by”: “BPP”,

​	“uri”: “https://pay.example.com?transaction_id=$transaction_id&amount=$amount&cur=$currency”,

​	“tl_method”: “http/post”,“type”: “ON-ORDER”,

​	“status”: “PAID”,

​	“params”: {	

​						“transaction_id”: “transaction identifier”,	

​						“transaction_status”: “transaction completion reference number”,	

​						“amount”: “1000”,	

​						“currency”: “INR”

​	}

​	“time”: {	“timestamp”: “2022-03-21 00:00:00”

​	}

}
</pre>
- **Cash on Delivery payment**

Buyer App decides on CoD payment:

**/init**:
<pre>
{
	“type”: “POST-FULFILLMENT”,

​	“status”: “NOT-PAID”,

​	“params”: {	

​						“amount”: “1000”,	

​						“currency”: “INR”,

​	}

}
</pre>
- **Buyer App collects payment, Seller App provides settlement details**

**/on_init**:
<pre>
{

​	“collected_by”: “BAP”,

​	“./ondc-settlement_details”: {	

​				“settlement_counterparty”: “seller-app”,	

​				“settlement_type”: “neft”,	

​				“settlement_bank_account_no”: “bank account no”,	

​				“settlement_ifsc_code”: “IFSC code”,

​	}

}
</pre>
- **Seller App collects payment, Buyer App provides settlement details**

**/init**:
<pre>
{

​	“collected_by”: “BPP”,

​	“./ondc-settlement_details”: {	

​				“settlement_counterparty”: “buyer-app”,	

​				“settlement_type”: “neft”,	

​				“settlement_bank_account_no”: “bank account no”,	

​				“settlement_ifsc_code”: “IFSC code”,

​	}

}
</pre>
- **Buyer App settles, updates settlement details**

**/update** (message.update_target = “billing”):
<pre>
{

​	“./ondc-settlement_details”: {	

​				“settlement_counterparty”: “seller-app”,	

​				“settlement_phase”: “sale-amount”,	

​				“settlement_reference”: “transaction reference no”,	

​				“settlement_status”: “PAID”,	

​				“settlement_timestamp”: “2022-03-31 00:00:00”

​	}

}
</pre>
- **Seller App settles, updates settlement details**

**/on_update** (message.update_target = “billing”):
<pre>
{

​	“./ondc-settlement_details”: {	

		  “settlement_counterparty”: “buyer-app”,	

			“settlement_phase”: “sale-amount”,	

			“settlement_reference”: “transaction reference no”,	

			“settlement_status”: “PAID”,	“settlement_timestamp”: “2022-03-31 00:00:00”

​	}

}
</pre>
- **Buyer App checks settlement status**

**/status**

- **Seller App generates invoice** **for seller-on-record**

**/on_confirm** - use Order.billing, Order.documents





