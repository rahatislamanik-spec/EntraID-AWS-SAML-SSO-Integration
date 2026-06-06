# Entra ID to AWS SAML Evidence Map

This file maps the screenshot evidence to the SAML federation claims in the README.

## Evidence Summary

| Evidence Area | Status | Screenshot Evidence | Boundary |
|---|---|---|---|
| Entra tenant context | Shown | `screenshots/02-entra-saml-setup-begin.png` | Contains tenant/admin identifiers; mask recommended before wider promotion |
| Enterprise application discovery | Shown | `screenshots/03-entra-basic-saml-config.png`, `screenshots/04-entra-entity-id-reply-url.png` | Filenames are historical and may not perfectly describe the screen |
| AWS Single-Account Access application | Shown | `screenshots/05-entra-attribute-claims-mapping.png`, `screenshots/07-entra-saml-overview-complete.png` | Shows the Entra enterprise application workflow |
| User assignment | Shown | `screenshots/06-entra-token-signing-certificate.png`, `screenshots/08-entra-users-groups-section.png`, `screenshots/10-entra-assignment-confirmed.png` | User names are visible in one screenshot |
| SAML claims and role mapping | Shown | `screenshots/07-entra-saml-overview-complete.png`, `screenshots/18-entra-role-arn-claim-configured.png` | Shows claim configuration; does not show every final ARN value in full |
| AWS IAM role creation | Shown | `screenshots/14-aws-create-role-saml-federation.png`, `screenshots/15-aws-readonlyaccess-policy-selected.png`, `screenshots/16-aws-role-name-review.png`, `screenshots/17-aws-entraid-readonly-role-created.png` | AWS account ID is partially visible in one screenshot |
| Federated console access | Shown | `screenshots/19-aws-console-sso-login-success.png` | Shows AWS console access; account/user header is masked |
| Entra SSO test panel | Shown | `screenshots/20-entra-sso-test-panel.png` | Shows the test interface, not a successful test result by itself |
| Conditional Access enforcement | Not shown | None | Future validation item |
| Deprovisioning behavior | Not shown | None | Future validation item |
| AWS CloudTrail sign-in event | Not shown | None | Future validation item |

## Reviewer Notes

- The implementation is documented as AWS Single-Account Access with IAM SAML role federation.
- Some AWS screens reference IAM Identity Center because AWS displays related identity guidance in the console; this repo does not claim a full IAM Identity Center multi-account deployment.
- Federation Metadata XML is intentionally excluded because it can contain tenant-specific URLs, certificates, and directory identifiers.
- Screenshots are useful evidence, but several filenames were created during the build process and should be treated as historical labels rather than precise technical titles.
