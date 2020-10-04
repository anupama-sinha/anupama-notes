
## Docker Desktop Installations in Windows 10 Home Edition 64 bit OS

### Enabling BIOS Virtualization
* Enter into BIOS during system boot and enable
> cmd : systeminfo(Virtualization Enabled In Firmware: No)

### Windows Hypervisor
> Control Panel > Turn off windows features on or off > Windows Hypervisor Platform	

### Docker Desktop
* https://docs.docker.com/docker-for-windows/install-windows-home/
				   
### WSL2 Linux Kernel Update
* https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel

### Docker ToolBox Installations : This must install Docker Quickstart,Oracle VM Virtual Box,Kitematic (Alpha)
* https://docs.docker.com/toolbox/toolbox_install_windows/
* https://github.com/docker/toolbox/releases

### Virtual Box
* https://www.virtualbox.org/wiki/Downloads

### Hyper-V Configuration : Replace line 69 of DockerToolBox/start.sh
"${DOCKER_MACHINE}" create -d virtualbox $PROXY_ENV "${VM}"
"${DOCKER_MACHINE}" create -d virtualbox --virtualbox-no-vtx-check $PROXY_ENV "${VM}"

### Issue of Docker Pull Timeout
* https://stackoverflow.com/a/48049181/14179048
* https://www.privateinternetaccess.com/blog/changing-your-dns-settings-on-windows-10/

Then start Quickstart Terminal and start the game. :)

### References
* https://www.youtube.com/watch?v=YH3sutAsxEM
* https://medium.com/@mbyfieldcameron/docker-on-windows-10-home-edition-c186c538dff3
* https://docs.docker.com/docker-for-windows/troubleshoot/#virtualization
* https://thewebspark.com/2019/04/02/how-to-enable-virtualization-in-bios-of-windows-10-home-hp-systems-solved/
* https://superuser.com/a/1512595