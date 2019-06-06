`"Binary stream "0" does not contain valid BinaryHeader. This may be due to invalid flow, or due to changes in the object version between serialization and serialization.`

See: https://github.com/docker/for-win/issues/1232
* VirtualBox is not compatible with Hyper-V, and Docker Desktop needs Hyper-V to run linux containers. Turn off VirtualBox.
* Windows update may have broken a component. Users have had success reinstalling file/printer sharing in windows: https://github.com/docker/for-win/issues/1232#issuecomment-338588366


`Unable to create: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: Hyper-V encountered an error trying to access an object on computer 'EMERIO-USER' because the object was not found. The object might have been deleted. Verify that the Virtual Machine Management service on the computer is running.
at New-Switch, : line 121`

`Unable to create: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: Generic failure`

See https://github.com/docker/for-win/issues/1221
* Your Hyper-V configuration is likely broken. This may be caused by a windows update.
* See https://github.com/docker/for-win/issues/1221#issuecomment-493118403 for the 'control flow guard' fix

`Unable to create: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: Generic failure
at New-Switch, : line 121`

Cause: Broken network adapter (perhaps caused by windows update)
1. Close docker desktop
1. Remove the VM `powershell -ExecutionPolicy ByPass -File "C:\Program Files\Docker\Docker\resources\MobyLinux.ps1" -destroy`
2. Remove the broken network adapter https://blogs.msdn.microsoft.com/jjameson/2011/03/14/removing-stale-network-adapters-in-hyper-v-vm/
3. Reset docker desktop to factory defaults
4. Restart docker desktop

`Unable to start: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: 'MobyLinuxVM' failed to start. (Virtual machine ID E2B4DEA4-1300-43B4-9D49-BBCC2DEEC5ED)`

`'MobyLinuxVM' failed to start worker process: Windows cannot verify the digital signature for this file. A recent hardware or software change might have installed a file that is signed incorrectly or damaged, or that might be malicious software from an unknown source. `

See tickets https://github.com/docker/for-win/issues?q=is%3Aissue+%3A+Windows+cannot+verify+the+digital+signature+for+this+file+is%3Aclosed
Likely already fixed, but its also been related to virus scanners in the past. Check your virus scanner is up to date.

`Unable to start: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: 'MobyLinuxVM' failed to change state.
at Start-MobyLinuxVM, <No file>: line 296`

Cause: Broken network adapter (perhaps caused by windows update)
1. Close docker desktop
1. Remove the VM `powershell -ExecutionPolicy ByPass -File "C:\Program Files\Docker\Docker\resources\MobyLinux.ps1" -destroy`
2. Remove the broken network adapter https://blogs.msdn.microsoft.com/jjameson/2011/03/14/removing-stale-network-adapters-in-hyper-v-vm/
3. Reset docker desktop to factory defaults
4. Restart docker desktop

`Unable to create: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: Inconsistent parameters PolicyStore PersistentStore and Dhcp Enabled`

See: https://github.com/docker/for-win/issues/2050
Are you using Cisco AnyConnect?
- stop Cisco agent, restart docker, start Cisco agent.

`Unable to start: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: 'MobyLinuxVM' failed to start.`
`Failed to start the virtual machine 'MobyLinuxVM' because one of the Hyper-V components is not running.`
`'MobyLinuxVM' failed to start. (Virtual machine ID E29796F3-81DE-4BA7-81DC-986110B3E602)`
`The Virtual Machine Management Service failed to start the virtual machine 'MobyLinuxVM' because one of the Hyper-V components is not running (Virtual machine ID E29796F3-81DE-4BA7-81DC-986110B3E602).
at Start-MobyLinuxVM, : line 287`

* Maybe VirtualBox or other 3rd party software preventing Hyper-V from running (e.g. 3rd party virus scanners)

Could you check if the following Hyper-V components are running?

* Hyper-V Host Compute Service
* Hyper-V Virtual Machine Management

`no space left on device`
- Typically  you've run out of space because of images, volumes, logs etc. Various ways of looking at whats there and deleting it.
- https://stackoverflow.com/questions/30604846/docker-error-no-space-left-on-device
- https://unix.stackexchange.com/questions/203168/docker-says-no-space-left-on-device-but-system-has-plenty-of-space


`Access Denied: Unable to start Docker, you must be part of the docker-users group`

Cause: Active directory / user account permissions setup

See https://github.com/docker/for-win/issues/868
1. You must restart after install
2. Login to active directory and add yourself into docker-users group. using local account may fail. (https://github.com/docker/for-win/issues/868#issuecomment-352279510)

`Unable to send Start: Unable to launch process: 1260`

?

`Error creating default "bridge" network: available `
?

`Cannot start service xyz: failed to create endpoint XYZ on network nat: HNS failed with error : The process cannot access the file because it is being used by another process.`

You are using the same port twice in two containers.

## Docker stops working after a while. Docker takes too much CPU / Memory.

Firstly be sure its not containers running, and if running in Linux container mode, check which processes are causing the issue:
Inspect the memory usage of the VM processes:

Enter the Linux VM shell
`docker run -it --privileged --pid=host justincormack/nsenter1`

List processes
`top`

Sort by memory then show RSS
`{shift+m} {shift+s}`

You should see something like...
```
Mem total:2027864 anon:718184 map:455228 free:91652
 slab:105852 buf:23384 cache:892000 dirty:140 write:0
Swap total:1048572 free:1043352
  PID   VSZ^VSZRW^  RSS (SHR) DIRTY (SHR) STACK COMMAND
 1947 1006m  350m 31852     4 10844     0   132 containerd --config /var/run/docker/containerd/containerd.toml --log-level debug
 3840  463m  304m  333m     4  179m     0   132 kube-apiserver --advertise-address=192.168.65.3 --allow-privileged=true --authorization-mode=Node,RBAC --clie
 1940  490m  268m 71024     4 28532     0   132 /usr/local/bin/dockerd -H unix:///var/run/docker.sock --config-file /run/config/docker/daemon.json --swarm-de
 1337  139m  117m 39828     4 19988     0   132 /usr/bin/containerd
 2570  226m  104m 99512   436 35608     0   132 kubelet --kubeconfig=/etc/kubernetes/kubelet.conf --config /etc/kubeadm/kubelet.yaml --bootstrap-kubeconfig=/
 1538  106m  104m  104m   424  104m     0   132 rpc.statd
...
```
