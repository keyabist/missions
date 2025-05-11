**Implementation** **Guide** **-** **Solar** **Panel** **and** **EV**
**Charger** **Installation**

**Version** **History**

> **Date**
>
> 2025-05-11

**Version**

> 1.0
>
> **Description**

Initial documentation

**Introduction**

This document serves as an implementation guide for the **Solar**
**Panel** **and** **EV** **Charger** **Installation** services over a
Beckn-enabled open network. It outlines use cases, data schema, message
flows, taxonomy, and guidance for integrating provider and seeker
applications using the Beckn Protocol. This guide assumes the reader has
a general understanding of Beckn architecture, API message design, and
ONIX integration.

**Outcome** **Visualization**

**Use** **Case:** **Booking** **a** **Solar** **Panel** **or** **EV**
**Charger** **Installation** **Service**

> 1\. **User** **Scenario**: Alex, a homeowner in San Francisco, wants
> to install a rooftop solar setup. He uses a Beckn-enabled app to
> explore available service providers.
>
> 2\. **Discovery**: Alex searches for solar installation services and
> views offerings from multiple providers.
>
> 3\. **Order**: He selects a 2kW system offered by “SunPower
> Installers,” priced at \$85,000. He reviews the estimated quote and
> confirms the order.
>
> 4\. **Fulfillment**: The provider confirms the booking and dispatches
> an empaneled technician for installation.
>
> 5\. **Post-Fulfillment**: Alex rates the service and optionally
> provides feedback via a form link.

**Flow** **Diagrams**

**General** **Beckn** **Flow**

> Unset
>
> User → BAP → BPP (search) BPP → BAP (on_search)
>
> User → BAP → BPP (select → init → confirm) BPP → BAP (on_select →
> on_init → on_confirm) Fulfillment & rating follow
>
> ACK/NACK responses are part of every exchange.

**API** **Calls** **and** **Schema**

**search**

Sample fields from a user request:

> ● **Property** **Type**: Residential or Commercial
>
> ● **System** **Capacity**: e.g., 2KW, 5KW
>
> ● **City**: San Francisco
>
> Unset
>
> "message": { "intent": {
>
> "descriptor": {
>
> "name": "Residential Solar Installation" },
>
> "category": { "descriptor": {
>
> "code": "solar_panel_installation" }
>
> }, "fulfillment": {
>
> "stops": \[
>
> {
>
> "location": { "city": {
>
> "name": "San Francisco" }
>
> } }
>
> \], "tags": \[
>
> {
>
> "descriptor": { "code": "installation_context" }, "list": \[
>
> {
>
> "descriptor": { "code": "property_type" }, "value": "Residential"
>
> } \]
>
> } \]
>
> } }
>
> }

**on_search**

Returns catalog of providers and their items:

> Unset
>
> {
>
> "message": { "catalog": {
>
> "providers": \[ {
>
> "id": "sunpower_sf", "descriptor": {
>
> "name": "SunPower Installers",
>
> "short_desc": "Residential and commercial solar panel installation"
>
> }, "items": \[
>
> {
>
> "id": "sp-resi-001",
>
> "descriptor": { "name": "2kW Rooftop Solar Setup" },
>
> "price": {
>
> "estimated_value": "85000", "currency": "USD"
>
> }, "tags": \[
>
> {
>
> "descriptor": { "code": "system_specification" },
>
> "list": \[ {
>
> "descriptor": { "code": "system_capacity" },
>
> "value": "2KW" }
>
> \] }, {
>
> "descriptor": { "code": "installation_context" },
>
> "list": \[ {
>
> "descriptor": { "code": "property_type" },
>
> "value": "Residential" }
>
> \] }
>
> \] }
>
> \] }
>
> \] }
>
> } }

**Taxonomy** **and** **Layer** **2** **Configuration**

> **Path**
>
> Provider Tag
>
> Item Tag - Capacity
>
> Item Tag - Context
>
> Item Tag - Installer
>
> Item Tag - Financial
>
> **Code**

govt_subsidy_empaneled

> system_capacity
>
> property_type
>
> empaneled / certified
>
> financial_assistance
>
> **Sample** **Enum**
>
> true / false
>
> 2KW / 5KW

Residential / Commercial

> true / false
>
> true / false
>
> These tags and codes should be embedded within the Layer 2
> configuration file for network enforcement.

**Notes** **on** **Integrating** **With** **Your** **Software**

> 1\. **Webhook** **Setup**: Set up an endpoint to receive Beckn
> messages.
>
> 2\. **Message** **Parsing**: Use context.action to determine the
> message type.
>
> 3\. **ACK** **Handling**: Immediately respond with an ACK for all
> received requests.
>
> 4\. **Schema** **Mapping**: Convert internal models to Beckn schema
> format using descriptors, tags, and pricing.
