OpenShift S2I Demo using JenkinsPipeline
====================
This repo has Jenkins pipeline script which is executed inside OpenShift as a pipeline. Builds inside the pipeline are OpenShift S2I builds.

** Pre-requisites

Openshift OCP or OKD 3.11+
Jenkins
Maven S2I builder image
OpenShift sync plugin for Jenkins
A java maven project  ( I am using Vaadin Addressbook springboot project as an example here)

** Pipeline steps

This pipeline will perform the following tasks

1) Create a new build config in OpenShift for Java application
2) Run the build and create a Docker image
3) Deploy the docker image into OpenShift
4) Expose it using an OpenShift route


*** End of documentation
