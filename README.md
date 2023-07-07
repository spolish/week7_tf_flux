## Task: Create K8S cluster, deploy flux and make CD in GitOps way for application "kbot"

Modules that will be used:
```
https://github.com/den-vasyliev/tf-google-gke-cluster

github.com/den-vasyliev/tf-fluxcd-flux-bootstrap

github.com/den-vasyliev/tf-github-repository

github.com/den-vasyliev/tf-hashicorp-tls-keys
```

### Prerequisites
install **terraform** and **flux**

### 1. Instalation

Copy repo and run terraform
```
git clone https://github.com/spolish/week7_tf_flux

terraform init

terraform apply
```

####  **Note: Terraform will reqier credentials from GitHub and name of project in Google Cloud**

### 2. Created Resources
			2.1 K8S cluster
			img
			2.2 Repo in Github
			img
			2.3 Flux in K8s cluster
			img
### 3. In repo **flux-gitops**/cluster create folder for app deployment
				3.1 ns.yaml
				``` 
				apiVersion: v1
				kind: Namespace
				metadata:
				  name: demo
				```
                
				3.2 kbot-gr.yaml
    ```
        ---
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: kbot
  namespace: demo
spec:
  url: http://github.com/spolish/kbot
  interval: 1m0s
  ref:
    branch: docker-flux
    ```

				3.3 kbot-hr.yaml
    ``` 
  ---
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: kbot
  namespace: demo
spec:
  chart:
    spec:
      chart: ./helm
      reconcileStrategy: Revision
      sourceRef:
        kind: GitRepository
        name: kbot
  interval: 1m0s
    ```
    