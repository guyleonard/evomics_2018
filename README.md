# Evomics 2018
Instructions and notes for creating the VM for the [evomics.org](http://evomics.org) Workshop on Genomics 2018.

Information on previous VMs can be found [here](https://github.com/guyleonard/evomics_2017).

# 2018 Workshop on Genomics

We will be using the latest Ubuntu Linux LTS for our Operating System.
For example, on AWS this is [ami-cf68e0d8](https://console.aws.amazon.com/ec2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-cf68e0d8) which is the 'us-east-1' copy of Ubuntu Xenial Xerus 16.04 LTS.
However, you should be able to use any Debian based version of Linux, ansible will stop on errors but it can also be resumed from that point.

## Automation

For this we are going to use a series of shell scripts, [Ansible](https://www.ansible.com/) and a few other package managers such as apt-get, pip, conda, and gem.

Not everything can be automated, so you may also wish to refer to the [miscellaneous](https://github.com/guyleonard/evomics_2018/blob/master/misc.md) changes.
Ansible is a simple piece of automation software you can use to install programs and manage systems.
You can see what is happening in each stage by looking in the 'main\_\*.yaml' file (or playbook) for each section (e.g. base, genomics, phylogenomics), it is a plain text file with a list of instructions to install programs, change files, download data etc. Some of these instructions are broken into smaller components called taks (e.g. the Assembly Tutorial from the Workshop on Genomics is a series of programs and files to be installed) and they can be found in the sub-folder "tasks" in each workshop.

# Using This Repository
To build the AMI for either workshop, you will need to run at least the [setup.sh](https://github.com/guyleonard/evomics_2018/blob/master/base.sh) script on your Virtual Machine (this can be on an Amazon VM (AMI) or from another provider or locally), and then both sets of "main\_\*.yaml" playbooks.

## Base AMI Setup

Log in to your virtual machine and run this code:

    wget -O- https://raw.githubusercontent.com/guyleonard/evomics_2018/master/setup.sh | bash

Typically, I would now make a copy of this system, and then use that as your 'base' VM for the next steps (this will save time if you run in to any issues).

If you already have a base VM that you wish to use, then you can do this instead:

    git clone https://github.com/guyleonard/evomics_2018.git
    ansible-playbook /home/ubuntu/evomics_2018/base/main.yaml -b -K -c local -i "localhost,"

## NB

If you are logged in to your VM using a key pair then you do not need to enter a password when you are asked for SUDO access. Just press 'enter' and continue.

## Workshop on Genomics AMI Setup:

### Software

Run the genomics software playbook fully:

    ANSIBLE_NOCOWS=1 ansible-playbook /home/ubuntu/evomics_2017/genomics/main_software.yaml -b -K -c local -i "localhost,"

or for just two tools, e.g. samtools & bwa:

    ANSIBLE_NOCOWS=1 ansible-playbook /home/ubuntu/evomics_2017/genomics/main_software.yaml -b -K -c local -i "localhost," --tags samtools,bwa
    
or for just one tutorial you could do:

    ANSIBLE_NOCOWS=1 ansible-playbook /home/ubuntu/evomics_2017/genomics/main_software.yaml -b -K -c local -i "localhost," --tags assembly

Tags can be mixed and matched as you like, they *should* install their dependencies where I have remembered ;).

### Data

There is only one playbook for the data section, but you can run it with tags as before.

    ANSIBLE_NOCOWS=1 ansible-playbook /home/ubuntu/evomics_2017/genomics/main_data.yaml -b -K -c local -i "localhost,"

# FAQs

* Hey Guy, why does it say "ANSIBLE_NOCOWS=1" above?
```
 _______________________________________
/ Do you really want to see cows in all \
| the output messages? Like this? Why?  |
\ Moooooooo.                            /
 ---------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
