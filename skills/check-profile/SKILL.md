---
name: check-profile
description: Analyzes a device type or set of requirements and recommends the optimal ONVIF profile and add-on combination. Examples: /onvif-pro:check-profile IP camera H.265 PTZ, /onvif-pro:check-profile access control biometrics
---

# ONVIF Profile Check Skill

Analyzes the device type or requirements provided in `$ARGUMENTS` and recommends the optimal combination of ONVIF profiles and add-ons.

## Analysis Procedure

### Step 1: Classify the Device / Requirements
Extract the following from `$ARGUMENTS`:
- **Device type**: camera, NVR, access control panel, card reader, biometric device, analytics server, etc.
- **Key functions**: video streaming, edge recording, access control, analytics, secure communication, etc.
- **Technical requirements**: H.265, PTZ, two-way audio, biometrics, MQTT, TLS, etc.

### Step 2: Profile Mapping Logic

Map profiles according to the criteria below:

**Video-related:**
- Basic IP camera streaming → **Profile S** (required)
- H.265, HTTPS streaming, two-way audio, OSD → **Profile T** (replaces or supplements Profile S)
- Camera with built-in SD card / local storage recording → add **Profile G**
- AI analytics metadata (object/face/license plate) → add **Profile M**

**Access control-related:**
- Electronic door lock/unlock control → **Profile C** (required)
- Centralized credential/schedule/access rule management → add **Profile A**
- Card readers, biometric readers, keypads → **Profile D** (required)

**Secure communications:**
- Standardized encrypted TLS communication → **TLS Configuration Add-on** (use alongside a profile)

### Step 3: Output Format

Provide the recommendation in the following format:

```
## ONVIF Profile Analysis Result

**Input device/requirements:** [summary of input]

### ✅ Required Profiles
| Profile | Reason | Key Features |
|---------|--------|--------------|
| Profile X | ... | ... |

### 📦 Recommended Additional Profiles / Add-ons
| Profile/Add-on | Condition | Provided Features |
|----------------|-----------|-------------------|
| Profile Y | Add if [condition] | ... |

### ⚠️ Notes
- Conditional features: [list of conditional features]
- Version considerations: [relevant version notes]
- Conformance verification: [items to verify]

### 🏗️ Example System Configuration
[Description of how devices connect]

### 📌 Next Steps
- [Recommended actions]
```

### Step 4: Add Practical Advice
- Real-world integration considerations for the recommended profile combination
- How to verify manufacturer compatibility (onvif.org/conformant-products/)
- Testing and validation approach
