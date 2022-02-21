# ONDC Launch Plan - v0.2 (Draft)



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
| **Cities**                                | NA                                                           | Delhi, Bengaluru, Udipi, Bhopal, Shillong                    | Delhi, Bengaluru, Udipi, Bhopal, Shillong | Delhi, Bengaluru, Udipi, Bhopal, Shillong | 300 cities                             |
| **Exit Criteria**                         | Successful load test of gateway & registry (as defined in c) | Validation of use cases with network policy                  | NA                                        | NA                                        | NA                                     |

[^1]: **Any participant who enters ONDC from this phase onwards will have to meet the exit criteria for Soft Launch (phase 0) in the Pre-production environment**  




&nbsp;
&nbsp;

#### (b) Minimum Tech requirements for launch

| #    | **Requirement**                    | Buyer App                                                    | Seller App                                                   |
| ---- | ---------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | Protocol version                   | Transaction APIs - current version is 0.9.3 (expect 0.9.4 for soft launch);Registry APIs - 0.2.0 | Transaction APIs - current version is 0.9.3 (expect 0.9.4 for soft launch);Registry APIs - 0.2.0 |
| 2    | All APIs implemented               | /search, /on_search;<br />/select, /on_select;<br />/init, /on_init;<br />/confirm, /on_confirm;<br />/status, /on_status;<br />/track, /on_track;<br />/cancel, /on_cancel;<br />/update, /on_update,<br />/rating, /on_rating;<br />/support, /on_support | /search, /on_search;<br />/select, /on_select;<br />/init, /on_init;<br />/confirm, /on_confirm;<br />/status, /on_status;<br />/track, /on_track;<br />/cancel, /on_cancel;<br />/update, /on_update,<br />/rating, /on_rating;<br />/support, /on_support |
| 3    | Protocol Error Codes               | N/A                                                          | Implement Protocol Level Error Codes [here](https://github.com/beckn/protocol-specifications/blob/core-0.9.4-draft/docs/protocol-drafts/BECKN-RFC-005-Error-Codes-Draft-01.md) |
| 4    | Self-Signed DSC                    | Generate self-signed DSC with key pairs for signing & encryption | Generate self-signed DSC with key pairs for signing & encryption |
| 5    | Auth header signing & verification | Verify auth headers for all responses;Sign auth headers for all requests; | Verify auth headers for all requests;Sign auth headers for all responses; |
| 6    | Network Policy                     | Comply with the configurations, schema policy, standard transaction codes that will form the network policy for the launch (draft doc is [here](https://docs.google.com/document/d/11tGCJiOv7iYK3ILvKB6MuOB_UsQFMgnyIWEtqhCdQUM/edit)) | Comply with the configurations, schema policy, standard transaction codes that will form the network policy for the launch (draft doc is [here](https://docs.google.com/document/d/11tGCJiOv7iYK3ILvKB6MuOB_UsQFMgnyIWEtqhCdQUM/edit)) |
| 7    | Cascaded Integration               | N/A                                                          | Cascaded integration between Retail Seller App and one or more Logistics Providers - includes searching for logistics providers, confirming the order and passing status & tracking information to the buyer app |
| 8    | Functional Test - Normal flows     | **Complete retail order flow (from catalog search to order completion), providing rating, status & tracking, updates;**<br />**Detailed flows to be provided;** | **Complete retail order flow (from catalog search to order completion), providing rating, status & tracking, updates;**<br />**Detailed flows to be provided;** |
| 9    | Functional Test - Exception flows  | **Includes Normal flow, returns (part), cancel, grievance;**<br /><br />**Draft proposal for returns & replacements is** [**here**](https://docs.google.com/document/d/1vNQhIj_3RvXZYx68OeUnOHqUYra8rGPn4hqS2816ESY/edit)**;**<br /><br />**Detailed flows to be provided;** | **Includes Normal flow, returns (part), cancel, grievance;**<br /><br />**Draft proposal for returns & replacements is** [**here**](https://docs.google.com/document/d/1vNQhIj_3RvXZYx68OeUnOHqUYra8rGPn4hqS2816ESY/edit)**;**<br /><br />**Detailed flows to be provided;** |
| 10   | Payment Integration                | Draft proposal for the Payment & Settlement protocol is [here](https://docs.google.com/document/d/1xvNdEAreNNqC5RqPp7ykwlFZCA1Mkw7E8_DGZbjgCf8/edit) | Draft proposal for the Payment & Settlement protocol is [here](https://docs.google.com/document/d/1xvNdEAreNNqC5RqPp7ykwlFZCA1Mkw7E8_DGZbjgCf8/edit) |
| 11   | Data Privacy & Security            | - Maintain logs of confirmed orders, i.e. confirmed payload for /confirm, /on_confirm;<br /><br /> - Transparent consent mechanism for sharing of PII;<br /><br />-  Purpose limitation, storage limitation for PII;<br /><br /> - Security Safeguards<br /><br />-  Data Privacy & Security will be the responsibility of the app entity; | - Maintain logs of confirmed orders, i.e. confirmed payload for /confirm, /on_confirm;<br /><br /> - Transparent consent mechanism for sharing of PII;<br /><br />-  Purpose limitation, storage limitation for PII;<br /><br /> - Security Safeguards<br /><br />-  Data Privacy & Security will be the responsibility of the app entity; |
| 12   | Security                           | Recommended: <br />Web Application Firewall (WAF - L3 / 4 / 7) | Recommended: <br />Web Application Firewall (WAF - L3 / 4 / 7) |
| 13   | Issue Logging & Monitoring         | Using Github Issue & Discussion board. All participants should have the Github IDs for one or more contacts added to ONDC Github Account | Using Github Issue & Discussion board. All participants should have the Github IDs for one or more contacts added to ONDC Github Account |



&nbsp;
&nbsp;


#### (c) Process for participants joining ONDC at any time

![Joining ONDC](https://user-images.githubusercontent.com/95357304/154923474-ef559ec3-8b40-4324-b339-9d885c9341d8.jpg)
![Joining ONDC 2](https://user-images.githubusercontent.com/95357304/154923134-e42d1ec5-a148-44be-b581-2264359e9c2c.jpg)





&nbsp;
