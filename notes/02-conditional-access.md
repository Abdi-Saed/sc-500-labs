# Lesson 2: Conditional Access in Microsoft Entra ID

## What Conditional Access is

Conditional Access (CA) is Entra's policy engine: **if** a sign-in matches the assignments, **then** apply the access controls. Lesson 1 proved who the user is. CA decides whether that sign-in gets access, and what it must satisfy first.

1. **Signals:** user or group, IP location, device, app and risk level.
2. **Decision:** block access, or grant it with requirements.
3. **Enforcement:** the user is blocked, prompted to meet the requirements, or let through.

CA runs after first-factor authentication, so it doesn't stop attackers from trying passwords.

## Anatomy of a policy

A policy needs at least a name, users or groups, target resources, and a grant or block control.

| Part | Question it answers | What you can choose |
| --- | --- | --- |
| Users | Who? | All users, groups, directory roles, guests and external users, workload identities |
| Target resources | What? | Cloud apps, user actions, authentication contexts |
| Network | From where? | IP ranges, countries or regions, trusted locations |
| Conditions | In what context? | Sign-in risk, user risk, device platforms, client apps, filter for devices |
| Grant | Allowed, and on what terms? | Block, or grant with requirements |
| Session | What can they do once in? | Limits applied after access is granted |

### Grant controls

- Multifactor authentication
- Authentication strength
- Device marked as compliant (through Intune)
- Microsoft Entra hybrid joined device
- Approved client app
- App protection policy
- Password change
- Terms of use

Several selected controls default to **require all** (AND). You can switch to **require one** (OR).

### Session controls

- **App enforced restrictions:** Exchange Online and SharePoint Online only; full or limited access based on the device.
- **Conditional Access App Control:** uses Defender for Cloud Apps to monitor sessions and block actions such as downloads.
- **Sign-in frequency:** how long before a user must sign in again to reach a resource.
- **Persistent browser session:** whether users stay signed in after closing the browser.

## How policies are evaluated

- There is no priority order. **All matching policies must be satisfied.**
- **Block always wins.**
- Inside a policy, assignments combine with AND.
- Grant controls are met first, then session controls apply.
- **Exclude beats include.**
- Role and group targeting is checked when a token is issued.
- Custom roles and roles scoped to an administrative unit can't be targeted.
- Managed identities aren't covered by Conditional Access.

## Common policies

| Requirement | Control | Extra license |
| --- | --- | --- |
| MFA for everyone | Grant: MFA or authentication strength | None beyond P1 |
| Phishing-resistant MFA for admins | Grant: authentication strength | None beyond P1 |
| Block legacy authentication | Client apps: Exchange ActiveSync and other clients, then Block | None beyond P1 |
| Block a country or region | Network: that location, then Block | None beyond P1 |
| Company-managed devices only | Grant: compliant device | Intune |
| Protect work data on personal phones (BYOD) | Grant: app protection policy (MAM without enrollment) | Intune |
| Block downloads of sensitive files | Session: Conditional Access App Control | Defender for Cloud Apps |
| Challenge risky sign-ins | Sign-in risk condition, then MFA | P2 |
| Fix likely compromised accounts | User risk condition, then risk remediation or password change | P2 |

**Sign-in risk** means this sign-in may not be the real user; MFA usually clears it. **User risk** means the account itself is likely compromised; it needs risk remediation or a secure password change.

## Deploying safely

- **Report-only mode:** evaluates a policy on real sign-ins without enforcing it. Doesn't work for policies scoped to user actions.
- **What If tool:** simulates a sign-in for a chosen user and conditions.
- **Sign-in logs:** show which policies applied to one sign-in.
- **Insights and reporting workbook:** shows policy impact over time; needs a Log Analytics workspace.

### Emergency access accounts

- At least two cloud-only accounts on the .onmicrosoft.com domain
- Global Administrator assigned as active permanent
- Protected with a passkey (FIDO2) or certificate-based authentication
- Excluded from policies that block or restrict sign-in
- Alert on every sign-in, and test at least every 90 days

## Licensing

- **Entra ID P1:** Conditional Access
- **Entra ID P2:** risk-based policies
- If licenses expire, existing policies stay in place but can't be updated.

## Sources

- [Build a Conditional Access policy](https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-conditional-access-policies)
- [Plan, implement, and administer Conditional Access](https://learn.microsoft.com/en-us/training/modules/plan-implement-administer-conditional-access)
- [Risk-based access policies](https://learn.microsoft.com/en-us/entra/id-protection/concept-identity-protection-policies)
- [Manage emergency access accounts](https://learn.microsoft.com/en-za/Entra/Identity/role-based-access-control/security-emergency-access)
