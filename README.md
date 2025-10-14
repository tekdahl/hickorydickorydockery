# Structure

This repository's actions depend on setting a few secrets

|secret type|secret name|
|---|---|
|Actions|DOCKER_REGISTRY|
|Actions|DOCKER_USERNAME|
|Actions|DOCKER_PASSWORD|

with just these set you could use the manual publish.

it takes the directory of your dockerfile as an argument and uses it as a tag for publishing to the docker registry.

`.github/workflows/docker-publish-manual.yml`

# Making a new job

Using this structure will result in actions that run a test build when a pr is made to main. On push/merge to main in the paths you restrict it to the test build will run and publish if the test build passes.

Using this job as an example you could configure any new action.

`hugobuilder/Dockerfile`

`.github/workflows/docker-call-hugobuilder.yml`

- place your Dockerfile in a folder that you want the docker tag to match

`newimage/Dockerfile` 

- copy the example yml and give it a unique name
`.github/workflows/docker-call-newimage.yml`
- change the paths trigger and docker-dir value to match your new Dockerfile's location adding a new folder with containing a Dockerfile etc.
