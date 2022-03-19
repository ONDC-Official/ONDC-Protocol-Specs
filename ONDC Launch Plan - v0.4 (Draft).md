# ONDC Launch Plan - v0.4 (Draft)



## 1. Overview

This note covers the following aspects of ONDC Launch:

- Different phases are covered here, along with the entry and exit criteria, for each phase;
- Process for participants to join ONDC during any phase;
- Operations-readiness requirements for participants;
- Market-readiness requirements for participants;



## 2. ONDC Launch

#### (a) ONDC launch summary

| Phases                                    | Infra Readiness                                              | Soft Launch (phase 0)                                        | Soft Launch (phase 1)                     | Soft Launch (phase 2) [^1]                | Expansion                              |
| ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------------------------------------- | ----------------------------------------- | -------------------------------------- |
| **Timelines**                             | 28th Feb - 4th Mar 2022                                      | 7th Mar - 11th Mar 2022                                      | 14th Mar - 31st Mar 2022                  | 1st Apr - 30th Apr 2022                   | 1st May - 15th Aug 2022                |
| **Environment**                           | Live                                                         | Live                                                         | Live                                      | Live                                      | Live                                   |
| **Addition to registry**                  | Manual                                                       | Manual                                                       | Manual                                    | Self-service registration                 | Self-service registration              |
| **Certification**                         | Manual                                                       | Manual                                                       | Manual                                    | Automated                                 | Automated                              |
| **Minimum number of participants**        | Depends on volunteering participants                         | 3 (1 retail buyer app, 1 retail seller app, 1 logistics seller app) | No minimum                                | No minimum                                | No minimum                             |
| **Entry criteria**                        | As defined in Tech requirements in (b)                       | As defined in Tech requirements in (b)                       | As defined in Tech requirements in (b)    | As defined in Tech requirements in (b)    | As defined in Tech requirements in (b) |
| **Users**                                 | NA                                                           | Test Users                                                   | Real-life users (by invitation only)      | Open to all users                         | Open to all users                      |
| **Domains**                               | Retail (Groceries), Logistics                                | Retail (Groceries + F&B), Logistics                          | Retail (Groceries + F&B), Logistics       | Retail (Groceries + F&B), Logistics       | Retail, Logistics                      |
| **Use Cases**                             | NA                                                           | **TBD**                                                      | As defined in Tech requirements in (b)    | As defined in Tech requirements in (b)    | As defined in Tech requirements in (b) |
| **Physical movement of goods & services** | No                                                           | No                                                           | Yes                                       | Yes                                       | Yes                                    |
| **Transfer of money**                     | NA                                                           | No                                                           | Yes                                       | Yes                                       | Yes                                    |
| **Network Policy**                        | Yes                                                          | Yes                                                          | Yes                                       | Yes                                       | Yes                                    |
| **Network Agreement**                     | No                                                           | No                                                           | Yes                                       | Yes                                       | Yes                                    |
| **Cities**                                | NA                                                           | Delhi, Bengaluru, Coimbatore, Bhopal, Shillong                    | Delhi, Bengaluru, Coimbatore, Bhopal, Shillong | Delhi, Bengaluru, Coimbatore, Bhopal, Shillong | 300 cities                             |
| **Exit Criteria**                         | Successful load test of gateway & registry (as defined in c) | Validation of use cases with network policy                  | NA                                        | NA                                        | NA                                     |

[^1]: **Any participant who enters ONDC from this phase onwards will have to meet the exit criteria for Soft Launch (phase 0) in the Pre-production environment**  




&nbsp;
&nbsp;

#### (b) Minimum Tech requirements for launch

| #    | **Requirement**                    | Buyer App                                                    | Seller App                                                   |
| ---- | ---------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | Protocol version                   | Transaction APIs - use latest version [here](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/tree/master/protocol-specifications/core/v0/api); Registry APIs - 0.2.0 | Transaction APIs - use latest version [here](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/tree/master/protocol-specifications/core/v0/api); Registry APIs - 0.2.0 |
| 2    | All APIs implemented               | /search, /on_search;<br />/select, /on_select;<br />/init, /on_init;<br />/confirm, /on_confirm;<br />/status, /on_status;<br />/track, /on_track;<br />/cancel, /on_cancel;<br />/update, /on_update,<br />/rating, /on_rating;<br />/support, /on_support | /search, /on_search;<br />/select, /on_select;<br />/init, /on_init;<br />/confirm, /on_confirm;<br />/status, /on_status;<br />/track, /on_track;<br />/cancel, /on_cancel;<br />/update, /on_update,<br />/rating, /on_rating;<br />/support, /on_support |
| 3    | Protocol Error Codes               | N/A                                                          | Implement Protocol Level Error Codes [here](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/blob/master/protocol-specifications/docs/draft/Error%20Codes.md) |
| 4    | Key pairs for signing & encryption | Pseudonymous key pairs for signing & encryption, as defined [here](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/blob/master/protocol-specifications/docs/draft/Auth%20Header%20Signing%20and%20Verification.md)| Pseudonymous key pairs for signing & encryption, as defined [here](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/blob/master/protocol-specifications/docs/draft/Auth%20Header%20Signing%20and%20Verification.md) 
| 5    | Auth header signing & verification | Verify auth headers for all responses;Sign auth headers for all requests; | Verify auth headers for all requests;Sign auth headers for all responses; |
| 6    | Network Policy                     | Comply with the configurations, schema policy, standard transaction codes that will form the network policy for the launch (draft doc is [here](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/blob/master/protocol-specifications/docs/draft/ONDC%20Network%20Policy%20-%20v0.3%20(draft).md)) | Comply with the configurations, schema policy, standard transaction codes that will form the network policy for the launch (draft doc is [here](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/blob/master/protocol-specifications/docs/draft/ONDC%20Network%20Policy%20-%20v0.3%20(draft).md)) |
| 7    | Cascaded Integration               | N/A                                                          | Cascaded integration between Retail Seller App and one or more Logistics Providers - includes searching for logistics providers, confirming the order and passing status & tracking information to the buyer app |
| 8    | Functional Test - Normal flows     | **Complete retail order flow (from catalog search to order completion), providing rating, status & tracking, updates;**<br />**Detailed flows to be provided;** | **Complete retail order flow (from catalog search to order completion), providing rating, status & tracking, updates;**<br />**Detailed flows to be provided;** |
| 9    | Functional Test - Exception flows  | **Includes Normal flow, returns (part), cancel, grievance;**<br /><br />**Draft proposal for cancellation, return & replacement is** [**here**](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/blob/master/protocol-specifications/docs/draft/Order%20Cancellation%2C%20Returns%20and%20Replacements%20Process%20Flow.md)**;**<br /><br />**Detailed flows to be provided;** | **Includes Normal flow, returns (part), cancel, grievance;**<br /><br />**Draft proposal for cancellation, return & replacement is** [**here**](https://github.com/Open-network-for-digital-commerce/ONDC-Protocol-Specs/blob/master/protocol-specifications/docs/draft/Order%20Cancellation%2C%20Returns%20and%20Replacements%20Process%20Flow.md)**;**<br /><br />**Detailed flows to be provided;** |
| 10   | Payment Integration                | Draft proposal for the Payment & Settlement protocol is [here](https://docs.google.com/document/d/1iqLdayk488ekEzKrEs-yn6gVrevbxBkILBe5j4oIxMY/edit) | Draft proposal for the Payment & Settlement protocol is [here](https://docs.google.com/document/d/1iqLdayk488ekEzKrEs-yn6gVrevbxBkILBe5j4oIxMY/edit) |
| 11   | Data Privacy & Security            | - Maintain logs of confirmed orders, i.e. confirmed payload for /confirm, /on_confirm;<br /><br /> - Transparent consent mechanism for sharing of PII;<br /><br />-  Purpose limitation, storage limitation for PII;<br /><br /> - Security Safeguards<br /><br />-  Data Privacy & Security will be the responsibility of the app entity; | - Maintain logs of confirmed orders, i.e. confirmed payload for /confirm, /on_confirm;<br /><br /> - Transparent consent mechanism for sharing of PII;<br /><br />-  Purpose limitation, storage limitation for PII;<br /><br /> - Security Safeguards<br /><br />-  Data Privacy & Security will be the responsibility of the app entity; |
| 12   | Security                           | Recommended: <br />Web Application Firewall (WAF - L3 / 4 / 7) | Recommended: <br />Web Application Firewall (WAF - L3 / 4 / 7) |
| 13   | Issue Logging & Monitoring         | Using Github Issue & Discussion board. All participants should have the Github IDs for one or more contacts added to ONDC Github Account | Using Github Issue & Discussion board. All participants should have the Github IDs for one or more contacts added to ONDC Github Account |



&nbsp;
&nbsp;


#### (c) Process for participants joining ONDC at any time

![Joining ONDC](https://user-images.githubusercontent.com/95357304/154923474-ef559ec3-8b40-4324-b339-9d885c9341d8.jpg)
![Joining ONDC 2](https://user-images.githubusercontent.com/95357304/154923134-e42d1ec5-a148-44be-b581-2264359e9c2c.jpg)





&nbsp;
