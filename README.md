Demo for Server Side Software Developers South NL meetup 'Back-end programming: making your applications cloud ready'

Meetup event: https://www.meetup.com/Server-Side-Software-Developers-South-NL/events/264919153/

# Pre-requisites
* Docker for Desktop with Kubernetes enabled
* WSL (When running on Windows)

Before continuing, test that connectivity to Kubernetes is working
```bash
# docker-for-desktop test
kubectl --context=docker-for-desktop cluster-info

# all Jenkins demo deployments will be done to the default docker cluster. To check the default settings:
kubectl config current-context
```

# Jenkins setup
Before starting docker-compose, make sure you have created the following files
* docker hub credentials for pushing images
  * .secrets/docker-hub-username
  * .secrets/docker-hub-password
* github credentials for discoverying repositories
  * .secrets/github-username
  * .secrets/github-password
* .env file
  ```bash
  SMEE_CHANNEL=<channel id>
  HOME=<only needed on Windows, set to C:\Users\username>
  ```

The following  files will be automatically created on first start, these are:
* ./secrets/kubectl-encoded-config (a copy of ~/.kube/config)
* ./secrets/ssh-github-java-meetup-key (generated ssh keypair)
* ./secrets/ssh-github-java-meetup-key.pub (generated ssh keypair, used for cloning repositories)

Start jenkins using docker-compose
```bash
docker-compose --compatibility up -V --build
```

After Jenkins is up and running you can go ahead and visit these URL's
* Jenkins URL: http://localhost:8080
* Jenkins configuration as code: http://localhost:8080/configuration-as-code
* Jenkins Job DSL reference: http://localhost:8080/plugin/job-dsl/api-viewer/index.html

# Kubernetes setup (Docker for Desktop)
Provision kubernetes with some default demo deployments. After running the script the following is done
* kubernetes-dashboard installed
* k8dash installed (another kubernetes dashboard)
* namespace 'java-meetup' created

```bash
# This installs some kubernetes tools using 'kubectl --context=docker-for-desktop'
./setup.sh
```

If something doesn't work in Docker for Desktop, try the following
```bash
# Check for any errors in the docker log
tail -f /c/Users/<username>/AppData/Local/Docker/log.txt

# If Kubernetes fails to start, remove all docker-for-desktop certificates.
# These are automatically re-generated after a first restart
rm -rf /c/ProgramData/DockerDesktop/pki
```

# References
* Jenkins Job DSL docs: https://jenkinsci.github.io/job-dsl-plugin
