#### **CentOS 7** Host Server Setup

* This alpha build uses `islandora-docker.com` as the test domain along with the Docker Compose service names e.g. `mysql, fedora, apache` etc.
     * If your institution's server uses real DNS, then skip the next step. If not, please proceed with the following:
     * To ensure this domain resolves properly, one will need to edit the servers and/or their own local `/etc/hosts` file.
         * Open up a terminal and enter: `sudo vi /etc/hosts`
              * Add one or more of the following: (**Examples ONLY**)

              * Ubuntu 16.04 LTS Host Server (or VM)  
              ```
             127.0.0.1       localhost islandora-docker.com fedora apache fedora.islandora-docker.com apache.islandora-docker.com
             ```

             * Enduser local laptop
             ```
             127.0.0.1 localhost   
             192.156.123.26       islandora-docker.com fedora apache fedora.islandora-docker.com apache.islandora-docker.com
             ```

#### Install / run on CentOS 7 server  
* Virtualbox Virtual Machine (VM)  
* a Vagrant image (recommend using [bento/centos-7.2](https://app.vagrantup.com/bento/boxes/centos-7.2))  
* Cloud based server e.g. Digital Ocean or Amazon Web Services  
* Post-installation, add the appropriate ssh keys to the root user's `.ssh/authorized_keys` file.  

#### Docker setup for CentOS 7

* Open a terminal, ssh to the CentOS server as root and install the following:  
     * `yum install openssl git htop ntp wget curl`

#### Install Docker on CentOS 7 (as root user)

* `wget -qO- https://get.docker.com/ | sh`

#### Create islandora user (as root)  
* `adduser islandora`
* `passwd islandora`
* `isle2017`
* `echo "islandora ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/islandora`
* Create Docker group `groupadd docker`  
* Add islandora user to docker group `usermod -aG docker islandora`  
* Add islandora user to wheel group `sudo usermod -aG wheel islandora`
* Exit out of the ssh session from the host server as the root user `exit`
* ssh back in as `islandora`
* Enable the Docker service to start on host server boot
    * `sudo systemctl enable docker.service`
* Start the Docker service
    * `sudo systemctl start docker.service`


#### Install Docker-Compose (version 1.17.1 as of 11/16/2017) as islandora-user on CentOS 7
* Open a terminal and ssh back into the CentOS Host Server/VM as the `islandora` user and perform the following:
* Add the RHEL/CENTOS epel-release package repository
    * `sudo yum install epel-release`
* Install Python Pip (package manager for Python Scripting Language)
    * `sudo yum install -y python-pip`
* Upgrade Python to latest version
    * `sudo yum upgrade python*`
* Upgrade Python Pip
    * `sudo pip install --upgrade pip`
* Install Docker compose
    * `sudo pip install docker-compose`


#### Clone ISLE repository
* Please note in some Linux Distributions, one might need to create the `/opt` directory (optional)  
    * One can `ls -lha /` to see if an `/opt` directory exists  
        * If missing, `sudo mkdir /opt`  
        * If not missing, proceed to next step.  
* `cd /opt`
* `sudo git clone https://github.com/Islandora-Collaboration-Group/ISLE.git`
   * This process will take 1 - 3 minutes depending on internet bandwidth
* `sudo chown -Rv islandora:islandora ISLE`
* `cd /opt/ISLE`


#### Return to Quickstart Guide
* Continue with next steps on [1.4. -Testing - Alpha Quickstart Guide](alpha_quickstart.md)
---