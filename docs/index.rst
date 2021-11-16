Welcome to AWS SSO CLI's documentation!
=======================================

**aws-sso** is an application for easily accessing IAM Roles
via `AWS Single Sign On <https://aws.amazon.com/single-sign-on/>` via
the command line.  It allows users to quickly retrieve the necessary 
temporary credentials for making AWS API calls by authenticating your your
SSO provider (such as `OneLogin <https://www.onelogin.com>` or 
`Okta <https://www.okta.com>` or even get AWS Web Console access from your
shell.

**aws-sso** is a replacement for the official 
`aws configure sso <https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html>`
wizard and designed to be easier to use and more secure.

**aws-sso** is written in Go and is designed to be run on macOS, Linux,
and Windows systems.

Check out the :doc:`usage` section for further information, including
how to :ref:`installation` the project.

.. note::

   This project is under active development.

Contents
--------

.. toctree::

   usage
   api
