# Microsoft Entra ID → AWS SAML 2.0 SSO Integration

> ✅ **Complete** — Full end-to-end SAML SSO federation between Microsoft Entra ID and Amazon Web Services, built and tested in a simulated enterprise environment.

---

## The Scenario

A growing fintech company runs its internal tools and user identities on **Microsoft 365 and Entra ID**, but its engineering and DevOps teams also need access to **Amazon Web Services** for cloud infrastructure work.

The problem: managing separate AWS usernames and passwords for every engineer is a security risk. When someone leaves the company, IT has to remember to remove them from AWS separately. If they forget, that is an open door.

The solution: **federate AWS access through Entra ID using SAML 2.0 SSO**. Engineers log in once with their Microsoft credentials. AWS trusts Entra ID to vouch for them. No separate AWS passwords. No orphaned accounts. Full audit trail in one place.

This is **Zero Trust** in practice — *identity is the perimeter*.

---

## What Was Built

### Microsoft Entra ID Side (Identity Provider)

- Added **AWS Single-Account Access** as an Enterprise Application
- Configured SAML 2.0 settings:
  - **Entity ID:** `urn:amazon:webservices`
  - **Reply URL (ACS):** `https://signin.aws.amazon.com/saml`
- Mapped user attributes sent in the SAML assertion:
  - UPN as unique user identifier
  - Given name, surname, email as user profile
  - **Role ARN** mapping directly to the AWS IAM role the user assumes
  - Session duration set to 900 seconds (15-minute security control)
- Managed **Token Signing Certificate** lifecycle (RS256, expires 2029)
- Assigned users to the application for access control
- Exported **Federation Metadata XML** for AWS trust configuration

### Amazon Web Services Side (Service Provider)

- Registered **EntraID** as a trusted **SAML 2.0 Identity Provider** in AWS IAM
- Uploaded Federation Metadata XML to establish cryptographic trust
- Created **EntraID-ReadOnly** IAM Role with SAML 2.0 federation trust policy
- Mapped the full Role ARN back into Entra ID attribute claims

---

## How the SSO Flow Works

```
User opens AWS in their browser
         |
         v
Redirected to Microsoft Entra ID login
         |
         v
User authenticates (password + MFA if required)
         |
         v
Entra ID generates a signed SAML assertion
(contains user identity + assigned AWS role ARN)
         |
         v
Assertion posted to AWS ACS URL
         |
         v
AWS validates signature against EntraID metadata
         |
         v
User assumes EntraID-ReadOnly IAM Role
         |
         v
AWS Console opens -- no separate AWS credentials used
```

---

## The Result

- User authenticated through Entra ID and landed directly in the AWS Console
- Role confirmed in AWS: `EntraID-ReadOnly/rahat.it.lab@...`
- Zero separate AWS credentials used
- Full sign-in audit trail maintained in Entra ID logs

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Identity Provider (IdP) | Microsoft Entra ID (Azure AD) |
| Service Provider (SP) | Amazon Web Services IAM |
| Federation Protocol | SAML 2.0 |
| Role Mapping | IAM Role ARN via SAML attribute claim |
| Certificate | RS256 Token Signing Certificate |
| Access Policy | AWS ReadOnlyAccess (managed) |

---

## Key Concepts Demonstrated

**SAML 2.0 Federation** - industry standard for enterprise SSO between cloud platforms

**Zero Trust Identity** - no implicit trust, every access request verified through the central identity provider

**Least Privilege Access** - ReadOnlyAccess policy grants minimum required permissions

**Certificate Lifecycle Management** - token signing certificate tracked with expiry monitoring

**Attribute-Based Access Control** - AWS role assignment driven by Entra ID attribute claims

**Audit Trail** - all authentication events logged in Entra ID sign-in logs

---

## Screenshots - Full Build Walkthrough

### Entra ID Configuration


**Step 2 - SAML Setup Initiated**
![SAML Setup](screenshots/02-entra-saml-setup-begin.png)

**Step 3 - Basic SAML Configuration**
![Basic SAML Config](screenshots/03-entra-basic-saml-config.png)

**Step 4 - Entity ID and Reply URL Configured**
![Entity ID and Reply URL](screenshots/04-entra-entity-id-reply-url.png)

**Step 5 - Attribute Claims Mapping**
![Attribute Claims](screenshots/05-entra-attribute-claims-mapping.png)

**Step 6 - Token Signing Certificate**
![Token Certificate](screenshots/06-entra-token-signing-certificate.png)

**Step 7 - SAML Configuration Overview**
![SAML Overview](screenshots/07-entra-saml-overview-complete.png)

**Step 8 - Users and Groups Section**
![Users Groups](screenshots/08-entra-users-groups-section.png)

**Step 9 - User Assigned to Application**
![User Assigned](screenshots/09-entra-user-assigned-to-app.png)

**Step 10 - Assignment Confirmed**
![Assignment Confirmed](screenshots/10-entra-assignment-confirmed.png)

**Step 11 - Final SAML Configuration**
![Final Config](screenshots/11-entra-saml-config-final.png)

### AWS IAM Configuration

**Step 12 - AWS IAM Dashboard**
![AWS IAM](screenshots/12-aws-iam-dashboard.png)

**Step 13 - EntraID Identity Provider Created in AWS**
![Identity Provider Created](screenshots/13-aws-identity-provider-entraid-created.png)

**Step 14 - Create Role with SAML Federation**
![Create Role](screenshots/14-aws-create-role-saml-federation.png)

**Step 15 - ReadOnlyAccess Policy Selected**
![ReadOnly Policy](screenshots/15-aws-readonlyaccess-policy-selected.png)

**Step 16 - Role Name and Review**
![Role Review](screenshots/16-aws-role-name-review.png)

**Step 17 - EntraID-ReadOnly Role Created**
![Role Created](screenshots/17-aws-entraid-readonly-role-created.png)

### Final Configuration and Proof

**Step 18 - Role ARN Mapped Back in Entra ID**
![Role ARN Mapped](screenshots/18-entra-role-arn-claim-configured.png)

**Step 19 - SSO Success - AWS Console via Entra ID Login**
![SSO Success](screenshots/19-aws-console-sso-login-success.png)

**Step 20 - Entra ID SSO Test Panel**
![SSO Test](screenshots/20-entra-sso-test-panel.png)

---

## What is Next

- Add Conditional Access policy to enforce MFA for AWS access
- Expand to multiple AWS roles based on Entra ID group membership
- Automate user assignment via PowerShell and Microsoft Graph API
- Add AWS CloudTrail evidence of federated sign-in events

---

## Author

**Md Rahat Islam Anik**
Cloud Computing & Network Administration - George Brown College - May 2026
[linkedin.com/in/rahatislamanik](https://linkedin.com/in/rahatislamanik) - [github.com/rahatislamanik-spec](https://github.com/rahatislamanik-spec)
