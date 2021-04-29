# Scout Suite

The report was generated manually within a docker container. The instructions to do so are found on the tool's [GitHub repo wiki](https://github.com/nccgroup/ScoutSuite/wiki/Docker-Image).

Currently it is run with the owners role configured.

On the host machine, run the following command to download the docker image and run it:

`docker run -it rossja/ncc-scoutsuite`

Within the container, configure the AWS cli tool.

`aws configure --profile documentation`

Run the scout tool and save the report.

`scout aws --profile documentation-owners --no-browser --report-dir /root/scout-report`

On the host machine, copy the report within the docker container to the host machine.

`docker cp $(docker ps -f ancestor=rossja/ncc-scoutsuite --format "{{.ID}}"):/root/scout-report ./`