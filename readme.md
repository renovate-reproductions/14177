# Reproducing renovate error: 

Error during gitlab-include update: "Error updating branch: update failure (repository=oberhaus77/renovate-test, branch=renovate/oberhaus77-renovate-test-1.x)"

## Run Renovate in Docker container

```shell

git clone https://gitlab.com/oberhaus77/renovate-test.git
cd renovate-test

docker run -ti --rm -v $(PWD):/work --entrypoint bash renovate/renovate:latest

--gitlab.com
export GITLAB_TOKEN=xxx
export RENOVATE_CONFIG_FILE=renovate.json
export LOG_LEVEL=debug
unset http_proxy https_proxy HTTP_PROXY HTTPS_PROXY

renovate --token ${GITLAB_TOKEN} --platform gitlab oberhaus77/renovate-test
```

## Expected result:

Two PRs should be created. One PR for gitlab includes and one PR for Dockerfile

## Current result:
PRs aren't created. Following errors are found in the log of renovate:

````
DEBUG: Starting search at index 62 (repository=oberhaus77/renovate-test, packageFile=.gitlab-ci.yml, branch=renovate/oberhaus77-renovate-test-1.x)
       "depName": "oberhaus77/renovate-test"
DEBUG: Found match at index 62 (repository=oberhaus77/renovate-test, packageFile=.gitlab-ci.yml, branch=renovate/oberhaus77-renovate-test-1.x)
       "depName": "oberhaus77/renovate-test"
 WARN: Error updating branch: update failure (repository=oberhaus77/renovate-test, branch=renovate/oberhaus77-renovate-test-1.x)
DEBUG: Ensuring Dependency Dashboard (repository=oberhaus77/renovate-test)
````

Also renovate tries to create PRs/Branches named pin-dependencies which includes updates for .gitlab-ci.yml

```
DEBUG: Starting search at index 5 (repository=oberhaus77/renovate-test, packageFile=Dockerfile, branch=renovate/pin-dependencies)
       "depName": "busybox"
DEBUG: Found match at index 5 (repository=oberhaus77/renovate-test, packageFile=Dockerfile, branch=renovate/pin-dependencies)
       "depName": "busybox"
DEBUG: Contents updated (repository=oberhaus77/renovate-test, packageFile=Dockerfile, branch=renovate/pin-dependencies)
       "depName": "busybox"
DEBUG: Starting search at index 62 (repository=oberhaus77/renovate-test, packageFile=.gitlab-ci.yml, branch=renovate/pin-dependencies)
       "depName": "oberhaus77/renovate-test"
DEBUG: Found match at index 62 (repository=oberhaus77/renovate-test, packageFile=.gitlab-ci.yml, branch=renovate/pin-dependencies)
       "depName": "oberhaus77/renovate-test"
 WARN: Error updating branch: update failure (repository=oberhaus77/renovate-test, branch=renovate/pin-dependencies)
DEBUG: Setting current branch to main (repository=oberhaus77/renovate-test, branch=renovate/oberhaus77-renovate-test-1.x)
```