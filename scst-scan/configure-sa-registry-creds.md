## <a id="config-sa-reg-creds"></a> Configure Service Accounts and Registry Credentials

The following sections describe how to configure service accounts and registry credentials.

The following access is required:

  - Read access to the registry containing the Tanzu Application Platform bundles. This is the registry from the [Relocate images to a registry](../install-online/profile.hbs.md#relocate-images-to-a-registry) step or `registry.tanzu.vmware.com`.
  - Read access to the registry containing the image to scan, if scanning a private image
  - Write access to the registry to which results are published

1. Create a secret `scanning-tap-component-read-creds` with read access to the registry containing the Tanzu Application Platform bundles. This pulls the SCST - Scan 2.0 images.

    >**Important** If you followed the directions for [Install Tanzu Application Platform](../install-intro.hbs.md), skip this step and use the `tap-registry` secret with your service account.

    ```console
    read -s TAP_REGISTRY_PASSWORD
    kubectl create secret docker-registry scanning-tap-component-read-creds \
      --docker-username=TAP-REGISTRY-USERNAME \
      --docker-password=$TAP_REGISTRY_PASSWORD \
      --docker-server=TAP-REGISTRY-URL \
      -n DEV-NAMESPACE
    ```

    Where `DEV-NAMESPACE` is the developer namespace where scanning occurs.

2. If you are scanning a private image, create a secret `scan-image-read-creds` with read access to the registry containing that image.

    >**Important** If you followed the directions for [Install Tanzu Application Platform](../install-intro.hbs.md), you can skip this step and use the `targetImagePullSecret` secret with your service account as referenced in your tap-values.yaml [here](../install-online/profile.hbs.md#full-profile).

    ```console
    read -s REGISTRY_PASSWORD
    kubectl create secret docker-registry scan-image-read-creds \
      --docker-username=REGISTRY-USERNAME \
      --docker-password=$REGISTRY_PASSWORD \
      --docker-server=REGISTRY-URL \
      -n DEV-NAMESPACE
    ```

3. Create a secret `write-creds` with write access to the registry for the scanner to upload the scan results to.

    ```console
    read -s WRITE_PASSWORD
    kubectl create secret docker-registry write-creds \
      --docker-username=WRITE-USERNAME \
      --docker-password=$WRITE_PASSWORD \
      --docker-server=DESTINATION-REGISTRY-URL \
      -n DEV-NAMESPACE
    ```

4. Create the service account `scanner` which enables SCST - Scan 2.0 to pull the image to scan. Attach the read secret created earlier under `imagePullSecrets` and the write secret under `secrets`.

    ```yaml
    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: scanner
      namespace: DEV-NAMESPACE
    imagePullSecrets:
    - name: scanning-tap-component-read-creds
    secrets:
    - name: scan-image-read-creds
    ```

    Where:

    - `imagePullSecrets.name` is the name of the secret used by the component to pull the scan component image from the registry.
    - `secrets.name` is the name of the secret used by the component to pull the image to scan. This is required if the image you are scanning is private.

5. Create the service account `publisher` which enables SCST - Scan 2.0 to push the scan results to a user specified registry.

    ```yaml
    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: publisher
      namespace: DEV-NAMESPACE
    imagePullSecrets:
    - name: scanning-tap-component-read-creds
    secrets:
    - name: write-creds
    ```

    Where:
    - `imagePullSecrets.name` is the name of the secret used by the component to pull the scan component image from the registry.
    - `secrets.name` is the name of the secret used by the component to publish the scan results.