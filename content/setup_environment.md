+++
title = 'Setup'
+++
### Instructions to set up your personal computer
- *You will need to bring your own PC*

We will be using the [ARDC Nectar Research Cloud](https://ardc.edu.au/services/ardc-nectar-research-cloud/) to perform RNAseq analyses

![](nectar_logo.png)

### Requirements
- terminal applciation installed
- or alternativelly Visual Studio Code installed\
*please see instructions below if don't have either of these*

### Accessing nectar using a ssh (Secure Shell) command
To access `Nectar` you will need the following 

|   |            |                    |
|---|------------|--------------------|
| 1 | username   | **workshop**         |
| 2 | password   | **Sagc_2024**       |
| 3 | IP address | ***given on the day*** |

Your individual IP address will be given on the day. Each participant will have their own VM (virtual machine). 
**Do not log in with the same IP address as someone else!***


your ssh command should look like this, but with a different IP address
```bash
ssh workshop@172.243.264.45
```
press enter, and you should see be prompted with some text
```
The authenticity of host '118.138.235.141 (118.138.235.141)' can't be established.
ECDSA key fingerprint is SHA256:dgHPXk3BkFPTErqpjAYkB5qAnHLYeUZE7qg3YKcWVQw.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```
- type in 'yes'
- enter your password if prompted

you should see your username in green text, e.g.
```
workshop@workshop
```
This means you have logged into the virtual machine.

you can now navigate to the workshop materials, which have been loaded to your instance.

```bash
cd ~/workshop/
ls -l
```

## Install and set up a terminal application (if needed)
#### Linux terminals
If you use Linux, then likely no explanation is needed. Open your preferred terminal program

#### OS X (Mac)
Mac operating systems have a terminal program, called Terminal. Just look for it in your Applications folder, or hit Command + Space and type ‘terminal’. You may find that other, 3rd party terminal programs are .

Use `ssh` in the *Terminal* app. For example, if your IP is `172.243.264.45` and user name is `user`, you would enter:

```
ssh user@172.243.264.45
```
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



# Environment Setup
	**NO NEED TO CONTINUE** if accessing this content on the nectar instance


On the day of the workshop this will have already been done, and will be available when you log into the Nectar instance using *ssh* \
however if you wish to use your own computing environment or from scratch you will have to install the dependencies
- install nextflow
- install conda
- install nf-core tools
- install singularity
- install GO

you can fistly check whether these have been installed with the following commands
```bash 
nextflow -version
conda -version
nf-core -version
singularity -version
```

#### install nextflow
```bash
mkdir ~/bin && cd ~/bin
curl -s https://get.nextflow.io | bash
chmod +x nextflow
```

adding ~/bin to PATH # in ~/.bashrc
```bash
echo 'export PATH=~/bin:${PATH}' >> ~/.bashrc && \
    source ~/.bashrc
```

test installation
```bash
nextflow -version
```
#### Setting the nextflow singularity cachedir

```bash
mkdir -p ~/SingularityCacheDir

#export NXF_SINGULARITY_CACHEDIR=~/SingularityCacheDir
echo 'export NXF_SINGULARITY_CACHEDIR=~/SingularityCacheDir' >> ~/.bashrc && \
    source ~/.bashrc

# check cache dir
echo $NXF_SINGULARITY_CACHEDIR
```

#### install Singularity

- install the following dependencies: (Ubuntu)
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
singularity  --version
```

install Conda
```bash
cd ~/bin && mkdir -p miniconda3
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
# chosing to install nf-core to a new conda environment called 'nf-core', with nexflow as well
conda create --name nf-core python=3.12 nf-core

# initiate conda 
conda init

# if prompted close terminal and reopen
exit
# reopen with ssh command

# see whether the environment is there
conda env list

#activate this environment
conda activate nf-core

# test installation
nextflow -v
nf-core -v
```
# Samtools
```bash
cd ~/bin
wget https://github.com/samtools/samtools/releases/download/1.21/samtools-1.21.tar.bz2
tar -xvjf samtools-1.21.tar.bz2
rm samtools-1.21.tar.bz2
cd samtools-1.21/
make
make install
```

[link to RNAseq Background slides](../Workshop_RNAseq_Intro.pdf)
