---
layout: default
title: "What is Jamf and why enterprises use it"
description: "Jamf and the Shared Signals Framework"
---

## What is jmaf

TL;DR
In enterprise security, Jamf acts as a "Transmitter" that monitors the real-time health (posture) of Apple devices. When a security "event" occurs—like a user disabling a passcode—Jamf transmits a Security Event Token (SET) to a "Receiver" (like Okta or Microsoft Entra ID). The Receiver then instantly terminates the user's active sessions, enforcing Zero Trust by blocking access until the device is repaired.


1. What is Jamf in the Access Control World? -> 

    While traditional access control deals with physical badges, Jamf manages Digital Access Control. It ensures that the "key" (the laptop or iPhone) being used to enter corporate systems is safe.

    Jamf is an Apple Enterprise Management platform. It sits at the intersection of IT (deployment/apps) and Security (encryption/compliance). For an enterprise, Jamf is the source of truth for whether a device is "Trusted" or "Untrusted."

2. Why Enterprises Use It -> 

    Enterprises deploy Jamf to solve the "Compliance Gap." Even if a user has a valid username and password, their device might be compromised. Jamf allows enterprises to enforce Conditional Access:

    Automated Remediation: If a device falls out of compliance, Jamf can automatically push a script to fix it.

    Zero-Touch Deployment: Devices are shipped from Apple directly to employees; Jamf configures them automatically the moment they touch Wi-Fi.

    Identity Sync: Using Jamf Connect, the local Mac password is kept in sync with the corporate identity (Okta/Google/Azure), preventing "shadow IT" and forgotten passwords.

3. How it is Built: The Architecture ->

    Jamf is built on a distributed cloud-native architecture that interacts directly with Apple’s native MDM (Mobile Device Management) framework.

    Jamf Pro Server: The control plane where admins define "Smart Groups" (dynamic groups that users join/leave based on their device status).

    The Binary Agent: A background process on macOS that executes deep-level security tasks (like checking for malware or specific system files) that standard MDM cannot reach.

    The Jamf Security Cloud: A global edge network that acts as a proxy, filtering traffic and detecting network-level threats (phishing, man-in-the-middle).

4. The Event Workflow: Transmitter and Receiver ->

    The modern "Zero Trust" magic happens through the Shared Signals Framework (SSF) and Continuous Access Evaluation (CAEP). Here is how the "Transmitter" and "Receiver" interact during a security event:

**The Transmitter:** Jamf Security Cloud
Jamf acts as the SSF Transmitter. Its job is to watch for specific Events:

**Detection:** A user disables FileVault (encryption) or installs a malicious profile.

**Trigger:** Jamf detects this state change immediately.

**Transmission:** Jamf generates a Security Event Token (SET)—a cryptographically signed JSON Web Token (JWT). This SET is "transmitted" via a webhook or an API stream to your identity provider.

**The Receiver:** Identity Provider (e.g., Okta, Microsoft Entra ID)
The Identity Provider acts as the SSF Receiver. It is "listening" for signals from Jamf:

**Ingestion:** The Receiver receives the SET from Jamf.

**Verification:** It verifies the signature to ensure the signal actually came from a trusted Jamf instance.

**Enforcement:** Based on the signal (e.g., risk_level: high), the Receiver takes action. It doesn't wait for the user to log in again; it kills the current session tokens for Slack, Email, and Salesforce instantly.

**Summary!**

Device Change: User makes an unsafe change.
Jamf (Transmitter): Sends a "Device Non-Compliant" signal.
IdP (Receiver): Receives the signal and revokes access.
The Result: The user is locked out of work apps within seconds until they turn security features back on.