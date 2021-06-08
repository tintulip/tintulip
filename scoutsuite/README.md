# Scout Suite

The report was generated manually within a docker container. The instructions to do so are found on the tool's [GitHub repo wiki](https://github.com/nccgroup/ScoutSuite/wiki/Docker-Image).

Currently it is run with the owners role configured.

On the host machine, run the following command to download the docker image and run it:

`docker run -it rossja/ncc-scoutsuite`

Within the container, configure the AWS cli tool.

`aws configure --profile tintulip`

Run the scout tool and save the report.

`scout aws --profile tintulip-owners --no-browser --report-dir /root/scout-report`

On the host machine, copy the report within the docker container to the host machine.

`docker cp $(docker ps -f ancestor=rossja/ncc-scoutsuite --format "{{.ID}}"):/root/scout-report ./`

## Using ScoutSuite with AWS SSO

Set up the security profile from within `~/.aws/config` replacing the account_name and account_id.

```bash
[profile tintulip-<account_name>-security]
sso_start_url = https://tintulip.awsapps.com/start/
sso_region = eu-west-2
sso_account_id = <account_id>
sso_role_name = SecurityAudit
region = eu-west-2
output = json
```

Authenticate with SSO

```
aws sso login --profile tintulip-<account_name>-security
```

Install scout using [pip](https://github.com/nccgroup/ScoutSuite/wiki/Setup#via-pip)
```
virtualenv -p python3 venv
source venv/bin/activate
pip install scoutsuite
```

Run scoutsuite using pip

```
scout aws -p tintulip-<account_name>-security --no-browser
```
