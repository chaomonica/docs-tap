## <a id="observability"></a> Observability

To watch the status of the scanning custom resources and child resources:

```console
kubectl get grypeimagevulnerabilityscan,imagevulnerabilityscan
kubectl get -l imagevulnerabilityscan pipelinerun,taskrun,pod
```

View the status, reason, and urls:

```console
kubectl get grypeimagevulnerabilityscan -o wide
kubectl get imagevulnerabilityscan -o wide
```

View the complete status and events of scanning custom resources:

```console
kubectl describe grypeimagevulnerabilityscan
kubectl describe imagevulnerabilityscan
```

List the child resources of a scan:

```console
kubectl get -l grypeimagevulnerabilityscan=$NAME pipelinerun,taskrun,pod,configmap
kubectl get -l imagevulnerabilityscan=$NAME pipelinerun,taskrun,pod
```

Get the logs of the controller:

```console
kubectl logs -f deployment/app-scanning-controller-manager -n app-scanning-system -c manager
```