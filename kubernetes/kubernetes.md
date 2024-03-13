# Kubernetes Concepts

## ConfigMap Generator and Sealed Secrets

Usually deployments make use of ConfigMap variables to launch the necessary 
containers, but sometimes a ConfigMap needs to be changed to account for new
variables or even simple more mundane changes. The problem arises when everything
is already up and running so the deployment wont acknowledge the new ConfigMap and
you need to manually kill the deployment and launch it again. 

This can be handled with the configMapGenerator option from kustomize. Every time 
the configuration files change, kustomize will create a new version of the configMap. 
Usually this new version has a hash as an appendix attached to it as a sort of 
version. Since its a new version, the deployment which consumes the new ConfigMap will 
then acknowledge the change and update accordingly.

![ConfigMap Generator workflow](images/ConfigMap%20Generator.png)

```

apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
configMapGenerator:
# generate a ConfigMap named my-java-server-props-<some-hash> where each file
# in the list appears as a data entry (keyed by base filename).
- name: my-java-server-props
  files:
  - application.properties
  - more.properties
# generate a ConfigMap named my-java-server-env-vars-<some-hash> where each literal
# in the list appears as a data entry (keyed by literal key).
- name: my-java-server-env-vars
  literals:	
  - JAVA_HOME=/opt/java/jdk
  - JAVA_TOOL_OPTIONS=-agentlib:hprof
# generate a ConfigMap named my-system-env-<some-hash> where each key/value pair in the
# env.txt appears as a data entry (separated by \n).
- name: my-system-env
  env: env.txt

```
