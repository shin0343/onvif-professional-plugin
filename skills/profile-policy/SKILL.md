---
name: profile-policy
description: Explains the ONVIF Profile Policy and the Add-on concept. Covers the creation, modification, and deprecation process for profiles and add-ons, the difference between an Add-on and a Profile, and the Profile Q deprecation history. Examples: /onvif-pro:profile-policy what is an add-on, /onvif-pro:profile-policy profile-q, /onvif-pro:profile-policy proposing a new profile
---

# ONVIF Profile Policy & Add-on Concept Skill

Explains the ONVIF Profile Policy and Add-on concept for the topic provided in `$ARGUMENTS`.

## ONVIF Profile Policy

**Official document:** [ONVIF Profile Policy v3.5 (October 2024)](https://www.onvif.org/wp-content/uploads/2024/10/onvif-profile-policy-v3-5.pdf)

### Core Principles of a Profile
- **Fixed feature set**: Once published, the mandatory and conditional features of a profile cannot be changed
- **Self-contained**: A single profile must fully cover its target use case on its own
- **Conformance claims restricted to registered products**: Only registered products may claim ONVIF conformance
- **Multiple profile support allowed**: A single device may implement multiple profiles (e.g., camera supporting Profile T + Profile G)

### Core Principles of an Add-on (Official Definition)
Source: https://www.onvif.org/profiles/add-on/

1. **Single use case**: Consists of one or more features that solve exactly one use case
2. **Profile dependency**: Must always be used with at least one existing non-deprecated profile (cannot stand alone)
3. **Cannot become a profile**: The add-on is not comprehensive enough on its own to qualify as a profile
4. **No duplication**: Features in an add-on must not already be covered by any existing non-deprecated profile
5. **Restricted conditional/optional requirements**:
   - Device: Conditional requirements not permitted; optional requirements not permitted
   - Client: Optional requirements allowed only on a case-by-case basis
6. **Versioned**: New features can be added and existing ones removed to adapt to changing technology (profiles cannot do this)
7. **Faster release cycle**: Specification and test tool development is faster than for profiles

### Profile / Add-on Creation Process
1. **Proposal**: Document market requirements and use cases
2. **Review**: ONVIF Technical Committee review
3. **Development**: Draft specification
4. **Test tool development**: Build conformance test tools
5. **Approval and release**: Publish official version
6. **Deprecation**: Gradually phase out profiles that are no longer needed

## Deprecated Profiles

### Profile Q (Deprecated)
- **Deprecation date**: April 1, 2022
- **Original purpose**: Basic device configuration (basic network setup, authentication, etc.)
- **Reason deprecated**: Profile S/T now covers this use case sufficiently
- **Important**: New products may no longer claim Profile Q conformance
- **Reference**: https://www.onvif.org/profiles/profile-q/

## Add-on vs Profile Comparison

| Aspect | Profile | Add-on |
|--------|---------|--------|
| Standalone use | ✅ Yes | ❌ No (requires a profile) |
| Feature scope | Comprehensive | Single use case |
| Version changes | ❌ Fixed | ✅ Versioned |
| Conditional requirements | Allowed | Not allowed for devices |
| Release speed | Slower | Faster |
| Standalone conformance claim | ✅ Yes | ❌ No (requires a profile) |

## Current Add-ons

### TLS Configuration Add-on (Currently the only official Add-on)
- **Purpose**: Standardize TLS communication configuration between ONVIF devices and clients
- **Version 1.0**: Conformance submission deadline — March 31, 2027
- **Version 2.0**: Planned release (early 2027)
- **Required profiles**: At least one of S, T, G, C, A, D, or M
- **Official page**: https://www.onvif.org/profiles/add-on/tls-configuration-add-on/
- **Webinar materials**: https://www.onvif.org/wp-content/uploads/2024/04/onvif-add-on-webinar-20240425.pdf

## Profile Feature Overview

Official ONVIF profile feature comparison table:
- **Document**: [ONVIF Profile Feature Overview v2.6 (April 2022)](https://www.onvif.org/wp-content/uploads/2022/04/onvif-profile-feature-overview.pdf)
- **Contents**: Side-by-side comparison of features across all profiles with Mandatory (M) / Conditional (C) designations at a glance

## Output Format

```
## ONVIF Profile Policy / Add-on Guidance

**Topic:** [input keyword]

### 📋 Relevant Policy
[Explanation of the applicable policy]

### ⚖️ Practical Implications
[Actionable advice from a developer/integrator perspective]

### 🔗 Official References
- [Relevant document links]
```
