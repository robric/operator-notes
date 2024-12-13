# operator-notes

Personal notes on operator-sdk.

## Pre-installation

Make sure you have go installed. If not, here is the one-liner for ARM install:
```
wget https://go.dev/dl/go1.21.0.linux-arm64.tar.gz -O /tmp/go.tar.gz && sudo tar -C /usr/local -xzf /tmp/go.tar.gz && echo "export PATH=\$PATH:/usr/local/go/bin" >> ~/.bashrc && source ~/.bashrc && go version
```

## Installation


Download the ARM binary and install it.

```bash
curl -LO "https://github.com/operator-framework/operator-sdk/releases/latest/download/operator-sdk_linux_arm64"
chmod +x operator-sdk_linux_arm64
sudo mv operator-sdk_linux_arm64 /usr/local/bin/operator-sdk
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
mkdir my-operator
cd my-operator
operator-sdk init --domain=my-notes-on-operators.com --repo=github.com/robric/operator-notes
```
This sets up the project with Kubebuilder, which is embedded in the SDK.

# Create a Custom Resource Definition (CRD)

Generate the API and CRD for a custom resource.

```
operator-sdk create api --group=app --version=v1 --kind=MyFirstCRD --resource --controller
```

* --group: The API group for your resource, e.g., app.example.com.
* --version: The API version, e.g., v1.
* --kind: The name of your resource, e.g., MyCustomResource.

```
ubuntu@instance-20240415-1500:~/my-operator/api/v1$ ls
groupversion_info.go  myfirstcrd_types.go  zz_generated.deepcopy.go
ubuntu@instance-20240415-1500:~/my-operator/api/v1$

```


