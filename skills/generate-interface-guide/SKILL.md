---
name: generate-interface-guide
description: Generates an ONVIF Interface Guide in XML (DocBook v5.x) format for conformance submission, per Interface Guide Specification v1.1.2 (April 2023). Collects product info and produces a complete, submission-ready document. Examples: /onvif-pro:generate-interface-guide Axis P3245-V Profile T+G firmware 10.12, /onvif-pro:generate-interface-guide Hanwha QNV-8080R Profile S
---

# ONVIF Interface Guide Generator Skill

Generates a complete ONVIF Interface Guide XML document for conformance submission, based on the product information in `$ARGUMENTS`.

**Specification:** ONVIF Interface Guide Specification v1.1.2 (April 2023)
**Reference:** https://www.onvif.org/wp-content/uploads/2023/04/ONVIF-Interface-Guide-Specification-v1-1-2.pdf

> **Important:** As of April 19, 2023, only v1.1.2 is accepted for DoC submissions. The guide must be in **XML format conforming to DocBook v5.x** standard. The XML template is available in the ONVIF Member Portal under "Resources": https://members.onvif.org/

---

## What the Interface Guide Is

The ONVIF Interface Guide provides the initial steps required to operate an ONVIF device or client using the ONVIF API. It accompanies the Declaration of Conformance (DoC) and Feature List in the conformance submission package. Intended audience: installers, system integrators, architects, engineers, end users.

**The guide does NOT describe the product's full feature set** — it describes specifically how to connect to and use the product's ONVIF interface.

---

## Information Gathering

If `$ARGUMENTS` is incomplete, ask the user for the following information:

### Required Information
1. **Product type**: `device` | `client` | `device/client`
2. **Company name** and **product name** (must exactly match the DoC)
3. **Firmware/software version number** (must exactly match the DoC and Feature List)
4. **Supported ONVIF Profiles** (e.g., Profile T, Profile G, Profile M)
5. **Default network settings**:
   - Default IP address (or "DHCP only")
   - DHCP behavior (enabled/disabled by default)
6. **Default login credentials**:
   - Default username
   - Default password (or "must be set at first login")
   - Default ONVIF service access URL (e.g., `http://<ip>/onvif/device_service`)
7. **Technical support contact**:
   - Company name, street address, city, country
   - Technical support website URL
8. **How to enable ONVIF** (enabled by default? if not, navigation path)
9. **Installation basics**: power source, network connection method

### Optional Information
- Company logo URL/path
- Regional support contacts
- Technical support email / phone
- Whether a family of products DoC is used (if yes: all product names in the family)

---

## Document Generation Procedure

### Step 1: Parse `$ARGUMENTS`
Extract as much product information as possible from the arguments. For missing required fields, ask the user before proceeding.

### Step 2: Generate DocBook v5.x XML

Generate a complete XML document using the DocBook v5.x structure. The document must include all mandatory sections per spec v1.1.2.

**Standard template structure:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<book xmlns="http://docbook.org/ns/docbook"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      version="5.0"
      xml:lang="en">

  <info>
    <title>ONVIF Interface Guide</title>
    <subtitle>[Product Name] [Version Number]</subtitle>
    <author>
      <orgname>[Company Name]</orgname>
    </author>
    <pubdate>[YYYY-MM-DD]</pubdate>
  </info>

  <!-- Section 1: Overview -->
  <chapter xml:id="overview">
    <title>Overview</title>
    <para>
      The purpose of this guide is to provide the initial steps required to operate this
      product using the ONVIF API. For more information on ONVIF, refer to
      <link xlink:href="http://www.onvif.org">http://www.onvif.org</link>.
    </para>
    <para>
      This ONVIF Interface Guide is issued by [Company Name] which is solely responsible
      for declared conformance and the information in this guide. Conformity is valid ONLY
      for the ONVIF product identified when used in a manner consistent with the intent of
      the referenced documents.
    </para>

    <!-- 5.1.1 Product Information -->
    <section xml:id="product-information">
      <title>Product Information</title>
      <para><emphasis role="bold">Product Type:</emphasis> [device | client | device/client]</para>
      <para><emphasis role="bold">Product Name:</emphasis> [Product Name]</para>
      <para><emphasis role="bold">Version Number:</emphasis> [Firmware/SW Version]</para>
      <!-- If family DoC: list all product names -->
    </section>

    <!-- 5.1.2 Supported ONVIF Profiles -->
    <section xml:id="supported-profiles">
      <title>Supported ONVIF Profiles</title>
      <itemizedlist>
        <listitem><para>[Profile S / T / G / C / A / D / M — list each]</para></listitem>
      </itemizedlist>
    </section>

    <!-- 5.1.3 Support Information -->
    <section xml:id="support-information">
      <title>Support Information</title>
      <address>
        <orgname>[Company Name]</orgname>
        <street>[Street Name and Number]</street>
        <city>[City]</city>
        <country>[Country]</country>
      </address>
      <para>
        Technical Support: <link xlink:href="[Support URL]">[Support URL]</link>
      </para>
    </section>
  </chapter>

  <!-- Section 2: Prerequisites -->
  <chapter xml:id="prerequisites">
    <title>Prerequisites</title>
    <para>[Hardware/OS/browser/network requirements to interact with the product]</para>
    <itemizedlist>
      <listitem><para>Network: Ethernet connection (100 Mbps or higher recommended)</para></listitem>
      <listitem><para>Client: Web browser or ONVIF-compliant VMS/NVR software</para></listitem>
    </itemizedlist>
  </chapter>

  <!-- Section 3: Installation -->
  <chapter xml:id="installation">
    <title>Installation</title>
    <section>
      <title>Power Source</title>
      <para>[PoE IEEE 802.3af/at/bt | DC adapter | specify]</para>
    </section>
    <section>
      <title>Network Connection</title>
      <para>[Connect RJ-45 Ethernet cable to the device's LAN port]</para>
    </section>
  </chapter>

  <!-- Section 4: Default Network Settings -->
  <chapter xml:id="network-settings">
    <title>Default Network Settings</title>
    <para><emphasis role="bold">Default IP Address:</emphasis> [e.g., 192.168.1.64 | DHCP]</para>
    <para><emphasis role="bold">DHCP:</emphasis> [Enabled by default | Disabled — static IP required]</para>
    <para>
      To discover the assigned IP address, use WS-Discovery or the manufacturer's
      device management tool.
    </para>
  </chapter>

  <!-- Section 5: Default Login -->
  <chapter xml:id="default-login">
    <title>Default Login</title>
    <para><emphasis role="bold">Default Username:</emphasis> [admin | root | specify]</para>
    <para><emphasis role="bold">Default Password:</emphasis> [specify | must be set on first login]</para>
    <para>
      <emphasis role="bold">ONVIF Service URL:</emphasis>
      <literal>http://&lt;device-ip&gt;/onvif/device_service</literal>
    </para>
  </chapter>

  <!-- Section 6: Local Configuration -->
  <chapter xml:id="local-configuration">
    <title>Local Configuration</title>
    <para>
      Access the device configuration page by navigating to
      <literal>http://&lt;device-ip&gt;</literal> in a web browser and logging in with
      the device credentials.
    </para>
    <para>Key settings locations:</para>
    <itemizedlist>
      <listitem><para>Network settings: [menu path]</para></listitem>
      <listitem><para>User management: [menu path]</para></listitem>
      <listitem><para>ONVIF settings: [menu path]</para></listitem>
    </itemizedlist>
  </chapter>

  <!-- Section 7: Enabling ONVIF -->
  <chapter xml:id="enabling-onvif">
    <title>Enabling ONVIF</title>
    <para>
      [The ONVIF interface is enabled by default. | The ONVIF interface must be enabled
      before use. Navigate to [Settings → Network → ONVIF] and enable the ONVIF service.]
    </para>
  </chapter>

  <!-- Section 8: Querying Capabilities -->
  <chapter xml:id="querying-capabilities">
    <title>Querying Capabilities</title>
    <section>
      <title>Discovery</title>
      <para>
        The device supports WS-Discovery. Send a WS-Discovery Probe message to the
        multicast address <literal>239.255.255.250:3702</literal> (UDP). The device
        responds with its ONVIF service address (XAddrs).
      </para>
      <para>Alternatively, add the device directly by entering its IP address in your
      ONVIF client.</para>
    </section>
    <section>
      <title>Get Capabilities</title>
      <para>
        Send a SOAP <literal>GetCapabilities</literal> or <literal>GetServices</literal>
        request to <literal>http://&lt;device-ip&gt;/onvif/device_service</literal>
        to retrieve the list of supported services and their endpoint addresses.
      </para>
      <programlisting language="xml"><![CDATA[
POST /onvif/device_service HTTP/1.1
Host: <device-ip>
Content-Type: application/soap+xml; charset=utf-8

<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
            xmlns:tds="http://www.onvif.org/ver10/device/wsdl">
  <s:Body>
    <tds:GetCapabilities>
      <tds:Category>All</tds:Category>
    </tds:GetCapabilities>
  </s:Body>
</s:Envelope>
      ]]></programlisting>
    </section>
  </chapter>

</book>
```

### Step 3: Populate the Template

Replace all `[placeholder]` values with the actual product information from `$ARGUMENTS` and any additional information collected from the user.

### Step 4: Output

1. Present the complete XML document
2. Remind the user:
   - The XML must be validated against DocBook v5.x schema before submission
   - Use the official ONVIF template from Member Portal (https://members.onvif.org/) as the base
   - The Interface Guide must be submitted together with the DoC and Feature List via Member Tools
   - Guide must be made available on the manufacturer's website or in product documentation

---

## Output Format

```
## ONVIF Interface Guide — [Product Name] [Version]

### ℹ️ Submission Requirements
- Format: XML (DocBook v5.x)
- Accepted spec version: v1.1.2 (April 2023) only
- Submit via: https://members.onvif.org/ (Member Tools)
- Must accompany: Declaration of Conformance (DoC) + Feature List

### 📄 Generated XML Document
[Complete DocBook v5.x XML]

### ✅ Mandatory Sections Included
- [x] Overview (standard text + Product Info + Supported Profiles + Support Info)
- [x] Prerequisites
- [x] Installation
- [x] Default Network Settings
- [x] Default Login
- [x] Local Configuration
- [x] Enabling ONVIF
- [x] Querying Capabilities (Discovery + GetCapabilities/GetServices)

### ⚠️ Before Submission
1. Validate XML against DocBook v5.x schema (https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=docbook)
2. Verify Product Name and Version Number match DoC and Feature List exactly
3. If product family: confirm all product names are listed
4. Make guide available on manufacturer website
5. Use official ONVIF XML template from Member Portal as base document
```
