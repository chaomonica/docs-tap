# Integration Guide

This topic describes how to integrate Supply Chain Security Tools - Scan 2.0 with the Out of the Box Supply Chain.

## Prequesites

Before you can integrate Supply Chain Security Tools - Scan 2.0 with the Out of the Box Supply Chain:
- Create an ImageVulnerabilityScan. You can either select from the provided samples or integrate your scanner:
  - [ImageVulnerabilityScan samples](./ivs-custom-samples.hbs.md)
  - [Customize your own ImageVulnerabilityScan](./ivs-create-your-own.hbs.md)
- Create a ClusterImageTemplate u

## <a id="verifying-integration"></a> Verifying Integration

The follow sections describe how you can verify scanning of an image using the scanner of your choice.

### <a id="validate-scan-ivs"></a> Validate scanning using an ImageVulernabilityScan
To verify that you have you can scan an image using your ImageVulernabilityScan.

To trigger your scan:

1. Deploy your ImageVulnerabilityScan to the cluster by running:

    ```console
    kubectl apply -f image-vulnerability-scan.yaml -n DEV-NAMESPACE
    ```
   - Where `DEV-NAMESPACE` is the name of the developer namespace you want to use.

1. Child resources are created.

    - view the child PipelineRun, TaskRuns, and pods
      ```console
      kubectl get -l imagevulnerabilityscan pipelinerun,taskrun,pod -n DEV-NAMESPACE
      ```

1. When the scanning completes, the status is shown. Specify `-o wide` to see the digest of the image scanned and the location of the published results.

    ```console
    $ kubectl get imagevulnerabilityscans -n DEV-NAMESPACE -o wide

    NAME                 SCANRESULT                           SCANNEDIMAGE          SUCCEEDED   REASON
    generic-image-scan   registry/project/scan-results@digest nginx:latest@digest   True        Succeeded

    ```

### <a id="validate-view-results"> Validate by viewing results

Scan results are uploaded to the container image registry as an [imgpkg](https://carvel.dev/imgpkg/) bundle.
To retrieve a vulnerability report:

1. Retrieve the result location from the ImageVulnerabilityScan CR Status
   ```console
   SCAN_RESULT_URL=$(kubectl get imagevulnerabilityscan my-scan -o jsonpath='{.status.scanResult}')
   ```

1. Download the bundle to a local directory and list the content
   ```console
   imgpkg pull -b $SCAN_RESULT_URL -o myresults/
   ls myresults/
   ```