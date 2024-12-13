# operator-notes

Personal notes on operator-sdk.

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

