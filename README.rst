DebOps for Turnkey
==================

"DebOps for Turnkey", aka tkl-debops, is a tool to manage `Turnkey Linux`_
appliances using `Ansible`_ and the `DebOps`_ tools.
It provides a set of default DebOps/Ansible configs that configure the
Turnkey appliances with their default configurations.
Custom configurations of the Turnkey appliances for a particular site can
easily be generated and controlled by customizing the included inventory files
or adding new inventory files.

Installation
------------

tkl-debops requires Ansible and DebOps to be installed.
If using the `Ansible Turnkey appliance`_ the following commands will need
to be run to install and configure tkl-debops for use::

    $ sudo apt-get install python-pip
    $ sudo pip install debops
    $ debops-update
    $ git clone https://github.com/jstruebel/tkl-debops.git
    $ cd tkl-debops
    $ ./tkl-debops-init

Some of the roles in DebOps require Ansible > 1.9 which is the version included
with Turnkey v14.1. To update to the latest Ansible version run the following
command::

    $ sudo pip install --upgrade --upgrade-strategy only-if-needed ansible

Usage
-----

Since tkl-debops is built on Ansible and DebOps, the normal methods of using
inventory, playbooks, and roles apply. The recommended workflow is to add the
hosts to be managed to appropriate groups in the ansible/inventory/hosts file
based on their Turnkey appliance. Any customized settings should be added to
inventory files under ansible/inventory/host_vars for each host. If many hosts
will share settings then create a new group and make it a child of the Turnkey
appliance group. Settings for the new groups can then be added to
ansible/inventory/group_vars.

The group names for Turnkey appliances are the appliance name preceded by
"tkl_". These groups have been added as children to the appropriate DebOps
groups in ansible/inventory/tkl-debops. The DebOps groups can be freely
mixed with the tkl groups as desired.

Once customized settings have been created, the hosts can be configures by
running the ``debops`` command from the project directory. Customized
.debops.cfg and ansible/inventory/hosts files may be added to git in a
forked repository. The preferred method is to only add customized files to
a separate branch to leave the master branch clean with upstream.

Anytime a customized project is cloned, the ``tkl-debops-init`` script must
be run from the project directory. The script will update the symlinks in
the ansible/playbooks directory that are necessary for the custom site.yml
file. It will not change any customizations that have been made to the
.debops.cfg or ansible/inventory/hosts file.

By default the included secalerts, tklbam, and hubdns roles are disabled
because they require additional configureation in the inventory before using.
To enable add the appropriate enable variable (secalert__enabled,
tklbam__enabled, or hubdns__enabled) to the inventory. Recommended practice
is to add it to the tkl_core group variables. Before enabling the secalerts
role be sure to also set the tkl__admin_email variable to a valid email
address in the inventory. Before enabling the tklbam or hubdns roles your
Turnkey Hub API key needs to be copied to the 'credentials/tklhub/apikey'
file within the secret directory created by the debops.secret role.
By default the secret directory is called 'secret' and relative to the
project directory.

Further information on the settings for the DebOps roles can be found in the
`DebOps documentation`_.

Development
-----------

Contributions to this project are welcome. Since the list of Turnkey appliances
that have default configurations is incomplete, the primary focus of
development will be to add default configurations for the missing appliances.
To contribute, fork `this repository`_ and issue a pull request on Github.

.. _Turnkey Linux: https://www.turnkeylinux.org/
.. _Ansible: https://www.ansible.com/
.. _Ansible Turnkey appliance: https://www.turnkeylinux.org/ansible
.. _DebOps documentation: https://docs.debops.org/en/latest/
.. _this repository: https://github.com/jstruebel/tkl-debops
