# Kubernetes Concepts

A high level overview of Kubernetes specific concepts.

## ConfigMap Generator

Usually deployments make use of ConfigMap variables to launch the necessary 
containers, but sometimes a ConfigMap needs to be changed to account for new
variables or even simple more mundane changes. The problem arises when everything
is already up and running so the deployment wont acknowledge the new ConfigMap and
you need to manually kill the deployment and launch it again. 

This can be handled with the [configMapGenerator](https://github.com/openshift/kubernetes-kubectl/blob/master/docs/book/pages/reference/kustomize.md#configmapgenerator) option from kustomize. Every time 
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

## Sealed Secret

[Sealed Secret](https://github.com/bitnami-labs/sealed-secrets/tree/main) is a third party tool to encrypt Secrets in Kubernetes so they are
safe to store. There is no native Kubernetes tools to safely store secrets, there 
is only the Secret object but that allows its management nothing else. There are two 
main features that allow for this type of workflow first a SealedSecret controller 
present on the target cluster and a ``kubeseal`` tool to encrypt the secrets so only 
the controller and nobody else is able to decrypt them. And only a certain controller
will be able to decrypt them because secrets are encrypted with your controller public
key.

Lets say you have a Private GKE cluster. In this case we use an offline sealing method
for our secrets. For example, we have a DB Password that we would like sealed. You
would seal the secret as such:

``kubeseal --cert=cert.pem<secret.yaml``

Once the sealedsecret configuration has been created we can create a SealedSecret
kubernetes object that will allow us to manage the sealedsecret. This can be safely
stored in a even public repositories and no one will be able to decrypt it only our 
specific source controller.

```

apiVersion: bitnami.com/v1alpha1
kind: SealedSecret
metadata:
  name: lock
  namespace: lock
spec:
  encryptedData:
  # Secret encrypted by Kubeseal
    LOCK_DB_PASSWORD: AgAaRL...
  template:
    metadata:
      name: lock
      namespace: lock
    type: Opaque

```

In order to install the sealedsecret controller onto our cluster there are several
ways. We can either install the controller through kustomize or through helm charts
whatever we find more comfortable.

![SealedSecrets Workflow](images/SealedSecret.png)