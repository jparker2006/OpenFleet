# Releasing

## Version lines

- **OpenFleet** product versions: `0.0.0` is this design preview.
- **PACP** protocol drafts: `v0.1` is the current draft. Protocol versions change independently of product versions (see [PACP versioning](docs/pacp/v0.1.md#13-versioning)).

## What 0.0.0 is

0.0.0 is a set of proposal documents. It contains no implementation, package, install step, endpoint or test suite. Its release notes and any tag must say **"design preview, no implementation"**. A 0.0.0 release does not mean that any component works or that PACP has been implemented anywhere.

Later versions are not scheduled. A version that contains software must state, before it is tagged, how to install it, what was tested, and on what evidence.

## How a release is reviewed

Work is prepared privately and published here only through an allowlisted export review:

1. **Allowlist.** Only files named on the release's approved file list are published. Directories are not copied wholesale.
2. **Written for the public.** Public files are written fresh with synthetic examples. Private notes, history and records are never copied in.
3. **Privacy and secret check.** Every file is read in full and scanned for personal data, credentials, private links and private identifiers.
4. **Claim check.** Every statement about capability has a status label and is no stronger than its evidence.
5. **Maintainer approval.** The maintainer approves the exact file list and diff.
6. **Publish and verify.** The change is published as an ordinary commit without force-pushing. The remote tree is then compared with the approved files.

The release does not happen if any step fails. Tags and GitHub Releases are created only by the maintainer, and only after step 6.

## What a release never claims

- that it works with any specific personal agent, vendor or service, the pilot targets included, unless that has been tested and the evidence is published;
- that any controller command or button exists before it has shipped;
- that anything conforms to PACP, because no conformance suite exists yet.

Every release keeps the unmodified [Apache License 2.0](LICENSE) text and any third-party attribution its contents require.
