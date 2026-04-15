Installing RNPL et al. using docker
==================================

### 0. Install Docker

Following the offical docker installation guide on your particular OS. 

For mac user, XVS and DV do not work on macOS through docker at this point, due to the fact that docker in macOS lack gpu pass through. 
one can install mageia 9 in a VM as an work around.

### 1. Build docker image

This will build the image, calling it `rnpletal`, from the Dockerfile in the current directory.

```bash
docker build -t rnpletal .
```

### 2. Run docker and access via bash:

```bash
docker run --gpus=all -v $(pwd):/PROJECT -it rnpletal bash
```
It assumes that you called your image `rnpletal` and that your project is in the current directory. The `$(pwd)` is the current directory, using `${pwd}` instead of `$(pwd)` in Windows PowerShell. Alternatively, you can use the full path to your project.

To attach to the current image running, you can use `docker ps` to see the running ones and then use this to attach to the running container `ID` (found in the `docker ps`).

```bash
docker exec -it ID bash
```

### Additional commits on specifical system

### 3.1 Digital Research Alliance of Canada (CC) 
docker is not allow to run on CC. Instead, it use apptainer. Installation guide of apptainer can be found [here](https://apptainer.org/docs/user/main/quick_start.html). You can build an Apptainer container from the docker image.
```bash
docker save rnpletal -o rnpletal.tar
apptainer build rnpletal.sif docker-archive://rnpletal.tar
```
You can then upload rnpletal.sif to CC Systems and use it.
```bash
module load apptainer
apptainer shell rnpletal.sif
```
If you want to run XVS or DV, you can start a interactive desktop session and run XVS or DV. In the desktop session terminal
```bash
apptainer shell rnpletal.sif
xvs
```

### 3.2 Windows: convert docker image into WSL (optional)
The offical instructions can be found [here](https://learn.microsoft.com/en-us/windows/wsl/use-custom-distro).

#### 2. Export the container to a tar file:
```bash
docker export rnpletal > rnpletal.tar
```
#### 3. Import the tar file into WSL:
```bash
wsl --import rnpletal <InstallLocation> rnpletal.tar
```
#### 4. Login to WSL and add new user:
```bash
wsl -d rnpletal
```
```bash
myUsername=xili
adduser -G wheel $myUsername
echo -e "[user]\ndefault=$myUsername" >> /etc/wsl.conf
passwd $myUsername
exit
```
change `xili` to your own user name.
###### 5. reboot WSL
```bash
wsl --terminate rnpletal
wsl -d rnpletal
```
###### 6. Clean up container (optional):
```bash
docker rm rnpletal
```
