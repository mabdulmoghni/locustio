# locust

A chart to install Locust, a scalable load testing tool written in Python.

This chart will setup everything required to run a full distributed locust environment with any amount of workers.

This chart will also create configmaps for storing the locust files in Kubernetes, this way there is no need to build custom docker images.

By default it will install using an example locustfile and lib from [locustfiles/example]. When you want to provide your own locustfile, you will need to create 2 configmaps using the structure from that example:

```console
kubectl -n preview create configmap my-loadtest-locustfile --from-file path/to/your/main.py
kubectl -n preview create configmap my-loadtest-lib --from-file path/to/your/lib/
```

And then install the chart passing the names of those configmaps as values:

```console
helm install locust ./ \
  --set loadtest.name=my-loadtest \
  --set loadtest.locust_locustfile_configmap=my-loadtest-locustfile \
  --set loadtest.locust_lib_configmap=my-loadtest-lib
```

**Homepage:** <https://github.com/locustio/locust>

## How to install this chart for Local Usage -- Development  

Use minikube for local setup  

```console
minikube start  
eval $(minikube docker-env  
helm install locust ./  
kubectl -n default get pods  
kubectl --namespace default port-forward service/locust 8089:8089  
Connect to http://localhost:8089  
```
#########  
## How to install this chart for Local Usage -- Development

Deploy to a K8s Cluster  

To install the chart with the release name `my-release`:

```console
helm install my-release ./  
```

To install with some set values:

```console
helm install my-release ./ --set values_key1=value1 --set values_key2=value2
```

To install with custom values file:

```console
helm upgrade locust ./ --install --wait --atomic --namespace=preview --set=app.name=locust --values=./values.yaml --debug  
```

To Uninstall 

```console
helm uninstall locust
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| extraConfigMaps | object | `{}` | Any extra configmaps to mount for the master and worker. Can be used for extra python packages |
| extraLabels | object | `{}` | Any extra labels to apply to all resources |
| fullnameOverride | string | `""` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"locustio/locust"` |  |
| image.tag | string | `"2.4.0"` |  |
| imagePullSecrets | list | `[]` |  |
| ingress.annotations | object | `{}` |  |
| ingress.className | string | `""` |  |
| ingress.enabled | bool | `false` |  |
| ingress.hosts[0].host | string | `"chart-example.local"` |  |
| ingress.hosts[0].path | string | `"/"` |  |
| ingress.hosts[0].pathType | string | `"ImplementationSpecific"` |  |
| ingress.tls | list | `[]` |  |
| loadtest.environment | object | `{}` | environment variables used in the load test for both master and workers |
| loadtest.environment_external_secret | object | `{}` | environment variables used in the load test for both master and workers, stored in secrets created outside this chart. Each secret contains a list of values in it. Usage: `secret_name: [AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY]` |
| loadtest.environment_secret | object | `{}` | environment variables used in the load test for both master and workers, stored as secrets |
| loadtest.excludeTags | string | `""` | whether to run locust with `--exclude-tags [TAG [TAG ...]]` options, so only tasks with no matching tags will be executed |
| loadtest.headless | bool | `false` | whether to run locust with headless settings |
| loadtest.locustCmd | string | `"/usr/local/bin/locust"` | The command to run Locust |
| loadtest.locust_host | string | `"https://www.google.com"` | the host you will load test |
| loadtest.locust_lib_configmap | string | `"example-lib"` | name of a configmap containing your lib (default uses the example lib) |
| loadtest.locust_locustfile | string | `"main.py"` | the name of the locustfile |
| loadtest.locust_locustfile_configmap | string | `"example-locustfile"` | name of a configmap containing your locustfile (default uses the example locustfile) |
| loadtest.locust_locustfile_path | string | `"/mnt/locust"` | the path of the locustfile (without trailing backslash) |
| loadtest.mount_external_secret | object | `{}` | additional mount used in the load test for both master and workers, stored in secrets created outside this chart. Each secret contains a list of values in it. Usage: `mountPath: yourMountLocation, files: { secret_name: [AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY] }` |
| loadtest.name | string | `"example"` | a name used for resources and settings in this load test |
| loadtest.pip_packages | list | `[]` | a list of extra python pip packages to install |
| loadtest.tags | string | `""` | whether to run locust with `--tags [TAG [TAG ...]]` options, so only tasks with any matching tags will be executed |
| master.args | list | `[]` | Any extra command args for the master |
| master.auth.enabled | bool | `false` | When enabled, UI basic auth will be enforced with the given username and password |
| master.auth.password | string | `""` |  |
| master.auth.username | string | `""` |  |
| master.command[0] | string | `"sh"` |  |
| master.command[1] | string | `"/config/docker-entrypoint.sh"` |  |
| master.deploymentAnnotations | object | `{}` | Annotations on the deployment for master |
| master.environment | object | `{}` | environment variables for the master |
| master.envs_include_default | bool | `true` | Whether to include default environment variables |
| master.image | string | `""` | A custom docker image including tag |
| master.logLevel | string | `"INFO"` | Log level. Can be INFO or DEBUG |
| master.pdb.enabled | bool | `false` | Whether to create a PodDisruptionBudget for the master pod |
| master.resources | object | `{}` | resources for the locust master |
| master.restartPolicy | string | `"Always"` | master pod's restartPolicy. Can be Always, OnFailure, or Never. |
| master.serviceAccountAnnotations | object | `{}` |  |
| master.strategy.type | string | `"RollingUpdate"` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| podSecurityContext | object | `{}` |  |
| securityContext | object | `{}` |  |
| service.annotations | object | `{}` |  |
| service.extraLabels | object | `{}` |  |
| service.type | string | `"ClusterIP"` |  |
| tolerations | list | `[]` |  |
| worker.args | list | `[]` | Any extra command args for the workers |
| worker.command[0] | string | `"sh"` |  |
| worker.command[1] | string | `"/config/docker-entrypoint.sh"` |  |
| worker.deploymentAnnotations | object | `{}` | Annotations on the deployment for workers |
| worker.environment | object | `{}` | environment variables for the workers |
| worker.envs_include_default | bool | `true` | Whether to include default environment variables |
| worker.hpa.enabled | bool | `false` |  |
| worker.hpa.maxReplicas | int | `100` |  |
| worker.hpa.minReplicas | int | `1` |  |
| worker.hpa.targetCPUUtilizationPercentage | int | `40` |  |
| worker.image | string | `""` | A custom docker image including tag |
| worker.logLevel | string | `"INFO"` | Log level. Can be INFO or DEBUG |
| worker.replicas | int | `1` |  |
| worker.resources | object | `{}` | resources for the locust worker |
| worker.restartPolicy | string | `"Always"` | worker pod's restartPolicy. Can be Always, OnFailure, or Never. |
| worker.serviceAccountAnnotations | object | `{}` |  |
| worker.strategy.type | string | `"RollingUpdate"` |  |

