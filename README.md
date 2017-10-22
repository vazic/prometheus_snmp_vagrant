# Prometheus SNMP Vagrant environment
Example Vagrant environment for Prometheus SNMP monitoring of network devices

This Vagrant environment contains 2 boxes:

1. Prometheus + SNMP_Exporter running on CentOS7
2. Arista vEOS switch


# How to start environment
1. Ensure you have latest [Vagrant](https://www.vagrantup.com/) installed on your host computer
2. Pull this repo with dependencies `git clone --recursive https://github.com/vazic/prometheus_snmp_vagrant.git`
3. Download Arista vEOS image - [see instructions](https://eos.arista.com/veos-running-eos-in-a-vm/#Download_vEOS)
4. Add vEOS image as vagrant box like so:

`vagrant box add --name vEOS-lab-4.17.5M ~/Downloads/vEOS-lab-4.17.5M-virtualbox.box`

(_version and name matters. if you want to use different - update Vagrantfile_)

5. Install Ansible >= 2.3.2 - [see instructions](http://docs.ansible.com/ansible/latest/intro_installation.html)
6. Start environment by running `vagrant up`

# Checking Environment status

If environment boot was completed without any errors, you should be example to access Prometheus GUI on port 9090 of your host machine. Try to open `http://localhost:9090/graph` in your browser.

If successful, you can list all SNMP metrics by running simple query: `{job="snmp"}`

You can access vEOS box with user/password: `admin/admin`
