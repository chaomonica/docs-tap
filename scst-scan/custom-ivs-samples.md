# Configure your custom ImageVulnerabilityScan samples for Supply Chain Security Tools - Scan

This topic provides you with sample ImageVulnerabilityScans for various scanners, and their associated secrets if required.

## <a id="overview"></a> ImageVulnerabilityScan samples

Below are sample ImageVulnerabilityScan (IVS) for various scanners. To use them, copy the YAML content and follow the instructions in the following section.

- [Carbon Black](./ivs-carbon-black.hbs.md)
- [Snyk](./ivs-snyk.hbs.md)
- [Prisma](./ivs-prisma.hbs.md)
- [Trivy](./ivs-trivy.hbs.md)

For information about integrating your own scanner, see [Integrate your own scanner](./app-scanning-alpha.hbs.md#integrate-your-own-scanner).

### <a id="use-samples"></a> Use custom ImageVulnerabilityScan samples

To use one of the samples:

1. Copy the sample YAML into a file named `custom-ivs.yaml`. Some scanners, such as Carbon Black, Snyk, and Prisma Scanner, require specific credentials that you must specifiy in the secret.
2. Obtain the one or more necessary images. For example, an image containing the scanner.
3. Edit these common fields of your ImageVulnerabilityScan:

   - `spec.image` is the image that you are scanning.
   - `scanResults.location` is the registry URL where results are uploaded. For example, `my.registry/scan-results`.
   - `serviceAccountNames` includes:
     - `scanner` is the service account that runs the scan. It must have read access to `image`.
     - `publisher` is the service account that uploads results. It must have write access to `scanResults.location`.
4. Complete any scanner specific changes specified in the sample.
5. You can either incorporate your custom ImageVulnerabilityScan into a [ClusterImageTemplate](./clusterimagetemplates.hbs.md) or run a standalone scan using:

  ```console
  kubectl apply -f custom-ivs.yaml -n DEV-NAMESPACE
  ```

  Where `DEV-NAMESPACE` is the name of the developer namespace where scanning occurs.