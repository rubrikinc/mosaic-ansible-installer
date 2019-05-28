# Rubrik Datos IO Ansible Deployment

Rubrik Datos IO Ansible scripts for deploying RecoverX.

Features the following:

* Configuration of new, customer provided, Datos IO nodes
* Installation of the Datos IO software.

# :blue_book: Documentation 

Here are some resources to get you started! If you find any challenges from this project are not properly documented or are unclear, please [raise an issue](https://github.com/rubrikinc/rdio-ansible-installer/issues/new/choose) and let us know! This is a fun, safe environment - don't worry if you're a GitHub newbie! :heart:

* [Quick Start Guide](docs/quick-start.md)

# :white_check_mark: Prerequisites

* Ansible 2.7.9
* Local user with sudo access for Playbook execution
* Python 2.7+ or 3.6+
* sshpass (`yum install sshpass` or `apt-get install sshpass`)
* Access to the OS package repository to install required packages.
  
* Requires the following variables data to be defined for any nodes using the this module:

```text
   # RDIO Node settings

    rdio_user: rdio_user
    rdio_user_pass: Rubrik123!
    rdio_user_uid: "2018"
    rdio_user_home: "/home/{{ rdio_user }}"      # must be on a non-root volume
    rdio_installer_directory: "{{ rdio_user_home }}"
    rdio_install_file: datos-3.0.1-p3-190329.tar.gz
    rdio_nfs: False
    rdio_nfs_mount: /mnt/datos_target
    rdio_nfs_export: /exports/datos_data
    rdio_nfs_target: fs1.dom.local
    mongodb: False
    cassdb_minimum_space: 268435456000          #Value in bytes
    mongodb_minimum_space: 1099511627776        #Value in bytes

    #RDIO Data Source settings

    rdio_app_user: rdio_app_user
    rdio_app_user_pass: Rubrik123!
    rdio_app_user_home: /home/{{ rdio_app_user }}
```

# :muscle: How You Can Help

We glady welcome contributions from the community. From updating the documentation to adding more functions for this module, all ideas are welcome. Thank you in advance for all of your issues, pull requests, and comments! :star:

* [Contributing Guide](CONTRIBUTING.md)
* [Code of Conduct](CODE_OF_CONDUCT.md)

# :pushpin: License

* [MIT License](LICENSE)

# :point_right: About Rubrik Build

We encourage all contributors to become members. We aim to grow an active, healthy community of contributors, reviewers, and code owners. Learn more in our [Welcome to the Rubrik Build Community](https://github.com/rubrikinc/welcome-to-rubrik-build) page.

We'd love to hear from you! Email us: build@rubrik.com :love_letter:
