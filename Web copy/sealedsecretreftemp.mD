# Sealing a Kubernetes Secret with kubeseal

This guide explains how to create a Kubernetes Secret and seal it using `kubeseal` to ensure it can be safely stored in version control.

## Command Breakdown

```bash
kubectl create secret generic abc --from-literal key=123 --dry-run=client -o json | kubeseal --controller-namespace kube-system --controller-name sealedsecrets-sealed-secrets --scope namespace-wide --namespace strapi > abc.yaml
```

### Steps Explained

#### Create a Kubernetes Secret

```bash
kubectl create secret generic abc --from-literal key=123 --dry-run=client -o json
```

- **`kubectl create secret generic abc`**: Creates a generic secret named `abc`.
- **`--from-literal key=123`**: Adds a key-value pair to the secret (`key=123`).
- **`--dry-run=client`**: Ensures the command runs locally without creating the secret on the cluster.
- **`-o json`**: Outputs the secret in JSON format.

> **Note:** The name of the secret (`abc` in this case) must match the name used when unsealing the secret and applying it in tools like Argo CD. Consistency is crucial to ensure the key-value pair is properly referenced during deployment.

#### Pipe the Secret to kubeseal

```bash
| kubeseal --controller-namespace kube-system --controller-name sealedsecrets-sealed-secrets --scope namespace-wide --namespace strapi
```

- **`|`**: Pipes the output of the previous command to `kubeseal`.
- **`kubeseal`**: Seals the secret so it can be safely stored in version control.
  - **`--controller-namespace kube-system`**: Specifies the namespace where the sealed-secrets controller is running.
  - **`--controller-name sealedsecrets-sealed-secrets`**: Specifies the name of the sealed-secrets controller.
  - **`--scope namespace-wide`**: Sets the scope of the sealed secret to be namespace-wide.
  - **`--namespace strapi`**: Specifies the namespace where the sealed secret will be used.

#### Output the Sealed Secret to a File

```bash
> abc.yaml
```

- **`>`**: Redirects the output to a file.
- **`abc.yaml`**: The name of the file where the sealed secret will be saved.

## Summary

This command creates a Kubernetes secret with a key-value pair, seals it using `kubeseal`, and saves the sealed secret to a file named `abc.yaml`. The sealed secret can then be safely stored in version control and applied to the Kubernetes cluster.

## References

- [kubectl create secret](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create-secret)
- [kubeseal](https://github.com/bitnami-labs/sealed-secrets)

