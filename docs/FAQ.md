# finna-pk FAQ

## What is finna-pk?

finna-pk is a fork of [openpubkey/opkssh](https://github.com/openpubkey/opkssh) that provides SSH authentication using OpenID Connect (OIDC) providers. It has been customized to fit our specific use case while maintaining compatibility with the OpenPubkey protocol.

## How is finna-pk different from opkssh?

finna-pk is a customized fork of opkssh. While it maintains compatibility with the core OpenPubkey protocol, it has been modified to better suit our specific requirements. The main differences include:

- Custom configuration directory (`.finna-pk` instead of `.opk`)
- Custom system configuration paths (`/etc/finna-pk` instead of `/etc/opk`)
- Tailored to our specific deployment needs

## What is OpenPubkey?

OpenPubkey is a protocol for leveraging OpenID Providers (OPs) to bind identities to public keys. It adds user- or workload-generated public keys to [OpenID Connect (OIDC)](https://openid.net/developers/how-connect-works/), enabling identities to sign messages or artifacts under their OIDC identity.

## How does finna-pk work?

finna-pk uses the OpenPubkey protocol to create PK Tokens that bind your OIDC identity (from providers like Google, Azure, GitLab, etc.) to a public key. These PK Tokens are then used to authenticate SSH connections, eliminating the need for traditional SSH key management.

## What OpenID Providers are supported?

finna-pk is compatible with any OpenID Connect provider, including:

- Google Workspace
- Microsoft Azure / Entra ID
- GitLab (including self-hosted)
- GitHub Actions
- Okta
- OneLogin
- Keycloak
- And any other OIDC-compliant provider

## Where are configuration files stored?

- **Client configuration**: `~/.finna-pk/config.yml` (Linux) or `%APPDATA%\.finna-pk\config.yml` (Windows)
- **Server provider configuration**: `/etc/finna-pk/providers`
- **Server policy files**: `/etc/finna-pk/auth_id` (system-wide) or `~/.finna-pk/auth_id` (user-specific)
- **Server config**: `/etc/finna-pk/config.yml`

## How do I get started?

1. Install finna-pk on your client machine
2. Run `finna-pk login` to authenticate with your OIDC provider
3. Configure your SSH server to use finna-pk for authentication
4. Add authorized identities to the policy file

See the [README](../README.md) and [installation guide](../scripts/installing.md) for detailed instructions.

## Is finna-pk secure?

Yes. finna-pk uses the OpenPubkey protocol, which provides cryptographic guarantees that:

- The identity in the PK Token is verified by the OpenID Provider
- The public key is cryptographically bound to the identity
- The private key is controlled by the user

The protocol does not add any new trusted parties beyond what is required for OpenID Connect.

## Can PK Tokens be replayed against OIDC providers?

No. finna-pk uses GQ signatures (when enabled) to prevent replay attacks. The OpenID Provider's signature is stripped from the ID Token and replaced with a proof, preventing the ID Token from being used against OIDC resource providers.

## What about key management?

finna-pk uses ephemeral key pairs. Keys are generated as needed and can be deleted when done. This eliminates traditional SSH key management headaches.

## Does finna-pk work with existing SSH setups?

Yes. finna-pk integrates with OpenSSH using the `AuthorizedKeysCommand` feature. It works alongside or instead of traditional SSH key authentication.

## Where can I get help?

- Check the [documentation](../README.md)
- Review [existing issues](https://github.com/FinnaCloud/finna-pk/issues)
- For security issues, contact **noc@finnacloud.com** (do not file a public issue)

## License

finna-pk is licensed under the Apache License 2.0. See the [LICENSE](../LICENSE) file for details.
