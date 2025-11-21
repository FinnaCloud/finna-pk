# Contributing to finna-pk

Welcome to finna-pk! We are so excited you are here. Thank you for your interest in contributing your time and expertise to the project. The following document details contribution guidelines.

# Getting Started

Whether you're addressing an open issue (or filing a new one), fixing a typo in our documentation, adding to core capabilities of the project, or introducing a new use case, anyone from the community is welcome here at finna-pk.

## Include Licensing at the Top of Each File

At the top of each file in your commit, please ensure the following is captured in a comment:

` SPDX-License-Identifier: Apache-2.0 `

## Sign Off on Your Commits

Contributors are required to sign off on their commits. A sign off certifies that you wrote the associated change or have permission to submit it as an open-source patch. All submissions are bound by the [Developer's Certificate of Origin 1.1](https://developercertificate.org/) and [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).

```
Developer's Certificate of Origin 1.1

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I
    have the right to submit it under the open source license
    indicated in the file; or

(b) The contribution is based upon previous work that, to the best
    of my knowledge, is covered under an appropriate open source
    license and I have the right under that license to submit that
    work with modifications, whether created in whole or in part
    by me, under the same open source license (unless I am
    permitted to submit under a different license), as indicated
    in the file; or

(c) The contribution was provided directly to me by some other
    person who certified (a), (b) or (c) and I have not modified
    it.

(d) I understand and agree that this project and the contribution
    are public and that a record of the contribution (including all
    personal information I submit with it, including my sign-off) is
    maintained indefinitely and may be redistributed consistent with
    this project or the open source license(s) involved.
```

Your sign off can be added manually to your commit, i.e., `Signed-off-by: Jane Doe <jane.doe@example.com>`. 

Then, you can create a signed off commit using the flag `-s` or `--signoff`:
`$ git commit -s -m "This is my signed off commit."`.

## Development Environment Setup

To run the [Google example](https://github.com/FinnaCloud/finna-pk/tree/main/examples/google):

```bash
go run ./examples/simple/example.go
```

This will open a browser window to Google. If you authenticate to Google successfully, you should see: `Verification successful: anon.author.aardvark@gmail.com (https://accounts.google.com) signed the message 'All is discovered - flee at once'` where `anon.author.aardvark@gmail.com` is your gmail address.

## Testing

Before submitting a pull request, please ensure all tests pass:

```bash
go test ./...
```

For integration tests:

```bash
./hack/integration-tests.sh
```

## Submitting Changes

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Ensure all tests pass
5. Sign off on your commits
6. Submit a pull request

## Questions?

If you have questions about contributing, please open an issue or contact the maintainers.
