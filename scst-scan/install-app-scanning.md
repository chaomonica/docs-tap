# <a id="installing"></a> Installing SCST - Scan 2.0 in a cluster

The following sections describe how to install SCST - Scan 2.0.

## <a id='scst-app-scanning-prereqs'></a> Prerequisites

SCST - Scan 2.0 requires the following prerequisites:

- Complete all prerequisites to install Tanzu Application Platform. For more information, see [Prerequisites](../prerequisites.hbs.md).
- Install the [Tekton component](../tekton/install-tekton.hbs.md). Tekton is in the Full and Build profiles of Tanzu Application Platform.

## <a id='configure-app-scanning'></a> Configure properties

When you install SCST - Scan 2.0, you can configure the following optional properties:

| Key | Default | Type | Description |
| --- | --- | --- | --- |
| caCertData | "" | string | The custom certificates trusted by the scan's connections |
| docker.import | true | Boolean | Import `docker.pullSecret` from another namespace (requires secretgen-controller). Set to false if the secret is already present. |
| docker.pullSecret | registries-credentials | string | Name of a Docker pull secret in the deployment namespace to pull the scanner images |
| workspace.storageSize  | 100Mi | string | Size of the PersistentVolume that the Tekton pipelineruns uses |
| workspace.storageClass  | "" | string | Name of the storage class to use while creating the PersistentVolume claims used by tekton pipelineruns |

## <a id='install-scst-app-scanning'></a> Install

To install SCST - Scan 2.0:

1. List version information for the package by running:

    ```console
    tanzu package available list app-scanning.apps.tanzu.vmware.com --namespace tap-install
    ```

    For example:

    ```console
    $ tanzu package available list app-scanning.apps.tanzu.vmware.com --namespace tap-install
    - Retrieving package versions for app-scanning.apps.tanzu.vmware.com...
        NAME                                VERSION              RELEASED-AT
        app-scanning.apps.tanzu.vmware.com  0.1.0-beta          2023-03-01 20:00:00 -0400 EDT
    ```

1. (Optional) Make changes to the default installation settings:

    Create an `app-scanning-values-file.yaml` file which contains any changes to the default installation settings.

    Retrieve the configurable settings and append the key-value pairs to be modified to the `app-scanning-values-file.yaml` file:

    ```console
    tanzu package available get app-scanning.apps.tanzu.vmware.com/VERSION --values-schema --namespace tap-install
    ```

    Where `VERSION` is your package version number. For example, `0.1.0-beta`.

    For example:

    ```console
    tanzu package available get app-scanning.apps.tanzu.vmware.com/0.1.0-beta --values-schema --namespace tap-install
    | Retrieving package details for app-scanning.apps.tanzu.vmware.com/0.1.0-beta...

      KEY                     DEFAULT                 TYPE     DESCRIPTION
      docker.import           true                    boolean  Import `docker.pullSecret` from another namespace (requires
                                                               secretgen-controller). Set to false if the secret will already be present.
      docker.pullSecret       registries-credentials  string   Name of a docker pull secret in the deployment namespace to pull the scanner
                                                               images.
      workspace.storageSize   100Mi                   string   Size of the Persistent Volume to be used by the tekton pipelineruns
      workspace.storageClass                          string   Name of the storage class to use while creating the Persistent Volume Claims
                                                               used by tekton pipelineruns
      caCertData                                      string   The custom certificates to be trusted by the scan's connections
    ```

2. Install the package by running:

    ```console
    tanzu package install app-scanning-alpha --package-name app-scanning.apps.tanzu.vmware.com \
        --version VERSION \
        --namespace tap-install \
        --values-file app-scanning-values-file.yaml
    ```

    Where `VERSION` is your package version number. For example, `0.1.0-beta`.

    For example:

    ```console
    tanzu package install app-scanning-alpha --package-name app-scanning.apps.tanzu.vmware.com \
        --version 0.1.0-beta\
        --namespace tap-install \
        --values-file app-scanning-values-file.yaml

        Installing package 'app-scanning.apps.tanzu.vmware.com'
        Getting package metadata for 'app-scanning.apps.tanzu.vmware.com'
        Creating service account 'app-scanning-default-sa'
        Creating cluster admin role 'app-scanning-default-cluster-role'
        Creating cluster role binding 'app-scanning-default-cluster-rolebinding'
        Creating package resource
        Waiting for 'PackageInstall' reconciliation for 'app-scanning'
        'PackageInstall' resource install status: Reconciling
        'PackageInstall' resource install status: ReconcileSucceeded
    ```