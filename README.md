# operator-notes

Personal notes on operator-sdk.

## Pre-installation

Make sure you have go installed. If not, here are one-liners.
ARM:
```
wget https://go.dev/dl/go1.21.0.linux-arm64.tar.gz -O /tmp/go.tar.gz && sudo tar -C /usr/local -xzf /tmp/go.tar.gz && echo "export PATH=\$PATH:/usr/local/go/bin" >> ~/.bashrc && source ~/.bashrc && go version
```
Intel:
```
wget https://go.dev/dl/go1.21.0.linux-amd64.tar.gz -O /tmp/go.tar.gz && \
sudo tar -C /usr/local -xzf /tmp/go.tar.gz && \
echo "export PATH=\$PATH:/usr/local/go/bin" >> ~/.bashrc && \
source ~/.bashrc && \
go version
sudo apt update && sudo apt install -y make
```

## Installation


Download the binaries and install it.
ARM:
```bash
curl -LO "https://github.com/operator-framework/operator-sdk/releases/latest/download/operator-sdk_linux_arm64"
chmod +x operator-sdk_linux_arm64
sudo mv operator-sdk_linux_arm64 /usr/local/bin/operator-sdk
```
Intel:
```bash
curl -LO "https://github.com/operator-framework/operator-sdk/releases/latest/download/operator-sdk_linux_amd64"
chmod +x operator-sdk_linux_amd64
sudo mv operator-sdk_linux_amd64 /usr/local/bin/operator-sdk

```

Check
```bash
ubuntu@instance-20240415-1500:~$ operator-sdk version
operator-sdk version: "v1.38.0", commit: "0735b20c84e5c33cda4ed87daa3c21dcdc01bb79", kubernetes version: "1.30.0", go version: "go1.22.5", GOOS: "linux", GOARCH: "arm64"
ubuntu@instance-20240415-1500:~$ 
```

## Scaffold the Operator

Use the Operator SDK to create the boilerplate code.

Initialize the operator project in a new directory

```bash
mkdir ipsec-route-operator
cd ipsec-route-operator/
operator-sdk init --domain=smo.juniper.net --repo=github.com/robric/operator-notes
```
This sets up the project with Kubebuilder, which is embedded in the SDK.
```
ubuntu@vm1:~$ mkdir ipsec-route-operator/
ubuntu@vm1:~$ cd ipsec-route-operator/
ubuntu@vm1:~/ipsec-route-operator$ operator-sdk init --domain=smo.juniper.net --repo=github.com/robric/operator-notes
INFO[0000] Writing kustomize manifests for you to edit... 
INFO[0000] Writing scaffold for you to edit...          
INFO[0000] Get controller runtime:
$ go get sigs.k8s.io/controller-runtime@v0.18.4 
INFO[0000] Update dependencies:
$ go mod tidy           
Next: define a resource with:
$ operator-sdk create api
```

This is how it looks like:

```
ubuntu@vm1:~/ipsec-route-operator$ tree
.
├── Dockerfile
├── Makefile
├── PROJECT
├── README.md
├── api
│   └── v1
│       ├── groupversion_info.go
│       ├── ipsecroute_types.go
│       └── zz_generated.deepcopy.go
├── bin
│   ├── controller-gen -> /home/ubuntu/ipsec-route-operator/bin/controller-gen-v0.15.0
│   └── controller-gen-v0.15.0
├── cmd
│   └── main.go
├── config
│   ├── crd
│   │   ├── bases
│   │   │   └── ipsecroutes.yaml
│   │   ├── kustomization.yaml
│   │   └── kustomizeconfig.yaml
│   ├── default
│   │   ├── kustomization.yaml
│   │   ├── manager_metrics_patch.yaml
│   │   └── metrics_service.yaml
│   ├── manager
│   │   ├── kustomization.yaml
│   │   └── manager.yaml
│   ├── manifests
│   │   └── kustomization.yaml
│   ├── prometheus
│   │   ├── kustomization.yaml
│   │   └── monitor.yaml
│   ├── rbac
│   │   ├── ipsecroute_editor_role.yaml
│   │   ├── ipsecroute_viewer_role.yaml
│   │   ├── kustomization.yaml
│   │   ├── leader_election_role.yaml
│   │   ├── leader_election_role_binding.yaml
│   │   ├── metrics_auth_role.yaml
│   │   ├── metrics_auth_role_binding.yaml
│   │   ├── metrics_reader_role.yaml
│   │   ├── role.yaml
│   │   ├── role_binding.yaml
│   │   └── service_account.yaml
│   ├── samples
│   │   ├── app_v1_ipsecroute.yaml
│   │   └── kustomization.yaml
│   └── scorecard
│       ├── bases
│       │   └── config.yaml
│       ├── kustomization.yaml
│       └── patches
│           ├── basic.config.yaml
│           └── olm.config.yaml
├── go.mod
├── go.sum
├── hack
│   └── boilerplate.go.txt
├── internal
│   └── controller
│       ├── ipsecroute_controller.go
│       ├── ipsecroute_controller_test.go
│       └── suite_test.go
└── test
    ├── e2e
    │   ├── e2e_suite_test.go
    │   └── e2e_test.go
    └── utils
        └── utils.go

22 directories, 47 files
ubuntu@vm1:~/ipsec-route-operator$ 
```
# Create a Custom Resource Definition (CRD)

Generate the API and CRD for a custom resource.

```
operator-sdk create api --group=app --version=v1 --kind=IpSecRoute --resource --controller
```

* --group: The API group for your resource, e.g., app.example.com.
* --version: The API version, e.g., v1.
* --kind: The name of your resource, e.g., MyCustomResource.

```
ubuntu@instance-20240415-1500:~/my-operator/api/v1$ ls
groupversion_info.go  myfirstcrd_types.go  zz_generated.deepcopy.go
ubuntu@instance-20240415-1500:~/my-operator/api/v1$

```

We will define a CRD to add routes:

``` 
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: ipsecroutes.app.example.com
spec:
  group: app.example.com
  names:
    kind: IpSecRoute
    plural: ipsecroutes
    singular: ipsecroute
  scope: Namespaced
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                routes:
                  type: array       # List of routes
                  items:
                    type: string    # Each route is a string (e.g., "5.6.7.8/32")
``` 






