# <a id="setup-for-ootb-supply-chains"></a> Setup for OOTB Supply Chains

The following section describe how to integration SCST - Scan 2.0 with the Out of the Box Supply Chain.

## <a id="steps-integration-with-ootb-supply-chain"></a> Steps to integration with the OOTB Supply Chain

1. Bring your own scanner by creating ImageVulnerabilityScan CRs. See [Configure your custom ImageVulnerabilityScan samples](./custom-ivs-samples.md) for how to creating ImageVulnerabilityScan samples.

1. Create ClusterImageTemplate. See [Author a ClusterImageTemplate for Supply Chain integration](./clusterimagetemplates.hbs.md) for how to create ClusterImageTemplate.

1. Configure supply chain with an ImageVulnerabilityScan

   1. View available ClusterImageTemplates by running:

     ```console
     kubectl get clusterimagetemplates
     ```

   2. Update your `tap-values.yaml` file to specify the ClusterImageTemplate. For example:

    ```yaml
    ootb_supply_chain_testing_scanning:
    image_scanner_template_name: CLUSTERIMAGETEMPLATE
    ```
    Where `CLUSTERIMAGETEMPLATE` is the ClusterImageTemplate with the embdedded ImageVulnerabilityScan using the scanner of your choice.