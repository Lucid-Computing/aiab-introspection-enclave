# AIAB enclave release configuration

This directory contains only measured deployment settings, a policy manifest, and a pinned measurement workflow. The current deployed release is **v0.1.2**, using CVM 0.14.9. Its deployment pins and verification status are in [deployment.json](deployment.json). The original v0.1.1 acceptance below is historical evidence.

Current configuration image: `ghcr.io/lucid-computing/aiab-introspection-enclave@sha256:e42c2af9be20bd0aac1550f216b104b88f7f73121797558818c177b786e7f0db`

Offline policy digest: `8d03b314f111446aba77f1b944fda015919b0e0e222b0026b9057fee47cc1092`

Measured Tinfoil release digest (v0.1.1): `34b158a1d180c004daa1e0fde014848726be04c0b4506da6b6ed861ee264496c`.

Live enclave: `aiab-introspection.lucid-computing.containers.tinfoil.dev`.
For the historical v0.1.1 release, independent verification matched its release and policy digests; the verified
gateway then passed allowed, blocked, signed-report, wrong-pin, direct-token
rejection, and public-route checks. That original acceptance included a stop scheduled for 2026-09-26; this is not a current lifecycle guarantee.

The runtime image is **PRIVATE** and contains executable AIAB source and classifier definitions. The source GitHub repository also remains **PRIVATE**. Only this small measured configuration repository is public. Tinfoil must pull the private image using its configured registry credential; image visibility and shared credentials must not be changed to work around access failures.

Repository secrets: TINFOIL_API_KEY, ANTHROPIC_API_KEY, AIAB_GATEWAY_TOKEN. Secret values never belong in this repository. Standard Tinfoil deployment secrets are accessible to Tinfoil infrastructure.

## Image verification

Source commit: `85dbae29ec47915caa4e917174a8674afaa4f0f5` (private source repository).

Two independent clean local builds from the committed source, with GitHub checkout file modes (0644 files, 0755 directories), matched the published registry manifest digest exactly. The local development checkout uses 0664 source files; its earlier digest differs because COPY preserves those permissions.

Image config digest: `sha256:e22afb3f006bbb4940ece646e67561f2d20a91fe7dda2eae37c4b7980d93085b`. Both OCI exports were inspected across all layers and contain no Python bytecode or application cache directories.

This public config contains no provider credentials or private repository history. The private runtime image contains the application and classifier definitions; those files are not published by this configuration repository. Browser TLS terminates at the host, and governed inference is sent to Anthropic over HTTPS; neither is claimed to execute confidentially.

## Organization migration (2026-10-23)

Release **v0.1.2** uses `ghcr.io/lucid-computing/aiab-introspection-enclave`,
retaining the exact application digest from v0.1.1. The image was published to the
new private package and GHCR confirmed the unchanged digest. No application
rebuild or policy change was needed.

New measured release digest: `1cf9d292b7631486880cdd8ced07e4a92df330908a23a7984dff2f1e9ffe6b34`.
Configuration release: https://github.com/Lucid-Computing/aiab-introspection-enclave/releases/tag/v0.1.2

The v0.1.1 measurement above remains historical evidence and must not be used
for v0.1.2. Gateway clients must verify the new release digest with repository
`Lucid-Computing/aiab-introspection-enclave`; the policy digest is unchanged.
The separately deployed signed-plan demo uses `aiab-signed-audit-enclave`.

The v0.1.2 staged deployment was promoted after becoming ready. Independent
verification and requests through the updated local gateway succeeded. Tinfoil
still stores the historical GitHub repository alias internally; the release and
client verification use the canonical `Lucid-Computing` repository.
