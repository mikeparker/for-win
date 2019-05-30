`"Binary stream "0" does not contain valid BinaryHeader. This may be due to invalid flow, or due to changes in the object version between serialization and serialization.`

See: https://github.com/docker/for-win/issues/1232
* VirtualBox is not compatible with Hyper-V, and Docker Desktop needs Hyper-V to run linux containers. Turn off VirtualBox.
* Windows update may have broken a component. Users have had success reinstalling file/printer sharing in windows: https://github.com/docker/for-win/issues/1232#issuecomment-338588366


`Unable to create: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: Hyper-V encountered an error trying to access an object on computer 'EMERIO-USER' because the object was not found. The object might have been deleted. Verify that the Virtual Machine Management service on the computer is running.
at New-Switch, : line 121`

See https://github.com/docker/for-win/issues/1221
* Windows updates may have broken some Hyper-V components. See https://github.com/docker/for-win/issues/1221#issuecomment-493118403 for the 'control flow guard' fix

`Unable to create: The running command stopped because the preference variable "ErrorActionPreference" or common parameter is set to Stop: Generic failure
at New-Switch, : line 121`

?

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
