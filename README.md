# AWS SSO CLI
![Tests](https://github.com/synfinatic/aws-sso-cli/workflows/Tests/badge.svg)
[![Report Card](https://goreportcard.com/badge/github.com/synfinatic/aws-sso-cli)](https://goreportcard.com/report/github.com/synfinatic/aws-sso-cli)
[![GitHub license](https://img.shields.io/badge/license-GPLv3-blue.svg)](https://raw.githubusercontent.com/synfinatic/aws-sso-cli/main/LICENSE)

## About

AWS SSO CLI is a secure replacement for using the [aws configure sso](
https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html)
wizard with a focus on security and ease of use for organizations with
many AWS Accounts and/or users with many IAM Roles to assume.

AWS SSO CLI requires your AWS account(s) to be setup with [AWS SSO](
https://aws.amazon.com/single-sign-on/)!  If your organization is using the
older SAML integration (typically you will have multiple tiles in OneLogin/Okta)
then this won't work for you.

## What does AWS SSO CLI do?

AWS SSO CLI makes it easy to manage your shell environment variables allowing
you to access the AWS API using CLI tools.  Unlike the official AWS tooling,
the `aws-sso` command does not require defining named profiles in your
`~/.aws/config` (or anywhere else for that matter) for each and every role you
wish to assume and use.

Instead, it focuses on making it easy to select a role via CLI arguments or
via an interactive auto-complete experience with automatic and user-defined
metadata (tags) and exports the necessary [AWS STS Token credentials](
https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html#using-temp-creds-sdk-cli)
to your shell environment.

## Demo

Here's a quick demo showing how to select a role to assume in interactive mode
and then run commands in that context (by default it starts a new shell).

[![asciicast](https://asciinema.org/a/445604.svg)](https://asciinema.org/a/445604)

## Getting started

### Installation

 * Option 1: [Download binary](https://github.com/synfinatic/aws-sso-cli/releases)
    1. Copy to appropriate location and `chmod 755`
 * Option 2: [Download RPM or DEB package](https://github.com/synfinatic/aws-sso-cli/releases)
    1. Use your package manager to install (Linux only)
 * Option 3: Install via [Homebrew](https://brew.sh)
    1. Run `brew install synfinatic/aws-sso-cli/aws-sso-cli`
 * Option 4: Build from source:
    1. Install [GoLang](https://golang.org) and GNU Make
    1. Clone this repo
    1. Run `make` (or `gmake` for GNU Make)
    1. Your binary will be created in the `dist` directory
    1. Run `make install` to install in /usr/local/bin

Note that the release binaries are not officially signed at this time so MacOS
and Windows systems may generate warnings.

### Configuration 

Just run `aws-sso`, it will automatically detect there is no configuration file and prompt
you for the necessary values to get started.  [More advanced configuration details are 
available here](docs/config.md).

### Basic Usage

`aws-sso` has [full documentation](docs/usage.md) for all the commands, but a brief overview:

 * `cache` -- Force refresh of AWS SSO role information
 * `console` -- Open AWS Console in a browser with the selected role
 * `eval` -- Print shell environment variables for use in your shell
 * `exec` -- Execute a command with the selected role
 * `flush` -- Force delete of cached AWS SSO credentials
 * `list` -- List all accounts & roles
 * `tags` -- List all the tags for each role
 * `time` -- Print how much time remains for currently selected role
 * `version` -- Print the version of aws-sso
 * `install-autocomplete` -- Install auto-complete functionality into your shell

The `--help` / `-h` flag will print more detailed and context sensitive help.

Assuming a role on the command line will generally have you invoking either the `exec` or `eval`
command:

The `exec` command allows interactively selecting the role as seen in the demo above and forks
a new shell with the necessary environment variables set.  By default, it will start a new shell
or you can give it a command to run.

The `eval` command is used to generate a series of `export XXXX` commands that can be imported
into the current shell via `eval $(aws-sso eval ...)`.  Because the `eval` command interacts
with the _current_ shell, you must specify the AWS AccountID and Role name (or ARN) instead of
using the interactive selector available in `exec`.

## Security

Unlike the official [AWS cli tooling](https://aws.amazon.com/cli/), _all_
authentication tokens and credentials used for accessing AWS and your SSO
provider are encrypted on disk using your choice of secure storage solution.
All encryption is handled by the [99designs/keyring](https://github.com/99designs/keyring)
library.

Credentials encrypted by `aws-sso` and not via the standard AWS CLI tool:

 * AWS SSO ClientID/ClientSecret -- `~/.aws/sso/cache/botocore-client-id-<region>.json`
 * AWS SSO AccessToken -- `~/.aws/sso/cache/<random>.json`
 * AWS Profile Access Credentials -- `~/.aws/cli/cache/<random>.json`

As you can see, not only does the standard AWS CLI tool expose the temporary
AWS access credentials to your IAM roles, but more importantly the SSO
AccessToken which can be used to fetch IAM credentials for any role you have
been granted access!

### What is not encrypted?

 * Contents of user defined `~/.aws-sso/config.yaml`
 * Meta data associated with the AWS Roles fetched via AWS SSO in `~/.aws-sso/cache.json`
    * Email address tied to the account (root user)
    * AWS Account Alias
    * AWS Role ARN

## Additional Documentation

 * [Configuration](docs/config.md) -- Detailed information every configuration option in `~/aws-sso/config.yaml`
 * [Usage](docs/usage.md) -- Detailed usage for every command


## License

AWS SSO CLI is licnsed under the [GPLv3](LICENSE).
