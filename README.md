# AWS Credential Helper

Credential helper meant to integrate with ALKScli to fetch AWS credentials
just-in-time.

## Install

Just copy the ACH script somewhere.

The credential_process setting in ~/.aws/config is annoying in that it only
takes absolute paths.  This means that if you want to use the same ~/.aws/config
on your host and in a Docker container, you need a stable path to put this
script in.  So, I put it in /usr/local/bin.

If by chance you use zinit, this is how I install it:

```
zinit for \
    as'null' \
    atload'
        ln -sf $PWD/ach /usr/local/bin/ach
    ' \
    atinit'
        export ACH_GPG_RECIPIENT=xxx
    ' \
        twang817/ach-credential-helper
```


## Configuration

This script will cache credentials on disk and must be able to encrypt/decrypt
them using a GPG keypair.  Set the GPG recipient using the ACH_GPG_RECIPIENT
variable.

```
export ACH_GPG_RECIPIENT="some@email.com"
```

Configure your profiles in ~/.aws/config

```
[profile myawsaccount]
credential_process = /usr/local/bin/ach myawsaccount
region = us-east-1
```

## Usage

Once configured, credentials can be retrieved just-in-time by setting an
AWS_PROFILE:

```
$ export AWS_PROFILE=myawsaccount
$ aws sts get-caller-identity
```
