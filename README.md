# troubleshooting_wsl

## While on VPN (Cisco), WSL may not be able to reach the internet.
Run the following commands in powershell (admin mode)
```
> Get-NetIPInterface -InterfaceAlias "vEthernet (WSL)" | Set-NetIPInterface -InterfaceMetric 1
> Get-NetAdapter | Where-Object {$_.InterfaceDescription -Match "Cisco AnyConnect"} | Set-NetIPInterface -InterfaceMetric 6000
```

# Using MINIKUBE on Windows with KUBECTL on WSL2
### https://medium.com/@joaoh82/setting-up-kubernetes-on-wsl-to-work-with-minikube-on-windows-10-90dac3c72fa1
This is another post on setting up your development environment to work with WSL (Windows Subsystem for Linux) and Window 10 Pro.
Last time was about setting up Docker to on your WSL box to work with your Docker Engine on your Windows, so you could work transparently between the environments. Here is the link, in case you want to check it out.
https://medium.com/@joaoh82/setting-up-docker-toolbox-for-windows-home-10-and-wsl-to-work-perfectly-2fd34ed41d51

Now we are tackling kubectl and minikube and make kubectl on your WSL to work with minikube on you Windows 10. So let’s go!
A few step you have to take on your own are:
	• Install minikube for Windows (not the Linux version in WSL, it won’t work). — https://kubernetes.io/docs/tasks/tools/install-minikube/
	• Install docker and kubectl in WSL.
kubectl — https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux
docker — https://docs.docker.com/install/linux/docker-ce/ubuntu/
Now let’s make WSL use minikube on Windows when you type minikube on WSL:
Create a simple bash script named minikube to run the Windows version of minikube and put it in your path in your WSL environment. What I did was create a folder called minikube in my $HOME directory on WSL and added that folder to my $PATH by adding export PATH=”$HOME/minikube:$PATH” to my $HOME/.profile . Anyway, here is the contents of the script:
```
#!/bin/sh
/mnt/c/ProgramData/chocolatey/bin/minikube.exe $@
```
Now when you type minikube on WSL you will get the minikube on Windows. BOOM!!
Moving on!
Let’s now start minikube (from Windows or WSL is fine) by running minikube start . This will configure Windows kubectl to be able to talk to Kubernetes in minikube, but the kubectl in WSL still won’t work. To fix that, open %userprofile%\.kube\config on Windows in a text editor and copy all the minikube context, cluster, and user from there into ~/.kube/config in your WSL environment, feel free to create the config file if it doesn’t exists.
Here it comes the catch now. We need to change a couple of things in the config file in WSL to find the certificates in the Windows machine.
Let’s say that this is your config below. You will need to change the certificate-authority , client-certificate and the client-key so the WSL can find it.
```
apiVersion: v1
clusters:
- cluster:
		certificate-authority: C:\Users\[YOUR-USER]\.minikube\ca.crt
		server: https://172.17.29.90:8443
  name: minikube
contexts:
- context:
	cluster: minikube
	user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
	client-certificate: C:\Users\[YOUR-USER]\.minikube\client.crt
	client-key: C:\Users\[YOUR-USER]\.minikube\client.key
```
New version of the config file in the WSL box:
```
apiVersion: v1
clusters:
- cluster:
	certificate-authority: /mnt/c/Users/[YOUR-USER]/.minikube/ca.crt
	server: https://172.17.29.90:8443
  name: minikube
contexts:
- context:
	cluster: minikube
	user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
	client-certificate: /mnt/c/Users/[YOUR-USER]/.minikube/client.crt
	client-key: /mnt/c/Users/[YOUR-USER]/.minikube/client.key
```	
Also, there is one last thing you need to do. You need to add this line to your .bashrc file:
```
export DOCKER_CERT_PATH=/mnt/c/Users/[YOUR-USER]/.minikube/certs
```
And then, to finish it off: source ~/.bashrc
And that is it. Feel free to run kubectl get all to test it. Or any other kubectl command.
