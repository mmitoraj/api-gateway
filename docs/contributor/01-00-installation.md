# Install API Gateway
- [Install API Gateway](#Install-api-gateway)
  - [Prerequisites](#prerequisites)
  - [Install Kyma API Gateway Operator manually](#install-kyma-api-gateway-operator-manually)
  - [Install the API Gateway module with Lifecycle Manager locally on k3d](#install-the-api-gateway-module-with-lifecycle-manager-locally-on-k3d)
    - [Install using the ModuleTemplate generated by the local build](#install-using-the-moduletemplate-generated-by-the-local-build)

## Prerequisites

- For API Gateway to work, the [Istio module](https://github.com/kyma-project/istio) must be installed in the cluster.
- Access to a Kubernetes cluster (you can use [k3d](https://k3d.io/v5.5.1/))
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [kubebuilder](https://book.kubebuilder.io/)
- [Docker](https://www.docker.com)
- [Kyma CLI](https://kyma-project.io/#/04-operation-guides/operations/01-install-kyma-CLI)

## Install Kyma API Gateway Operator manually

1. Clone the project.

```bash
git clone https://github.com/kyma-project/api-gateway.git && cd api-gateway
```

1. Set the API Gateway Operator image name.

```bash
export IMG=api-gateway-operator:0.0.1
export K3D_CLUSTER_NAME=kyma
```

3. Provision the k3d cluster.

```bash
kyma provision k3d
```
>**TIP:** To verify the correctness of the project, build it using the `make build` command.

4. Build the image.

```bash
make docker-build
```

5. Push the image to the registry.

<div tabs name="Push image" group="api-gateway-operator-installation">
  <details>
  <summary label="k3d">
  k3d
  </summary>

   ```bash
   k3d image import $IMG -c $K3D_CLUSTER_NAME
   ```

  </details>
  <details>
  <summary label="Docker registry">
  Globally available Docker registry
  </summary>

   ```bash
   make docker-push
   ```

  </details>
</div>

6. Deploy API Gateway Operator.

```bash
make deploy
```

## Install the API Gateway module with Lifecycle Manager locally on k3d

Use make to install the API Gateway module with Lifecycle Manager locally.

### Install using the ModuleTemplate generated by the local build

1. Provision the k3d cluster, deploy Lifecycle-Manager, build the module, and deploy it.

```bash
make local-run
```

2. Delete the k3d cluster.

```bash
make local-stop
```

## Install the API Gateway module from the latest release

### Procedure

1. To install API Gateway, you must install the latest version of Kyma API Gateway Operator and API Gateway CustomResourceDefinition first. Run:

   ```bash
   kubectl apply -f https://github.com/kyma-project/api-gateway/releases/latest/download/api-gateway-manager.yaml
   ```

2. Apply the default API Gateway custom resource (CR):

   ```bash
   kubectl apply -f https://github.com/kyma-project/api-gateway/releases/latest/download/apigateway-default-cr.yaml
   ```

   You should get a result similar to this example:

   ```bash
   apigateways.operator.kyma-project.io/default created
   ```

3. Check the state of API Gateway CR to verify if API Gateway was installed successfully:

   ```bash
   kubectl get apigateways/default
   ```

   After successful installation, you get the following output:

   ```bash
   NAME      STATE
   default   Ready
   ```

## Useful links

To learn how to use Kyma API Gateway Operator, read the documentation in the [`user`](/docs/user) directory.

If you are interested in the detailed documentation of the Kyma API Gateway Operator's design and technical aspects, check the [`contributor`](/docs/contributor/) directory.