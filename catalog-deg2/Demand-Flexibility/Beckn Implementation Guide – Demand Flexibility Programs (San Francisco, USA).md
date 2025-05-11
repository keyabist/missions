**Beckn** **Implementation** **Guide** **–** **Demand** **Flexibility**
**Programs** **(San** **Francisco,** **USA)**

**Version** **1.0**

**Version** **History**

> **Date**
>
> 10-05-2025

**Version**

> 1.0
>
> **Description**

Initial Draft Based on Beckn Schema

**Introduction**

This document provides integration guidelines for implementing
demand-side flexibility programs over a Beckn-enabled open energy
network. It showcases how energy providers and aggregators in San
Francisco can publish flexible load reduction programs, enabling users
to participate in grid-responsive activities and earn monetary rewards
for reducing electricity usage during peak hours.

This guide assumes the reader is familiar with the Beckn protocol,
message flow, and schema standards. It maps the JSON-based catalog
provided by participating BPPs to the Beckn on_search response pattern.

**Outcome** **Visualization**

**Scenario:**

*Emily,* *a* *tech-savvy* *resident* *of* *San* *Francisco,* *wants*
*to* *save* *on* *her* *electricity* *bill* *and* *support* *the* *grid*
*during* *peak* *hours.*

> ● She installs a Beckn-enabled energy app and opts into flexible
> energy-saving programs.
>
> ● She sees programs from:
>
> ○ **SF** **Grid** **LoadFlex** offering \$0.20/kWh for manual
> reductions during 5–8 PM
>
> ○ **FlexiGrid** **Aggregator** automating her smart devices to reduce
> usage and pay \$0.15/kWh
>
> ● Emily links her Nest thermostat with FlexiGrid and accepts
> LoadFlex’s manual opt-in alerts.
>
> ● During a Flex Alert, she gets a push notification to reduce usage or
> lets FlexiGrid control her thermostat automatically.
>
> ● Post-event, she sees energy saved and incentive earned within the
> app dashboard.

**Flow** **Diagrams**

**1.** **Beckn** **Message** **Flow**

As per Beckn’s architecture:

> ● search is initiated by the BAP.
>
> ● The BPPs respond asynchronously with on_search.

The catalog shown below is what is returned by the BPPs during the
on_search.

**API** **Calls** **and** **Schema**

**on_search**

**Sample** **Catalog** **Response** **Breakdown:**

> Unset
>
> "message": { "catalog": {
>
> "descriptor": {
>
> "name": "Energy Incentives and Subsidies" },
>
> "providers": \[...\] }
>
> }

**Provider** **1:** **SF** **Grid** **LoadFlex**

> **Field**
>
> Program Name
>
> Description
>
> Reward
>
> Mode
>
> Enrollment
>
> Info
>
> **Value**

Evening Peak Saver Manual Opt-in

Users are notified during peak hours (5–8 PM) to reduce usage manually

\$0.20/kWh

Manual

Required

[<u>sfeu.org/flexibility/peak-saver</u>](https://sfeu.org/flexibility/peak-saver)

**Provider** **2:** **FlexiGrid** **Aggregator**

> **Field**
>
> Program
>
> Name
>
> **Value**

Smart Appliance Flex Program

> Description
>
> Reward
>
> Mode
>
> Enrollment
>
> Info

Automated control of smart devices like Nest, Ecobee, Tesla Wall Charger

\$0.15/kWh

Automated

Required with device linking

[<u>flexigrid.com/programs/smart-load</u>](https://flexigrid.com/programs/smart-load)

**Taxonomy** **and** **Layer** **2** **Configuration**

Each flexibility event is tagged with the following structured metadata
(using tags schema):

**Tag** **Examples**

> Unset
>
> "tags": \[ {
>
> "descriptor": { "name": "Flexibility Event Details" }, "list": \[
>
> { "descriptor": { "name": "Reward" }, "value": "\$0.20 per reduced
> kWh" },
>
> { "descriptor": { "name": "Acceptance Mode" }, "value": "Manual" }
>
> \] }
>
> \]
>
> Note: Ensure the domain in context is set as flexibility.
>
> Version should be aligned with protocol schema version in use (e.g.,
> 1.1.0).

**Writing/Integrating** **Your** **Software**

**As** **a** **BAP:**

> ● Parse on_search catalogs and list programs in UI
>
> ● Group by provider and show tags like rewards, devices supported, and
> enrollment status
>
> ● Allow user enrollment and manage consent to device access

**As** **a** **BPP:**

> ● Publish programs using the catalog schema
>
> ● Use tags to encode custom details like appliance control mode,
> compatible brands
>
> ● Include additional_desc with URLs for policy details

**Links** **to** **Downloadable** **Resources**

> ● [<u>Beckn Protocol
> Schema</u>](https://github.com/beckn/protocol-specifications)
