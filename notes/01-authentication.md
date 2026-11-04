# Lesson 1: Authentication in Microsoft Entra ID

## Authentication and authorization

- **Authentication (AuthN):** answers "who are you?" and proves an identity.
- **Authorization (AuthZ):** answers "what are you allowed to do?" This is handled by RBAC, Entra roles, and Conditional Access session controls.

Microsoft Entra ID is the identity provider. It authenticates users and issues tokens, and access decisions are based on those tokens.

## Authentication factors

- **Something you know:** a password or PIN.
- **Something you have:** a registered device, security key, or authenticator app.
- **Something you are:** a biometric, such as a fingerprint or face scan.

Multifactor authentication (MFA) requires two or more factors from different categories.

## Methods from weakest to strongest

| Tier | Methods | Notes |
| --- | --- | --- |
| Weak | Password only | Vulnerable to phishing, password spraying, and credential stuffing |
| Better | SMS, voice call, OATH tokens, Authenticator push | SMS and voice can be SIM-swapped and phished, so treat them as a last resort |
| Stronger | Authenticator with number matching, passwordless phone sign-in | Number matching defeats MFA fatigue attacks |
| Phishing-resistant | FIDO2 security keys and passkeys, Windows Hello for Business, certificate-based authentication (CBA) | A fake sign-in page cannot replay them |

Passwordless does not always mean phishing-resistant. Number matching stops MFA fatigue, but a user can still approve a sign-in started from an adversary-in-the-middle page.

## Key features

- **Authentication methods policy:** turns methods on or off and sets which users or groups can use each one.
- **Temporary Access Pass (TAP):** a time-limited passcode issued by an admin for onboarding or recovery. A new user can register a passkey or Authenticator without ever having a password.
- **Self-service password reset (SSPR):** lets users reset their password after proving their identity with registered methods. Hybrid environments need password writeback so the change reaches on-premises AD.
- **Microsoft Entra Password Protection:** blocks weak passwords using Microsoft's global banned list and a custom banned list. It can extend to on-premises AD domain controllers.
- **Authentication strengths:** a Conditional Access control that specifies which MFA methods count. A basic "require MFA" control accepts any MFA method.
- **Security defaults:** free baseline protection that requires MFA registration and blocks legacy authentication. Turn it off once you use Conditional Access.

## Authentication strength rules

To satisfy authentication strength, a method must be:

1. Allowed in the authentication methods policy or legacy MFA service settings, with the user enabled for it.
2. Registered by the user, either before sign-in or as part of the access request.
3. Included in the required authentication strength.

If several authentication strength policies apply, the user must satisfy all of them. If the user has already signed in with a method that meets the required strength, Entra ID does not prompt again.

## Hybrid identity sign-in

| Option | How it works | When to choose it |
| --- | --- | --- |
| Password hash sync (PHS) | A hash of the password hash is synced to the cloud, and Entra authenticates the user | Simplest option, and it enables leaked credential detection in ID Protection |
| Pass-through authentication (PTA) | Entra forwards the password to an on-premises agent for validation | Policy requires passwords to be validated on-premises |
| Federation (AD FS) | Authentication is fully redirected on-premises | Most complex option, and Microsoft encourages moving away from it |

If a question asks for the least administrative effort or leaked credential detection, choose PHS.

## Legacy authentication

POP, IMAP, and SMTP basic authentication cannot prompt for MFA, so attackers use them for password spraying. Block legacy authentication with a Conditional Access policy, or use security defaults if you do not have Entra ID P1.

## Sources

- [Explore Microsoft Entra ID authentication methods](https://learn.microsoft.com/en-us/training/modules/manage-implement-authentication-methods/2-explore-authentication-methods)
- [How Conditional Access authentication strength works](https://learn.microsoft.com/entra/identity/authentication/concept-authentication-strength-how-it-works)
