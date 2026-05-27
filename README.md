# Entra ID → AWS SAML SSO Integration

> 🚧 **Build in Progress** — Core integration complete, documentation and screenshots being added.

---

## What This Is

This project connects **Microsoft Entra ID** (Azure Active Directory) to **Amazon Web Services** using **SAML 2.0 Single Sign-On**.

In plain English: instead of users having a separate username and password for AWS, they log in once through Microsoft Entra ID — and AWS automatically lets them in with the right permissions. One identity. One login. No extra credentials.

This is a real-world implementation of **Zero Trust** — the idea that *identity is the perimeter*.

---

## Why This Matters

In most organizations, users juggle multiple sets of credentials across cloud platforms. This creates security risks:

- Forgotten or weak passwords on secondary platforms
- No central place to revoke access when someone leaves
- No audit trail of who accessed what and when

By federating AWS access through Entra ID:

- ✅ One place to manage all user access
- ✅ Instant deprovisioning — remove access in Entra ID, access is revoked everywhere
- ✅ Full audit trail in Entra ID sign-in logs
- ✅ MFA and Conditional Access policies apply automatically to AWS too

---

## What Was Built

### Microsoft Entra ID Side (Identity Provider)
- Added **AWS Single-Account Access** as an Enterprise Application in Entra ID
- Configured SAML Basic Settings:
  - **Entity ID:** `urn:amazon:webservices`
  - **Reply URL (ACS):** `https://signin.aws.amazon.com/saml`
- Mapped user attributes:
  - UPN → unique user identifier
  - Given name, surname, email → user profile
  - Assigned roles → AWS IAM role mapping
  - Session duration → 900 seconds (15 min security control)
- Generated and managed **Token Signing Certificate** (expires 2029)
- Downloaded **Federation Metadata XML** for AWS trust configuration
- Assigned users to the application for access control

### Amazon Web Services Side (Service Provider)
- Registered **EntraID** as a trusted **SAML 2.0 Identity Provider** in AWS IAM
- Uploaded Federation Metadata XML to establish trust relationship
- Created **EntraID-ReadOnly** IAM Role with:
  - SAML 2.0 federation trust policy pointing to EntraID provider
  - ReadOnlyAccess permissions policy attached
- Validated role ARN mapping between Entra ID attribute claims and AWS role

---

## How It Works — The Flow

```
User attempts to access AWS
        ↓
Entra ID authenticates the user (MFA if required)
        ↓
Entra ID generates a signed SAML assertion
        ↓
SAML assertion sent to AWS ACS URL
        ↓
AWS validates the assertion against EntraID IdP metadata
        ↓
User assumes EntraID-ReadOnly IAM Role
        ↓
User lands in AWS Console — no separate credentials needed
```

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Identity Provider | Microsoft Entra ID (Azure AD) |
| Service Provider | Amazon Web Services IAM |
| Federation Protocol | SAML 2.0 |
| Attribute Mapping | UPN, Roles, Email, Name |
| Access Control | IAM Role with Trust Policy |
| Certificate | RS256 Token Signing Certificate |

---

## What's Coming

- [ ] Screenshots of full configuration walkthrough
- [ ] Architecture diagram (Entra ID → SAML → AWS)
- [ ] PowerShell script to automate user assignment to the Enterprise App
- [ ] Conditional Access policy applied to the AWS SSO application
- [ ] Expansion to AWS IAM Identity Center for multi-account SSO

---

## Author

**Md Rahat Islam Anik**  
Cloud Computing & Network Administration · George Brown College · May 2026  
[linkedin.com/in/rahatislamanik](https://linkedin.com/in/rahatislamanik) · [github.com/rahatislamanik-spec](https://github.com/rahatislamanik-spec)
