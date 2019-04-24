# Rubrik Datos IO Ansible Deployment

Rubrik Datos IO Ansible scripts for deploying RecoverX.

Features the following:

* Configuration of new, customer provided, Datos IO nodes
* Installation of the Datos IO software.

## :exclamation: :construction: Early Access/Work In Progress

* This is the initial public release in progress. Bugs may be present.

# :blue_book: Documentation 

Here are some resources to get you started! If you find any challenges from this project are not properly documented or are unclear, please [raise an issue](https://github.com/rubrikinc/rdio-ansible-installer/issues/new/choose) and let us know! This is a fun, safe environment - don't worry if you're a GitHub newbie! :heart:

* [Quick Start Guide](/docs/quick-start.md)

# :white_check_mark: Prerequisites

* Ansible version X.X.X must be installed.
* 
* Requires the following variables data to be defined for any nodes using the this module:

```text
## RDIO
rdio_user: rdio_user
rdio_user_pass: Rubrik123!
rdio_user_home: /home/{{ rdio_user }}
rdio_install_file: datos_3.0.0_CentOS_6.8_2018-11-16-00-59.tar.gz
rdio_nfs: False
rdio_nfs_mount: /mnt/datos_target
rdio_nfs_export: /data
rdio_nfs_target: fs1.dom.local
mongodb: False
```

* User needs to provide a valid ssh key to log into the Datos IO servers

# :muscle: How You Can Help

We glady welcome contributions from the community. From updating the documentation to adding more functions for this module, all ideas are welcome. Thank you in advance for all of your issues, pull requests, and comments! :star:

* [Contributing Guide](CONTRIBUTING.md)
* [Code of Conduct](CODE_OF_CONDUCT.md)

# :pushpin: License

* [MIT License](LICENSE)

# :point_right: About Rubrik Build

We encourage all contributors to become members. We aim to grow an active, healthy community of contributors, reviewers, and code owners. Learn more in our [Welcome to the Rubrik Build Community](https://github.com/rubrikinc/welcome-to-rubrik-build) page.

We'd love to hear from you! Email us: build@rubrik.com :love_letter:
