.. _section-role-griffin:

Role **griffin**
================================================================================

.. note::

    This documentation page and role itself is still work in progress.

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/griffin>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-griffin>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-griffin>`__

Ansible role for convenient installation of the custom Griffin monitoring tool.
This role attempts to keep the things as simple as possible and performs only
basic installation of the system. The is a monitoring stack based on following
free tools:

* `Telegraf <https://docs.influxdata.com/telegraf/v1.12/>`__
* `PostgreSQL <https://www.postgresql.org/>`__
* `TimescaleDB <https://www.timescale.com/>`__
* `Grafana <https://grafana.com/>`__

**Table of Contents:**

* :ref:`section-role-griffin-installation`
* :ref:`section-role-griffin-dependencies`
* :ref:`section-role-griffin-usage`
* :ref:`section-role-griffin-variables`
* :ref:`section-role-griffin-files`
* :ref:`section-role-griffin-author`

This role is part of the `MSMS <https://github.com/honzamach/msms>`__ package.
Some common features are documented in its :ref:`manual <section-manual>`.


.. _section-role-griffin-installation:

Installation
--------------------------------------------------------------------------------

To install the role `honzamach.griffin <https://galaxy.ansible.com/honzamach/griffin>`__
from `Ansible Galaxy <https://galaxy.ansible.com/>`__ please use variation of
following command::

    ansible-galaxy install honzamach.griffin

To install the role directly from `GitHub <https://github.com>`__ by cloning the
`ansible-role-griffin <https://github.com/honzamach/ansible-role-griffin>`__
repository please use variation of following command::

    git clone https://github.com/honzamach/ansible-role-griffin.git honzamach.griffin

Currently the advantage of using direct Git cloning is the ability to easily update
the role when new version comes out.


.. _section-role-griffin-dependencies:

Dependencies
--------------------------------------------------------------------------------

This role is dependent on following roles:

* :ref:`postgresql <section-role-postgresql>`

No other roles have dependency on this role.


.. _section-role-griffin-usage:

Usage
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers_griffin]
    your-server

Example content of role playbook file ``role_playbook.yml``::

    - hosts: servers_griffin
      remote_user: root
      roles:
        - role: honzamach.griffin
      tags:
        - role-griffin

Example usage::

    # Run everything:
    ansible-playbook --ask-vault-pass --inventory inventory role_playbook.yml

    ansible-playbook -i inventory playbook.yml --extra-vars '{"hm_griffin__apt_force_update":"yes"}'


.. _section-role-griffin-variables:

Configuration variables
--------------------------------------------------------------------------------


Internal role variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. envvar:: hm_griffin__install_packages

    List of packages defined separately for each linux distribution and package manager,
    that MUST be present on target system. Any package on this list will be installed on
    target host. This role currently recognizes only ``apt`` for ``debian``.

    * *Datatype:* ``dict``
    * *Default:* (please see YAML file ``defaults/main.yml``)
    * *Example:*

    .. code-block:: yaml

        hm_griffin__install_packages:
          debian:
            apt:
              - syslog-ng
              - ...

.. envvar:: hm_griffin__apt_force_update

    Force APT cache update before installing any packages ('yes','no').

    * *Datatype:* ``string``
    * *Default:* ``no``


Built-in Ansible variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. envvar:: ansible_lsb['codename']

    Debian distribution codename is used for :ref:`template customization <section-overview-role-customize-templates>`
    feature.


.. _section-role-griffin-files:

Managed files
--------------------------------------------------------------------------------

.. note::

    This role supports the :ref:`template customization <section-overview-role-customize-templates>` feature.

This role manages content of following files on target system:

* ``/etc/telegraf/telegraf.conf`` *[TEMPLATE]*
* ``/etc/grafana/grafana.ini`` *[TEMPLATE]*
* ``/etc/grafana/provisioning/datasources/griffin.yaml`` *[TEMPLATE]*


.. _section-role-griffin-author:

Author and license
--------------------------------------------------------------------------------

| *Copyright:* (C) since 2019 Honza Mach <honza.mach.ml@gmail.com>
| *Author:* Honza Mach <honza.mach.ml@gmail.com>
| Use of this role is governed by the MIT license, see LICENSE file.
|
