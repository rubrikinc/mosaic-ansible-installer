# ansible-rdio-recoverx

Ansible Playbook to deploy Rubrik DatosIO RecoverX 3.0 in a single or multi-node deployment on CentOS 7.x x64 or Ubuntu 14 or 16 x64.
This Playbook deploys a standard configuration without any Stores/Sources/Schedules/Policies.

## Playbook Requirements

### Datos IO Nodes

* Datos IO Nodes: CentOS 7.x, Ubuntu 14.04 or 16.04
* Python 2.7+ or 3.6+
* Access to the OS package repository to install required packages.
* Root user password or ssh keys.

### Ansible Host

* Ansible 2.7.9
* Local user with sudo access for Playbook execution
* Python 2.7+ or 3.6+
* sshpass (`yum install sshpass` or `apt-get install sshpass`)
* Access to the OS package repository to install required packages.

## Configuration

1. Deploy 1, 3 or 5 CentOS or Ubuntu nodes to act as the Datos IO nodes.
2. Verify that the DataIO nodes have a non-root filesystem mounted with 300+GB of space. This will be used for the Datos IO user home directory and installation files.
3. Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) on the host that will run the installation scripts.
    * This should not be the Datos IO nodes.
4. Download the [Ansible RDIO Deployment Script](https://github.com/rubrikinc/rdio-ansible-deploy) scripts from the RubrikInc organization on GitHub to the host that will run Ansible.
5. Download the Rubrik Datos IO tarball from [Rubrik Support](https://support.rubrik.com)
6. Place desired DatosIO tarball into the `files/` directory of the [Ansible RDIO Deployment Script](https://github.com/rubrikinc/rdio-ansible-deploy) the prior to Playbook execution.
7. Verify that the extract directory for the [Ansible RDIO Deployment Script](https://github.com/rubrikinc/rdio-ansible-deploy) is not world writable.
     * Run `chmod 755 ./rdio-ansible-deploy`
     * If this cannot be avoided run `export ANSIBLE_CONFIG=./ansible.cfg`
8. Edit the role variables in the `defaults/vars.yml` file.

    ```text
    # RDIO Node settings

    rdio_user: rdio_user
    rdio_user_pass: Rubrik123!
    rdio_user_uid: "2018"
    rdio_user_home: /home/rdio_user       # must be on a non-root volume
    rdio_install_file: datos-3.0.1-p3-190329.tar.gz
    rdio_nfs: False
    rdio_nfs_mount: /mnt/datos_target
    rdio_nfs_export: /exports/datos_data
    rdio_nfs_target: fs1.dom.local
    mongodb: False

    #RDIO Data Source settings

    rdio_app_user: rdio_app_user
    rdio_app_user_pass: Rubrik123!
    rdio_app_user_home: /home/rdio_app_user
    ```

    * `rdio_user` - User to create that will run Datos IO on the Datos IO nodes
    * `rdio_user_pass` - Password of the Datos IO user
    * `rdio_user_home` - Home directory of the Datos IO user. This must be on a non-root volume.
    * `rdio_install_file` - Name of the Datos IO tarball to deploy. Should be placed in `files/`
    * `rdio_nfs` - Set to 'True' if NFS storage will be used as the data store.
    * `rdio_nfs_mount` - Mount point for the NFS data store on the Datos IO servers (if NFS will be used).
    * `rdio_nfs_export` - Export on the NFS server (if NFS will be used).
    * `rdio_nfs_target` - Hostname or IP address of the NFS server (if NFS will be used).
    * `mongodb` - Set to 'True' if using MongoDB as a data source, as this will enable the required fuse configuration changes.

    * `rdio_app_user` - User to create on the data source that will run the Datos IO agent.
    * `rdio_app_user_pass` - Password of the Datos IO Application User
    * `rdio_app_user_home` - Home directory of the Datos IO Application User.
  
9. Edit the `defaults/hosts` inventory file:

    ```text
    [all:vars]
    ansible_connection=ssh
    ansible_user=root

    [rx]
    rdio-1 ansible_host=192.168.1.10
    rdio-2 ansible_host=192.168.1.11
    rdio-3 ansible_host=192.168.1.12

    [cassdb]
    cassdb-1 ansible_host=192.168.1.20
    cassdb-2 ansible_host=192.168.1.21
    cassdb-3 ansible_host=192.168.1.22

    [mongodb]
    mongodb-1 ansible_host=192.168.1.30
    mongodb-2 ansible_host=192.168.1.31
    mongodb-3 ansible_host=192.168.1.32
    ```

   * All Datos IO nodes should be part of the `rx` group
   * All Cassandra database nodes should be part of the `cassdb` group
   * All MongoDB nodes should be part of the `mongodb` group
   * List each host with the format `<hostname> ansible_host=<ip_address>`

10. Run the playbook to install Datos IO based on your configuration:
    * With out ssh keys installed for root on each node:

    ```bash
    export ANSIBLE_CONFIG=./ansible.cfg
    ansible-playbook -i defaults/hosts install-rx.yml --ask-pass
    ```

    * With ssh keys installed for root on each node:

    ```bash
    ssh-agent
    ssh-add ~/.ssh/id_rsa
    export ANSIBLE_CONFIG=./ansible.cfg
    ansible-playbook -i defaults/hosts install-rx.yml
    ```

## License

* [MIT License](https://github.com/rubrikinc/rdio-ansible-deploy/blob/master/LICENSE)