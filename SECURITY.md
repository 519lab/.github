# Security Policy

## Supported Versions

Security fixes are applied to the latest release and the default branch (`main`) only. Older releases are not patched unless stated otherwise in the repository README.

| Version          | Supported |
| ---------------- | --------- |
| Latest release   | ✅        |
| `main` branch    | ✅        |
| Older releases   | ❌        |

## Reporting a Vulnerability

**Do not report security vulnerabilities through public GitHub issues, discussions, or pull requests.**

Report vulnerabilities privately using GitHub's built-in reporting:

1. Go to the **Security** tab of the affected repository.
2. Click **Report a vulnerability**.
3. Fill out the advisory form.

This works from any repository in this account. If you don't see the button, see GitHub's guide: [Privately reporting a security vulnerability](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability).

Please include as much of the following as you can:

- Affected repository, version, commit, or component
- Vulnerability type (e.g. injection, auth bypass, RCE, information disclosure)
- Step-by-step reproduction or proof of concept
- Impact assessment: what an attacker could achieve
- Any suggested fix or mitigation

## Response Timeline

| Stage                          | Target                          |
| ------------------------------ | ------------------------------- |
| Acknowledgement of report      | Within 3 business days          |
| Initial triage and severity    | Within 7 business days          |
| Status updates                 | At least every 14 days          |
| Fix for Critical / High        | Within 30 days of triage        |
| Fix for Medium / Low           | Within 90 days of triage        |

Severity is assessed with [CVSS v3.1/v4.0](https://www.first.org/cvss/).

## Coordinated Disclosure

We follow a coordinated disclosure model:

- All report details stay **private** between the reporter and maintainers until a fix is released.
- The default embargo is **90 days** from the initial report, or until a patched release is available, whichever comes first.
- If a fix needs more time, we will explain why and agree on an extension with the reporter. We will not extend indefinitely.
- If a vulnerability is being actively exploited in the wild, we may disclose sooner to protect users.
- Once a fix is released, we publish a GitHub Security Advisory and request a CVE where appropriate.

We ask reporters not to disclose publicly, share details with third parties, or exploit the issue beyond what is needed to demonstrate it until the advisory is published.

## Recognition

Reporters who want credit will be acknowledged by name or handle in the published GitHub Security Advisory (as a credited contributor) and in the release notes. Anonymous reports are equally welcome. Tell us your preference in the report.

There is no paid bug bounty at this time.

## Safe Harbor

Security research performed in good faith under this policy is considered authorized. We will not pursue legal action against researchers who:

- Make a good-faith effort to avoid privacy violations, data destruction, and service disruption
- Only interact with accounts and data they own or have explicit permission to access
- Report promptly and give us reasonable time to fix before any disclosure
- Do not use the vulnerability for anything beyond demonstrating it

## Out of Scope

- Vulnerabilities in third-party dependencies that are already publicly known (report those upstream; we track them through Dependabot)
- Findings from automated scanners with no demonstrated impact
- Social engineering, phishing, or physical attacks
- Denial of service through volumetric traffic
- Missing best-practice headers or configuration with no exploitable impact

## Contact

Use GitHub private vulnerability reporting (above) as the primary channel. If GitHub reporting is unavailable, email **security@519lab.com** and do not include exploit details until we reply with a secure channel.
