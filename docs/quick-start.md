# mosaic-ansible-installer

Ansible role to deploy Rubrik DatosIO RecoverX 3.0 in a single or multi-node deployment on CentOS 7.x x64 or Ubuntu 14 or 16 x64.
This role deploys a standard configuration without any Stores/Sources/Schedules/Policies.

## Role Requirements

### Datos IO Nodes

* Datos IO Nodes: CentOS 7.x, Ubuntu 14.04 or 16.04
* Python 2.7+ or 3.6+
* Access to the OS package repository to install required packages.
* Root user password or ssh keys.

### Ansible Host

* Ansible 2.7.9
* Local user with sudo access for role execution
* Python 2.7+ or 3.6+
* sshpass (`yum install sshpass` or `apt-get install sshpass`)
* Access to the OS package repository to install required packages.

## Configuration

1. Deploy 1, 3 or 5 CentOS, Red Hat Enterprise Linux, Ubuntu, Amazon Linux or Amazon Linux 2 nodes to act as the Datos IO nodes.
2. Verify that the DataIO nodes have a non-root filesystem mounted with 300+GB of space. This will be used for the Datos IO user home directory and installation files.
3. Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) on the host that will execute the Ansible role.
    * This should not be the Datos IO nodes.
4. Download the [Ansible Mosaic Deployment Role](https://github.com/rubrikinc/mosaic-ansible-installer) role from the RubrikInc organization on GitHub to the host that will run Ansible.
5. Download the Rubrik Datos IO tarball from [Rubrik Support](https://support.rubrik.com)
6. Place desired DatosIO tarball into the `files/` directory of the [Ansible Mosaic Deployment Role](https://github.com/rubrikinc/mosaic-ansible-installer) the prior to role execution.
7. Verify that the extract directory for the [Ansible Mosaic Deployment Role](https://github.com/rubrikinc/mosaic-ansible-installer) is not world writable.
     * Run `chmod 755 ./mosaic-ansible-installer`
     * If this cannot be avoided run `export ANSIBLE_CONFIG=./ansible/ansible.cfg`
8. Edit the role variables in the `defaults/vars.yml` file.

    ```text
    # Mosaic Node settings

    mosaic_user: mosaic_user
    mosaic_user_pass: Rubrik123!
    mosaic_user_uid: "2018"
    mosaic_user_home: "/home/{{ mosaic_user }}"      # must be on a non-root volume
    mosaic_installer_directory: "{{ mosaic_user_home }}"
    mosaic_install_file: datos-3.0.1-p3-190329.tar.gz
    mosaic_nfs: False
    mosaic_nfs_mount: /mnt/datos_target
    mosaic_nfs_export: /exports/datos_data
    mosaic_nfs_target: fs1.dom.local
    mongodb: False
    cassdb_minimum_space: 268435456000          #Value in bytes
    mongodb_minimum_space: 1099511627776        #Value in bytes

    # Mosaic Data Source settings

    mosaic_app_user: mosaic_app_user
    mosaic_app_user_pass: Rubrik123!
    mosaic_app_user_home: /home/{{ mosaic_app_user }}
    ```

    | Variable | Description |
    | -------- | ----------- |
    | `mosaic_user` | User to create that will run Datos IO on the Datos IO nodes |
    | `mosaic_user_pass` | Password of the Datos IO user |
    | `mosaic_user_uid` | The UUID of the Datos IO user on the Datos IO nodes. |
    | `mosaic_user_home` | Home directory of the Datos IO user. This must be on a non-root volume. |
    | `mosaic_installer_directory` | Directory where the Datos IO software should be installed on the Datos IO nodes. Default is the Datos IO user's home directory. |
    | `mosaic_install_file` | Name of the Datos IO tarball to deploy. Should be placed in `files/` |
    | `mosaic_nfs` | Set to 'True' if NFS storage will be used as the data store. |
    | `mosaic_nfs_mount` | Mount point for the NFS data store on the Datos IO servers (if NFS will be used). |
    | `mosaic_nfs_export` | Export on the NFS server (if NFS will be used). |
    | `mosaic_nfs_target` | Hostname or IP address of the NFS server (if NFS will be used). |
    | `mongodb` | Set to 'True' if using MongoDB as a data source, as this will enable the required fuse configuration changes. |
    | `cassdb_minimum_space` | The minimum space in bytes required on the Datos IO nodes in the `mosaic_user_home` directory for Cassandra protection. |
    | `mongodb_minimum_space` | The minimum space in bytes required on the Datos IO nodes in the `mosaic_user_home` directory for MongoDB protection. |

    | `mosaic_app_user` | User to create on the data source that will run the Datos IO agent. |
    | `mosaic_app_user_pass` | Password of the Datos IO Application User. |
    | `mosaic_app_user_home` | Home directory of the Datos IO Application User. |
  
9. Edit the `ansible/hosts` inventory file:

    ```text
    [all:vars]
    ansible_connection=ssh
    ansible_user=root

    [rx]
    mosaic-1 ansible_host=192.168.1.10
    mosaic-2 ansible_host=192.168.1.11
    mosaic-3 ansible_host=192.168.1.12

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

10. Run the ansible-playbook command to install Datos IO based on your configuration:
    * With out ssh keys installed for root on each node:

    ```bash
    export ANSIBLE_CONFIG=./ansible/ansible.cfg
    ansible-playbook -l rx -i ansible/hosts install-rx.yml --ask-pass
    ```

    * With ssh keys installed for root on each node:

    ```bash
    ssh-agent
    ssh-add ~/.ssh/id_rsa
    export ANSIBLE_CONFIG=./ansible/ansible.cfg
    ansible-playbook -l rx -i ansible/hosts install-rx.yml
    ```

    * The -l option on `ansible-playbook` limits the instal to just the Datos IO nodes. Database Sources are still being tested.

## License

* [MIT License](../LICENSE)