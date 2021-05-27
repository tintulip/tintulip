# Terraform

Is our tool of choice for Infrastructure as Code.

## Managing versions on local machines

We are using [tfenv](https://github.com/tfutils/tfenv).

### Installed version verification

When installing a new tf version, `tfenv` tries to verify its integrity with `keybase` but happily skips it with a mere warning if `keybase` is not installed. :warning: this can introduce a supply chain risk.

Either installing [`keybase`](https://keybase.io/download) or configure `tfenv` to use your local `gpg` keyring instead following [the usage documentation](https://keybase.io/download):

```
gpg --recv-keys 72D7468F # from https://www.hashicorp.com/security - check there for a new key if 72D7468F shows up as revoked
echo 'trust-tfenv: yes' > ~/.tfenv/use-gnupg
```

## Terratest
We are using [Terratest](github.com/gruntwork-io/terratest/modules/terraform) to test the terraform code. In the [application-infra README](https://github.com/tintulip/application-infra/blob/main/README.md#test) contains the commands required to run the tests.
