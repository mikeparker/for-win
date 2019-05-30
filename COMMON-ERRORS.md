`"Binary stream "0" does not contain valid BinaryHeader. This may be due to invalid flow, or due to changes in the object version between serialization and serialization.`

See: https://github.com/docker/for-win/issues/1232
* VirtualBox is not compatible with Hyper-V, and Docker Desktop needs Hyper-V to run linux containers. Turn off VirtualBox.
* Windows update may have broken a component. Users have had success reinstalling file/printer sharing in windows: https://github.com/docker/for-win/issues/1232#issuecomment-338588366


`Unable to create: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: Hyper-V encountered an error trying to access an object on computer 'EMERIO-USER' because the object was not found. The object might have been deleted. Verify that the Virtual Machine Management service on the computer is running.
at New-Switch, : line 121`

`Unable to create: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: Generic failure`

See https://github.com/docker/for-win/issues/1221
* Windows updates may have broken some Hyper-V components. See https://github.com/docker/for-win/issues/1221#issuecomment-493118403 for the 'control flow guard' fix

`Unable to create: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: Generic failure
at New-Switch, : line 121`

Cause: Broken network adapter (perhaps caused by windows update)
1. Close docker desktop
1. Remove the VM `powershell -ExecutionPolicy ByPass -File "C:\Program Files\Docker\Docker\resources\MobyLinux.ps1" -destroy`
2. Remove the broken network adapter https://blogs.msdn.microsoft.com/jjameson/2011/03/14/removing-stale-network-adapters-in-hyper-v-vm/
3. Reset docker desktop to factory defaults
4. Restart docker desktop

`Unable to start: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: 'MobyLinuxVM' failed to start. (Virtual machine ID 317A8D7D-9484-45FC-ABD4-07083AC4454E)`

`'MobyLinuxVM' failed to start worker process: Windows cannot verify the digital signature for this file. A recent hardware or software change might have installed a file that is signed incorrectly or damaged, or that might be malicious software from an unknown source. (0xC0000428). (Virtual machine ID 317A8D7D-9484-45FC-ABD4-07083AC4454E)
at Start-MobyLinuxVM, <No file>: line 300`

?

`Unable to start: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: 'MobyLinuxVM' failed to start. (Virtual machine ID E2B4DEA4-1300-43B4-9D49-BBCC2DEEC5ED)

'MobyLinuxVM' failed to start worker process: Windows cannot verify the digital signature for this file. A recent hardware or software change might have installed a file that is signed incorrectly or damaged, or that might be malicious software from an unknown source. `

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


`no space left on device`
- Typically  you've run out of space because of images, volumes, logs etc. Various ways of looking at whats there and deleting it.
- https://stackoverflow.com/questions/30604846/docker-error-no-space-left-on-device
- https://unix.stackexchange.com/questions/203168/docker-says-no-space-left-on-device-but-system-has-plenty-of-space


`Access Denied: Unable to start Docker, you must be part of the docker-users group`

Cause: Active directory / user account permissions setup

See https://github.com/docker/for-win/issues/868
1. You must restart after install
2. Login to active directory and add yourself into docker-users group. using local account may fail. (https://github.com/docker/for-win/issues/868#issuecomment-352279510)
