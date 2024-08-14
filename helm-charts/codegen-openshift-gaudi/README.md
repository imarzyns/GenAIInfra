# CodeGen

Helm chart for deploying CodeGen service on Red Hat Openshift with Gaudi accelerator.

## Installing the Chart

To install the chart, run the following:

```console
cd GenAIInfra/helm-charts/
./update_dependency.sh
helm dependency update codegen
export NAMESPACE="CODEGEN_NAMESPACE"
export CLUSTERDOMAIN="$(oc get Ingress.config.openshift.io/cluster -o jsonpath='{.spec.domain}' | sed 's/^apps.//')"
export HFTOKEN="HUGGINGFACE_TOKEN"
export MODELDIR="/mnt/opea-models"
export MODELNAME="meta-llama/CodeLlama-7b-hf"

helm install codegen codegen-openshift-gaudi --set codegen.image.repository=image-registry.openshift-image-registry.svc:5000/${NAMESPACE}/codegen --set llm-uservice.llmUservice.image.repository=image-registry.openshift-image-registry.svc:5000/${NAMESPACE}/llm-tgi --set codegen-react-ui.reactUi.image.repository=image-registry.openshift-image-registry.svc:5000/${NAMESPACE}/react-ui --set global.clusterDomain=${CLUSTERDOMAIN} --set global.huggingfacehubApiToken=${HFTOKEN}
```

## Verify

To verify the installation, run the command oc get pods to make sure all pods are running. Wait about 5 minutes for building images. When 3 pods achieve Completed status, the rest with services should go to Running.

## Launch the UI

To access the frontend, find the route for react-ui with command `oc get routes` and open it in the browser.