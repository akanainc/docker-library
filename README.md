# Akana Platform Container - Automated deployment

Fully automated docker image that can contain Akana Platform containers.  The docker container is Akana container agnostic.

The type of Akana container is defined and created at the time of `docker run`.  The Akana automation scripts are used to create and configure the Akana container.  See the automation documentation on more information on using automation: https://github.com/akanainc/automation

## Supported tags and respective Dockerfile links
* [`8.4.0`](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile), [`8.4.2`](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile), [`8.4.3`](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile), [`8.4.10`](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile), [`8.4.11`](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile), [`8.4.12`](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile), [`8.4.13`](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile), [`8.4.15`](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile), [`8.4.15.1`](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile), [`8.4.16`](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile), [`8.4.19`](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile), [`latest`](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile) [(stable/latest/Dockerfile)](https://github.com/akanainc/docker-library/blob/master/container/Dockerfile)
* [`8.3.0`](https://github.com/akanainc/docker-library/blob/8.3.x/container/Dockerfile), [`8.3.1`](https://github.com/akanainc/docker-library/blob/8.3.x/container/Dockerfile) [(stable/latest/Dockerfile)](https://github.com/akanainc/docker-library/blob/8.3.x/container/Dockerfile)
* [`8.2`](https://github.com/akanainc/docker-library/blob/8.2.x/container/Dockerfile), [`8.2.2`](https://github.com/akanainc/docker-library/blob/8.2.x/container/Dockerfile), [`8.2.5`](https://github.com/akanainc/docker-library/blob/8.2.x/container/Dockerfile), [`8.2.7`](https://github.com/akanainc/docker-library/blob/8.2.x/container/Dockerfile), [`8.2.8`](https://github.com/akanainc/docker-library/blob/8.2.x/container/Dockerfile) [(stable/8.2.x/Dockerfile)](https://github.com/akanainc/docker-library/blob/8.2.x/container/Dockerfile)
* 8.1, 8.1.1
* [`8.0.11`](https://github.com/akanainc/docker-library/blob/8.0.x/base/Dockerfile), [`8.0.12`](https://github.com/akanainc/docker-library/blob/8.0.x/base/Dockerfile) [(stable/8.0.x/Dockerfile)](https://github.com/akanainc/docker-library/blob/8.0.x/container/Dockerfile)

## Pull

Run the following command.  This is not required, images will be pulled during the run command if they are not found locally.

```
docker pull akanainc/container
```

## Create image
Create new Docker image:

```
docker run -p 9900:9900 -p 10001:10001 --restart=unless-stopped -name <image_name> -d --env PROPERTY_FILES=<archived_property_files> --env PMRUNNING=true "akanainc/container"
```

This can be completed by using the default property files located on S3 'https://s3-us-west-1.amazonaws.com/automation8/properties/gateway_manual_registration.zip', or these files can be updated to represent a custom AMI container.

After initially starting the container, it could take up to 10 minutes for the container to be completely installed and configured, depending on the type of container that is being provisioned.

All valid options to pass into the docker container is:

* `--installpath`	Path to the installation
* `--key` Container key of the container that needs to be deleted or created
* `--name` Container name
* `--timeout` Wait time for server to start up
* `--hostname` Hostname of server to be built
* `--propertyFiles` Property files to build the containers.
* `--logFile` Log file for all output.
* `--javaopts` Custom JAVA_OPTS that will be used at startup time, must be surrounded by '"'.
* `--pmrunning` Should the script validate PM is running, prior to making calls to PM
* `--custompolicies` Custom Policies to deploy to the deploy directory.
* `-m` monitor the container
* `--containerlog` Sets container build log level, defaults to 'ERROR'.  Valid values are: 'DEBUG', 'INFO', 'WARNING', 'ERROR', 'CRITICAL'.
* `--databaselog` Sets database build log level, defaults to 'CRITICAL'.  Valid values are: 'DEBUG', 'INFO', 'WARNING', 'ERROR', 'CRITICAL'.
* `--javahome` Custom JAVA_HOME directory is needed.
* `--datacentername` Data Center Name that this container exists in.  Needed for MongoDB sharding.

All options can also be passed in using Docker environment variables.  The environment variables are maped up in the following way:

* `INSTALL_PATH` == --installpath
* `KEY` == --key
* `NAME` == --name
* `TIMEOUT` == --timeout
* `HOSTNAME` == --hostname
* `PROPERTY_FILES` == --propertyFiles
* `LOG_FILE` == --logFile
* `JAVA_OPTS` == --javaopts
* `PM_RUNNING` == --pmrunning - This setting will be recognized as either `true` or `True`.
* `CUSTOM_POLICIES` == --custompolicies
* `MONITOR` == -m - This setting will be recognized as either `true` or `True`.
* `CONTAINER_LOG` == --containerlog
* `DATABASE_LOG` == --databaselog
* `JAVA_HOME` == --javahome
* `DATA_CENTER_NAME` == --datacentername

A valid Docker run command using only environment variables would look something like:

```
docker run -p 9900:9900 -p 10001:10001 --restart=unless-stopped --name ami_82 -d -v /tmp/registeredSecuredAMI:/opt/akana_sw/stage/mount --env PROPERTY_FILES=/opt/akana_sw/stage/mount/https_gateway_registered.zip --env MONITOR=True --env PM_RUNNING=True --env CONTAINER_LOG=DEBUG --env DATA_CENTER_NAME=USWest "akanainc/container"
```

### Support for Trace Agents
The Akana Docker container supports connecting to Trace tools like `DynaTrace` or `AppDynamics`.  The agent is set in the container log file, but the agent also needs to be volume mounted to the Docker instance.  In the run command, you would also need to include `-v /app/appd/agent.jar:/opt/akana_sw/appd/agent.jar`.

### Container Logs
A log viewer is automatically installed on all containers.  By default, this log viewer will display all container logs stored under `/opt/akana_sw/sm8/instances/<container>/logs`.

The log viewer can be seen via a browser at http://<hostname>:10001, or which ever port is mapped to the Docker port `10001`. 

The log viewer service is ran as a `nohup` process.  Any logs should be located under `/opt/akana_sw/stage/install/logviewer`.

### Debugging
At time the container build will need to be debugged. 

* Logging can be increase with setting the following command options:
  * `--containerlog` This should be set to either INFO or DEBUG
  * `--databaselog` If a new database is being built or schemas are being updated the user can run in either INFO or DEBUG.  This is a very verbose setting if running in DEBUG.
* Set the run command to output all log statements to a log file. This is accomplished by adding the `--logFile` option:

```
docker run -p 9900:9900 -p 10001:10001 --restart=unless-stopped -name <image_name> -d "akanainc/container" --propertyFiles <archived_property_files> --pmrunning --logFile /opt/akana_sw/logs/createContainers.log
```

Automation can be updated without building a new container.  Provide the following command option at the Docker run command:

* `--updateautomation` This will provide the location of the automation archive to be extracted.

```
docker run -p 9900:9900 -p 10001:10001 --restart=unless-stopped -name <image_name> -d "akanainc/container" --propertyFiles <archived_property_files> --pmrunning --logFile /opt/akana_sw/logs/createContainers.log --updateautomation <automation_archive_file>
```
