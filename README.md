# troubleshooting_wsl

## While on VPN (Cisco), WSL may not be able to reach the internet.
Run the following commands in powershell (admin mode)
```
> Get-NetIPInterface -InterfaceAlias "vEthernet (WSL)" | Set-NetIPInterface -InterfaceMetric 1
> Get-NetAdapter | Where-Object {$_.InterfaceDescription -Match "Cisco AnyConnect"} | Set-NetIPInterface -InterfaceMetric 6000
```

# Using MINIKUBE on Windows with KUBECTL on WSL2
