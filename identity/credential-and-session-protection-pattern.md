# Credential and session compromise response

A working pattern for the record a program produces the moment a credential exposure is confirmed, opinionated about which control actually stops the attacker and which one just feels like doing something.

A confirmed exposure starts the same way in almost every program: someone resets the password.
It is the fastest visible action available, it closes a ticket, and against an infostealer it does almost nothing, because the same infected host that gave up the password also gave up the live session cookie, the saved TOTP seed sitting in a browser extension, and every other credential stored on that machine.
Rotating the password leaves the attacker holding a session that was never invalidated and a second factor that was never really a second factor.
The structural failure is treating a credential exposure as a password problem when the artifact that actually survived rotation is the one doing the damage.

This document is the record a program produces when a credential exposure is CONFIRMED, meaning a breach-corpus hit, a stealer-log notification, or a phishing report has landed, not a suspicion still being triaged.
It is bounded the way the [access review pattern](access-review-pattern.md) is bounded: one artifact, one shape, produced every time the trigger fires, so the response does not get reinvented under time pressure by whoever is on call.

## What this covers

The response record for a single confirmed credential or session exposure, tied to one identity and, where the exposure came from an infected endpoint, one host.
Not the malware analysis, not the broader incident if the exposure turns out to be part of one, and not the detection pipeline that produced the alert.
Where the exposure escalates past one identity, it hands off to the [evidence-readiness register](../incident-response/evidence-readiness-register.md) and the [materiality determination record](../incident-response/materiality-determination-record.md), which is where population and notification questions live.
This is the artifact that runs before that question is even asked, on every confirmed hit, whether or not it ever grows into an incident.

## Rotation is not the primary control

Rotating the password answers a question the attacker stopped needing answered the moment the host was infected.
An infostealer that harvests credentials from a browser store harvests the session cookies from that same store in the same pass, and a live session cookie authenticates without the password at all.
It also harvests any TOTP seed sitting in a browser-extension authenticator and any password saved in the browser's own vault, both of which survive a password change untouched.

So a response that rotates the password and stops has left the attacker with a working session and a working second factor, and has produced a closed ticket that looks like remediation.
The primary controls are forced session and refresh-token revocation across every application the identity touches, and MFA re-enrollment on a clean device, because those are the two artifacts a password rotation does not invalidate.
Password rotation still belongs in the record, but it belongs after those two, as cleanup rather than as the control the response depends on.

## A browser-extension authenticator is not a second factor

The pitch for a browser-based authenticator or password manager is convenience: the second factor lives next to the password, one click away.
That proximity is also why it fails the moment the browser is the thing that got infected.
An infostealer that reads a browser's credential store reads its extension storage in the same operation, and a TOTP seed sitting in that storage is retrievable as plain text with the same tooling that lifts the passwords, no cracking required.

A second factor is supposed to be independent of the channel the first factor travels through, so that compromising one does not compromise the other.
A browser extension authenticator shares the channel, the process, and in most infection paths the exact file the credential itself was lifted from, so it is not a second factor in any sense that survives contact with a stealer.
Treat any code, seed, or saved password recovered from a browser-extension store as compromised on the same infection as the password beside it, and treat a hardware key or an out-of-band authenticator app as the only MFA type this record can mark as unaffected by an endpoint compromise.

## What is presumed stolen, and why the presumption resolves against you

The response record does not ask what was exfiltrated from the infected host.
It asks what the host could produce, and it presumes all of it stolen unless egress logging proves a narrower scope, which is the same rule the [evidence-readiness register](../incident-response/evidence-readiness-register.md) applies to a database instead of an endpoint.
An unanswerable "was it actually sent to the attacker" resolves against the responder, at the full population of what that host held and what it could reach, not at the smaller population that would be convenient to notify.

For a single infected endpoint that population is concrete and larger than most people expect on first pass: every credential saved in every browser profile on the machine, every session cookie live at the moment of infection, every TOTP seed stored in a browser extension, any local password-vault database sitting in a documents or desktop folder, and any file the malware's harvester module judged valuable enough to grab whole.
Most stealer infections leave no persistence and no other trace, so there is frequently no forensic path to a narrower answer, which is exactly the condition under which the presumption does the work instead of the evidence.
Scope the response to that full population on day one, because scoping down later requires proof that does not usually exist, and scoping up later means re-running a response that already told people it was finished.

## What the record must contain, and in what order

The order is the content, in the same sense it is for the [evidence-readiness register](../incident-response/evidence-readiness-register.md)'s three-call sequence: getting the order backwards produces a record that looks complete and leaves the attacker with something live.

1. **Identity and trigger.** Which identity, and which of the three trigger types fired: breach-corpus hit, stealer-log notification, or phishing report. The trigger type determines how much can be presumed versus how much has to be investigated.
2. **Session and token revocation.** Every application session and refresh token for that identity, invalidated everywhere, not just at the identity provider. This is the step that actually stops the attacker and it happens before anything else.
3. **MFA re-enrollment.** A clean re-enrollment on a verified device, treating any browser-extension-based second factor as burned rather than reusable.
4. **Password rotation.** Downstream of the first two, not a substitute for either.
5. **Artifact population presumed exposed.** The full set from the section above, scoped to the host, not to what is provable.
6. **Disposition of the host.** Reimaged, isolated pending analysis, or accepted as a personal device outside the organization's control, which changes what can be enforced but not what is presumed stolen.

## The response record

```text
Exposure ID:         CS-2026-0143
Identity:            <name>
Trigger:             Stealer-log notification, corporate domain match
Detected:            2026-03-11
Session revocation:  2026-03-11 14:02 (all applications)
MFA re-enrollment:   2026-03-11 14:20 (hardware key, in person)
Password rotation:   2026-03-11 15:10
Host disposition:    Personal device, reimage not enforceable
Presumed population: All saved credentials, cookies, and browser-extension
                      secrets on the host as of infection date
Owner:               <name>
```

| Field                          | Why it is there                                                                                                                                       |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Trigger                        | The three trigger types carry different confidence, and the record should say which one started the clock                                             |
| Session revocation timestamp   | The primary control. If this field is blank the response has not actually happened yet                                                                |
| MFA re-enrollment, with method | A re-enrollment onto another browser extension has not fixed anything; the method field forces that distinction onto the record                       |
| Password rotation timestamp    | Kept, but positioned after the two controls it depends on, so the record cannot read as complete on rotation alone                                    |
| Host disposition               | A personal device is not reimageable on the organization's authority, and the record should say so rather than imply a control that was not available |
| Presumed population            | The default is the full population from the presumption rule above; a narrower entry requires a cited evidence source, not an assumption              |

## Time-to-session-revocation is the metric

Time-to-password-reset is the number every ticketing tool reports by default, and it is the wrong one, for the same reason revocation lag rather than campaign completion is the number that matters in the [access review pattern](access-review-pattern.md).
A password reset completed in ten minutes with the session still live for another six hours reports as a fast response and was not one.

Measure the interval from confirmed trigger to session and token revocation, not to password rotation, because that interval is the window the attacker actually had a usable credential.
Publish it beside the rotation-time number rather than instead of it, so a program cannot improve its dashboard by rotating faster while revoking at the same pace it always did.
A widening gap between the two numbers over time is itself a finding: it means the response process is optimizing for the metric that gets reported rather than the control that ends the exposure.

## Where this connects

- Egress logging that could narrow the presumed population beyond the default comes from the same sources the [evidence-readiness register](../incident-response/evidence-readiness-register.md) catalogs, and the same "complete for the window" qualifier applies before any narrower claim is accepted.
- Where the identity holds standing privileged access, or the host reaches a capability in the [crown jewel inventory](../assets/crown-jewel-inventory-pattern.md), the presumed population extends to what that access could have touched, not just what the host stored, and that population belongs on the [risk register](../risk-management/risk-register-pattern.md) until closed.
- Who may authorize skipping a step in the fixed order, for example accepting a password rotation without session revocation on a system that cannot support it, is a row in the [decision rights register](../governance/decision-rights-register.md), recorded as an [exception](../risk-management/security-exception-record.md) with an expiry, not a note in the response record.
- A pattern of exposures traced back to browser-extension authenticators or password managers is a finding for the organization's MFA standard, not a one-off remediation, and it belongs beside the [access review](access-review-pattern.md) population that certifies which authenticator types are acceptable.
- Time-to-session-revocation and the rotation-time gap belong in the [metrics catalog](../metrics/security-metrics-catalog.md), reported together for the reason stated above.
- An exposure that grows past a single identity, or where the presumed population is large enough to be reportable, hands off to the [materiality determination record](../incident-response/materiality-determination-record.md), which starts from the same presumption-resolves-against-you rule applied to a population instead of a host.

## Common failure modes

1. **Rotation reported as resolution.** The ticket closes on a new password while the session and the browser-extension TOTP seed are still live. Revocation and re-enrollment are the controls; rotation is cleanup.
2. **Browser-extension MFA treated as intact.** A second factor stored beside the first factor in the same infected browser is presumed compromised, not presumed safe because "MFA was enabled."
3. **Presumption narrowed without evidence.** The population is scoped down to "just the one account" because a wider scope is inconvenient, with no egress log cited to justify it.
4. **Corporate device assumptions applied to personal ones.** The host disposition field is left blank or filled in as reimaged when the device was never under the organization's control to begin with.
5. **Revocation done at the identity provider only.** Application sessions issued before revocation, particularly long-lived refresh tokens, keep working after the identity provider shows the session as ended.
6. **Time-to-password-reset as the only published number.** The dashboard improves by rotating faster while the revocation interval, the number that matters, goes unmeasured.
7. **No order enforced.** Whoever is on call does whichever step is fastest first, and password rotation is almost always the fastest, so it happens first and the response reads as complete before the session is actually closed.
8. **One-off treatment of a systemic browser-extension finding.** The same authenticator or password manager keeps appearing across unrelated exposures and each one is remediated individually instead of becoming a standard.
