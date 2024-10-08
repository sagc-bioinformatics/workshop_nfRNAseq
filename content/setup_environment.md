+++
title = 'Setup'
+++

### Instructions to set up your personal computer
For this workshop, we will be using the [ARDC Nectar Research Cloud](https://ardc.edu.au/services/ardc-nectar-research-cloud/) to perform RNAseq analyses

Instuctions on how to access the nectar instance will be given on the day
- an ssh command
- a username
- a password 
```
# your ssh command should look like this
ssh username@172.243.264.45
```

### Requirements
- a pc with the terminal applciation installed
- or alternativelly Visual Studio Code installed


## Install and set up a terminal application

#### Linux terminals
If you use Linux, then likely no explanation is needed. Open your preferred terminal program

#### OS X (Mac)
Mac operating systems have a terminal program, called Terminal. Just look for it in your Applications folder, or hit Command + Space and type ‘terminal’. You may find that other, 3rd party terminal programs are .

Use `ssh` in the *Terminal* app. For example, if your IP is `172.243.264.45` and user name is `user`, you would enter:

```
ssh user@172.243.264.45
```

When prompted, type your password and then press `return`.

### Windows

The latest builds of Windows 10 and 11 come with SSH server and client. To see if your windows installation has SSH:
- start Command Prompt (cmd) or Powershell
- type in 'ssh'

If SSH is available, you should see something like 

```
usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface]
           [-b bind_address] [-c cipher_spec] [-D [bind_address:]port]
           [-E log_file] [-e escape_char] [-F configfile] [-I pkcs11]
           [-i identity_file] [-J [user@]host[:port]] [-L address]
           [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port]
           [-Q query_option] [-R address] [-S ctl_path] [-W host:port]
           [-w local_tun[:remote_tun]] destination [command]
```

If SSH is not available, please install [PuTTY](https://www.putty.org/).

Open PuTTY and connect using the IP address of your VM as the host name, and set port to 22.

When prompted, type in your username and password.

# Login via Terminal
To log in to Nectar, you need to use a Secure Shell (SSH) connection. To connect, you need 3 things

- The assigned IP address of your instance (i.e. 172.243.264.45). These will be circulated at the beginning of the workshop
- Your login name. This will be 'SAGC' for all participants.
- Your password. All participants will be provided with a password at the beginning of the workshop.
To log in: type the following into your terminal, using your allocated instance’s IP address:

```bash
ssh username@172.243.264.45
```
You will receive a message saying:

```
The authenticity of host '172.243.264.45 (172.243.264.45)' can't be established.

Remember your host address will be different than the one above. There will then be a message saying:

Are you sure you want to continue connecting (yes/no)?
```
If you would like to skip this message next time you log in, answer ‘yes’. It will then give a warning:

```
Warning: Permanently added '172.243.264.45' (ECDSA) to the list of known hosts.

```
Enter the password provided at the beginning of the workshop.


# log into nectar instance
using the username and IP address provided
- example
```
IP='172.243.264.45'
user='workshop'
sshkey='/Users/danielthomson/.ssh/dan_nectar'
```
using your information run the following command in your terminal
```bash
ssh -i $sshkey $user@$IP
#ssh -i /Users/danielthomson/.ssh/dan_nectar ubuntu@118.138.235.112
# or using other ssh command
```
navigate to worshop materials
```bash
cd ~/workshop/
ls -l
```




# Environment Setup
	**NO NEED TO CONTINUE** if accessing this content on the nectar instance


On the day of the workshop this will have already been done, and will be available when you log into the Nectar instance using *ssh* \
however if you wish to use your own computing environment or from scratch you will have to install the dependencies
- install nextflow
- install conda
- install nf-core tools
- install singularity
- install GO


#### install nextflow
```bash
mkdir ~/bin
cd ~/bin
curl -s https://get.nextflow.io | bash
chmod +x nextflow
```

#### adding ~/bin to PATH # in ~/.bashrc
```bash
echo 'export PATH=~/bin:${PATH}' >> ~/.bashrc && \
    source ~/.bashrc
```
#### Nexctflow options and commands

```bash
# test installation
nextflow -h
# run help menu
nextflow run -help
# check version
nextflow -version
```
#### Managing environment
- setting the nextflow singularity cachedir

```bash
mkdir -p ~/SingularityCacheDir

#export NXF_SINGULARITY_CACHEDIR=~/SingularityCacheDir
echo 'export NXF_SINGULARITY_CACHEDIR=~/SingularityCacheDir' >> ~/.bashrc && \
    source ~/.bashrc

# check cache dir
echo $NXF_SINGULARITY_CACHEDIR
```
- can add this line to ~/.bashrc

#### install Singularity

-On Ubuntu or Debian install the following dependencies: 
```bash
sudo apt-get update && sudo apt-get install -y \
    build-essential \
    uuid-dev \
    libgpgme-dev \
    squashfs-tools \
    libseccomp-dev \
    wget \
    pkg-config \
    git \
    cryptsetup-bin
```
if you are using an HPC environment without superuser (su) access, then you will have to ask IT/administrator to install it
Sahmri HPC, Nectar or Deepthought will already have it installed


- install GO
- GO is a dependency of Singularity
```bash
export VERSION=1.23.2 OS=linux ARCH=amd64 && \
    wget https://dl.google.com/go/go$VERSION.$OS-$ARCH.tar.gz && \
    sudo tar -C /usr/local -xzvf go$VERSION.$OS-$ARCH.tar.gz && \
    rm go$VERSION.$OS-$ARCH.tar.gz

# set up your environment for Go.

echo 'export GOPATH=${HOME}/go' >> ~/.bashrc && \
    echo 'export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin' >> ~/.bashrc && \
    source ~/.bashrc
```

Install Singularity
```bash
# download singularity
export VERSION=3.9.6 && # adjust this as necessary \
    wget https://github.com/sylabs/singularity/releases/download/v${VERSION}/singularity-ce-${VERSION}.tar.gz && \
    tar -xzf singularity-ce-${VERSION}.tar.gz && \
    cd singularity-ce-${VERSION}

#compile Singularity
./mconfig && \
    make -C ./builddir && \
    sudo make -C ./builddir install

# test installation
singularity -h
```

#### install Conda
```bash
cd ~/bin
mkdir -p miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda3/miniconda.sh
bash miniconda3/miniconda.sh -b -u -p miniconda3
rm miniconda3/miniconda.sh

#### add PATH to ~/.bashrc
echo 'export PATH=${PATH}:~/bin/miniconda3/bin' >> ~/.bashrc && \
    source ~/.bashrc

```

add bioconda to conda channel
```bash
conda config --add channels bioconda

```
#### install nf-core tools

```bash
conda install nf-core

# test installation
nextflow run hello
nf-core list
```
[link to introduction slides](../static/Workshop_RNAseq_Intro.pdf)
