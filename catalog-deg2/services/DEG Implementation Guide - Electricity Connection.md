**DEG** **Implementation** **Guide** **-** **Electricity**
**Connection** **Version** **1.0**

> **Version** **History**

Date: 11-05-2025 Version: 1.0

> Description: Initial Version
>
> **Introduction**

This document provides material that helps network participants build
and integrate their application with the **Digital** **Electricity**
**Grid** **(DEG)** network for electricity connection services. It
assumes the reader has a general understanding of the Beckn protocol,
its API interfaces, message structure, and how a typical network
transaction flows from search to fulfillment.

> **Structure** **of** **the** **Document**
>
> 1\. Outcome Visualization
>
> 2\. Flow Diagrams
>
> 3\. API Calls and Schema
>
> 4\. Taxonomy and Layer 2 Configuration
>
> 5\. Notes on Integrating with Your Own Software
>
> 6\. Links to Downloadable Resources
>
> 7\. Sandbox Details
>
> **Outcome** **Visualization**

**Use** **Case** **–** **Discovery** **and** **Request** **for** **New**
**Electricity** **Connection**

> ● Ramya moves into a newly constructed home in San Francisco and needs
> a new electricity connection.
>
> ● She opens a DEG-compatible consumer app and searches for "New
> Electricity Connection".
>
> ● The app sends a search call with an intent specifying
> "electricity-connection".
>
> ● The Gateway multicasts the request to all relevant BPPs.
>
> ● Providers like PG&E and GreenVolt respond with catalogs using the
> on_search callback.
>
> ● Ramya reviews the offerings and selects one for connection.
>
> **Flow** **Diagrams**

**Message** **Flow** **(search/on_search)**

> 1\. User triggers search from BAP
>
> 2\. BAP → Gateway
>
> 3\. Gateway multicasts to BPPs
>
> 4\. BPPs respond to BAP via on_search
>
> 5\. BAP aggregates catalogs and presents to user
>
> **API** **Calls** **and** **Schema**
>
> **1.** **search**

**Purpose**: Declares user intent for discovery. **Category**:
electricity-connection

**Sample** **Payload**:

{

> "context": {
>
> "domain": "service",
>
> "action": "search",
>
> "version": "1.1.0",
>
> "bap_id": "example-bap.com",
>
> "bap_uri": "https://api.example-bap.com/pilot/bap/energy/v1",
>
> "transaction_id": "connection-txn-8001",
>
> "message_id": "connection-msg-10001",
>
> "timestamp": "2025-05-07T13:59:00Z",
>
> "ttl": "PT10M",
>
> "location": {
>
> "country": { "code": "USA" },
>
> "city": { "code": "NANP:628" }
>
> }
>
> },
>
> "message": {
>
> "intent": {

"descriptor": { "name": "Request for a new electricity connection" },

"category": { "descriptor": { "code": "electricity-connection" } }

> }
>
> }

}

> **2.** **on_search**

**Purpose**: Returns catalog with providers and items.

> **Includes**: Pricing, service category, tags like connection_type,
> subsidy, etc.
>
> **Taxonomy** **and** **Layer** **2** **Configuration**
>
> **Tag** **Location**
>
> item.tags
>
> item.tags
>
> item.tags
>
> item.tags
>
> item.tags
>
> **Code**
>
> connection_type
>
> govt_subsidy

turnaround_days

> service_quality
>
> application_fee
>
> **Sample** **Values**

residential, commercial

> true, false
>
> 3, 5, 7
>
> premium, standard
>
> numeric (e.g. 50.00)
>
> **Integration** **Notes**
>
> ● Implement search API on BPP and on_search callback on BAP.
>
> ● All calls must respond with ACK/NACK.
>
> ● Use consistent message_id and transaction_id.
>
> ● Validate catalogs using Beckn schema.
