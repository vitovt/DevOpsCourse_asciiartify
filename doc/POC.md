# Deploy ArgoCD

Here's a detailed guide for deploying a Kubernetes cluster and configuring team access to the ArgoCD GUI.


## Step 1: Gather Cluster Information

First, check the information about your Kubernetes cluster:

```sh
kubectl cluster-info
```

This command displays the master and services addresses of your Kubernetes cluster.

```sh
kubectl get all
```

This lists all resources in the default namespace, helping you understand what's currently running.

## Step 2: Create ArgoCD Namespace

ArgoCD requires its own namespace in Kubernetes:

```sh
kubectl create namespace argocd
```

This command creates a new namespace named 'argocd'.

Verify the creation of the namespace:

```sh
kubectl get ns
```

This lists all the namespaces, including the newly created 'argocd' namespace.

## Step 3: Install ArgoCD

To install ArgoCD, first, download the installation manifest:

```sh
curl https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml | vim -R -
```

This command downloads the manifest and opens it in read-only mode using Vim. You **allways must** inspect the manifest before applying it.

Apply the ArgoCD manifest:

```sh
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

This applies the ArgoCD manifest in the 'argocd' namespace.

## Step 4: Verify ArgoCD Installation

Check the resources in the ArgoCD namespace:

```sh
kubectl get all -n argocd
```

This lists all ArgoCD components installed in the 'argocd' namespace.

## Step 5: Access the ArgoCD GUI

Port-forward the ArgoCD server service to access the GUI:

```sh
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

This command forwards the local port 8080 to the ArgoCD server's port 443.

For external access, replace `<external-ip>` with your machine's external IP:

```sh
kubectl port-forward svc/argocd-server -n argocd --address <external-ip> 8080:443
```

## Step 6: Access ArgoCD via HTTPS

Use this command to bypass HTTPS certificate verification:

```sh
curl --insecure https://localhost:8080
```

This allows you to access the ArgoCD GUI via an insecure connection for initial setup.

## Step 7: Obtain ArgoCD Initial Admin Password

Retrieve the initial admin password for ArgoCD:

```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

This extracts the encoded password, decodes it, and displays it.

## Step 8: Log into ArgoCD GUI

Access the ArgoCD GUI at:

`https://localhost:8080`

Use these credentials:
- Username: `admin`
- Password: `<decoded pass>` (obtained from the previous step)

Now, you can log into the ArgoCD GUI and start managing your Kubernetes applications!

Remember to replace `<external-ip>` with your actual external IP address when setting up external access. This guide assumes a basic understanding of Kubernetes and command-line usage.