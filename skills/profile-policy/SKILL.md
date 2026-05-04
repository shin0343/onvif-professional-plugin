---
name: profile-policy
description: Explains the ONVIF Profile Policy and Add-on concept, covering profile creation/modification/deprecation, Profile vs Add-on differences, Profile Q deprecation, TLS Add-on details, and the versioning model. Examples: /onvif-pro:profile-policy what is an add-on, /onvif-pro:profile-policy profile-q, /onvif-pro:profile-policy tls add-on, /onvif-pro:profile-policy proposing a new profile
---

# ONVIF Profile Policy & Add-on Concept Skill

Explains the ONVIF Profile Policy and Add-on concept for the topic in `$ARGUMENTS`.

---

## ONVIF Profile Policy

**Official document:** [ONVIF Profile Policy v3.5 (October 2024)](https://www.onvif.org/wp-content/uploads/2024/10/onvif-profile-policy-v3-5.pdf)

### Core Principles of a Profile

1. **Fixed feature set**: Once published, mandatory and conditional features of a profile cannot be changed — profiles are immutable
2. **Self-contained**: A single profile must fully cover its target use case without requiring other profiles
3. **Protocol-based**: Profiles define which WSDL services (SOAP/HTTP), RTSP behaviors, and data schemas are mandatory
4. **Conformance claims restricted to registered ONVIF members**: Only registered products may claim conformance
5. **Multiple profile support allowed**: A single device may implement multiple profiles (e.g., camera supporting Profile T + G + M)
6. **Version-tied conformance**: Conformance is tied to a specific firmware/software version; valid indefinitely for that version

### Core Principles of an Add-on

**Official definition source:** https://www.onvif.org/profiles/add-on/

1. **Single use case**: Consists of one or more features that solve exactly one use case
2. **Profile dependency**: Must always be used with at least one existing non-deprecated profile — cannot be used standalone
3. **Scope limit**: The add-on is not comprehensive enough on its own to qualify as a profile
4. **No duplication**: Features must not already be covered by any existing non-deprecated profile
5. **Restricted conditional/optional requirements**:
   - **Device**: Conditional requirements NOT permitted; optional requirements NOT permitted
   - **Client**: Optional requirements permitted only on a case-by-case basis
6. **Versioned**: New features can be added and existing ones removed as technology evolves (profiles cannot do this)
7. **Faster release cycle**: Specification and test tool development is faster than for profiles

---

## Profile / Add-on Comparison

| Aspect | Profile | Add-on |
|--------|---------|--------|
| Standalone use | ✅ Yes | ❌ No (requires a profile) |
| Feature scope | Comprehensive (full use case) | Single use case |
| Feature changes after release | ❌ Fixed (immutable) | ✅ Versioned (can add/remove) |
| Conditional requirements for devices | ✅ Allowed | ❌ Not allowed |
| Optional requirements for devices | ✅ Allowed | ❌ Not allowed |
| Optional requirements for clients | ✅ Allowed | Only case-by-case |
| Standalone conformance claim | ✅ Yes | ❌ No (requires a profile) |
| Release speed | Slower (full TC process) | Faster |
| Protocol definition | SOAP/WSDL + RTSP (where applicable) | SOAP/WSDL only (currently) |

---

## Profile Creation / Modification / Deprecation Process

### New Profile Proposal
1. **Market needs assessment**: Document unmet use cases not covered by existing profiles
2. **Technical proposal**: Identify required WSDL services, new schemas, RTSP behavior changes
3. **ONVIF Technical Committee (TC) review**: Feasibility and scope validation
4. **Specification draft**: Draft mandatory/conditional/optional feature matrix
5. **Test tool development**: ONVIF Device Test Tool routines for every mandatory feature
6. **Public review**: Member review period
7. **Approval and release**: Official version published with conformance deadline

### Profile Deprecation
- Profiles can be deprecated when their use case is fully covered by newer profiles
- After deprecation: new products may NOT claim conformance; existing certified products retain their status
- Add-ons may NOT be paired with deprecated profiles (must use non-deprecated profiles)

---

## Current Profiles (Active)

| Profile | Status | Target Use Case | Core Protocol |
|---------|--------|-----------------|---------------|
| **Profile S** | Active | IP video streaming (H.264) | SOAP + RTSP |
| **Profile T** | Active | Advanced video (H.265, HTTPS) | SOAP + RTSP (Media2) |
| **Profile G** | Active | Edge storage and retrieval | SOAP + RTSP (playback) |
| **Profile C** | Active | Door control | SOAP only |
| **Profile A** | Active | Access control configuration | SOAP only |
| **Profile D** | Active | Access control peripherals | SOAP only |
| **Profile M** | Active | Metadata & analytics | SOAP + RTSP (metadata) |
| ~~**Profile Q**~~ | **Deprecated** | Basic device config | (N/A — do not use) |

---

## Deprecated Profiles

### Profile Q (Deprecated April 1, 2022)
- **Original purpose**: Basic device configuration — network setup, discovery, authentication
- **Why deprecated**: Profile S/T now cover basic device configuration sufficiently via `device.wsdl` (GetCapabilities, GetNetworkInterfaces, GetUsers, etc.)
- **Impact**: New products may **not** claim Profile Q conformance; TLS Add-on must NOT be paired with Profile Q
- **Reference**: https://www.onvif.org/profiles/profile-q/

---

## Current Add-ons

### TLS Configuration Add-on (Only official Add-on as of 2025)

**Official page:** https://www.onvif.org/profiles/add-on/tls-configuration-add-on/
**Webinar:** https://www.onvif.org/wp-content/uploads/2024/04/onvif-add-on-webinar-20240425.pdf

| Version | Status | Deadline |
|---------|--------|----------|
| v1.0 | Active — conformance claims open | Submit by March 31, 2027 |
| v2.0 | Planned release (early 2027) | TBD |

**Purpose:** Standardize TLS communication configuration between ONVIF devices and clients — a gap not covered by any existing profile.

**Key capabilities:**
- TLS initial setup and configuration via `advancedsecurity.wsdl`
- Certificate upload, delete, and listing (`GetCertificates`, `UploadCertificateWithPrivateKey`)
- CRL (Certificate Revocation List) management
- TLS version negotiation (`GetSupportedTLSVersions`)
- Supports TLS 1.2 and TLS 1.3

**Required profiles:** At least one of S, T, G, C, A, D, or M (cannot be used with deprecated Profile Q).

**Protocol:** SOAP/HTTP control via `advancedsecurity.wsdl` → establishes HTTPS transport for all other ONVIF services.

---

## Profile Feature Overview Reference

**Document:** [ONVIF Profile Feature Overview v2.6 (April 2022)](https://www.onvif.org/wp-content/uploads/2022/04/onvif-profile-feature-overview.pdf)
- Side-by-side comparison of all profile features with M (Mandatory) / C (Conditional) designations
- Essential reference for understanding what each profile requires vs. what is optional

---

## Output Format

```
## ONVIF Profile Policy / Add-on Guidance

**Topic:** [input keyword]

### 📋 Relevant Policy
[Explanation of the applicable policy rule, with source reference]

### 🔌 Protocol Implications
[How this policy affects SOAP/WSDL service implementation or RTSP behavior]

### ⚖️ Practical Implications
[Actionable advice from a developer/integrator perspective]

### 🔗 Official References
- [Relevant document links with titles and dates]
```
