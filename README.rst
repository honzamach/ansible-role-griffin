.. _section-role-griffin:

Role **griffin**
================================================================================

Ansible role for convenient installation of the custom Griffin monitoring tool.

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/griffin>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-griffin>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-griffin>`__


Description
--------------------------------------------------------------------------------

This role attempts to keep the things as simple as possible and performs only
basic installation of the system. The is a monitoring stack based on following
free tools:

* `Telegraf <https://docs.influxdata.com/telegraf/v1.12/>`__
* `PostgreSQL <https://www.postgresql.org/>`__
* `TimescaleDB <https://www.timescale.com/>`__
* `Grafana <https://grafana.com/>`__

.. note::

    This role supports the :ref:`template customization <section-overview-customize-templates>` feature.


Requirements
--------------------------------------------------------------------------------

Python3 with pip3 utility should already be installed on target system.


Dependencies
--------------------------------------------------------------------------------

This role is dependent on following roles:

* :ref:`postgresql <section-role-postgresql>`


Managed files
--------------------------------------------------------------------------------

This role directly manages content of following files on target system:

* ``/etc/telegraf/telegraf.conf``
* ``/etc/grafana/grafana.ini``
* ``/etc/grafana/provisioning/datasources/griffin.yaml``


Role variables
--------------------------------------------------------------------------------

There are following internal role variables defined in ``defaults/main.yml`` file,
that can be overriden and adjusted as needed:

.. envvar:: hm_griffin__user

    Name of the UNIX system user for griffin system.

    * *Datatype:* ``string``
    * *Default:* ``griffin``

.. envvar:: hm_griffin__group

    Name of the UNIX system group for griffin system.

    * *Datatype:* ``string``
    * *Default:* ``griffin``

.. envvar:: hm_griffin__package_repository_url

    Base URL to package repository.

    * *Datatype:* ``string``
    * *Default:* ``https://alchemist.cesnet.cz``

.. envvar:: hm_griffin__suite

    Enforce which package suite to install on target servers no matter the membership
    in groups ``servers-production``, ``servers-demo`` and ``servers-development``.

    * *Datatype:* ``string``
    * *Default:* (undefined)

.. envvar:: hm_griffin__package_list

    List of griffin-related packages, that will be installed on target system.

    * *Datatype:* ``list of strings``
    * *Default:* (please see YAML file ``defaults/main.yml``)

.. envvar:: hm_griffin_do_cleanup

    Do system cleanup (flag).

    * *Datatype:* ``boolean``
    * *Default:* ``false``

.. envvar:: hm_griffin__apt_force_update

    Force APT cache update before installing any packages ('yes','no').

    * *Datatype:* ``string``
    * *Default:* ``no``

.. envvar:: hm_griffin__check_queue_size

    Monitoring configuration setting for checking queue size in the *incoming* directory.

    * *Datatype:* ``dict``
    * *Default:* ``{'w': 5000, 'c': 10000}``

.. envvar:: hm_griffin__check_queue_dirs

    Monitoring configuration setting for checking queue size in other than *incoming*
    directories.

    * *Datatype:* ``dict``
    * *Default:* ``{'w': 100, 'c': 1000}``

.. envvar:: hm_griffin__deprecated_files

    List of deprecated files and folders that may be stil present after previous
    versions of griffin system. These will be removed to keep the system tidy.

    * *Datatype:* ``list of strings``
    * *Default:* (please see YAML file ``defaults/main.yml``)

Additionally this role makes use of following built-in Ansible variables:

.. envvar:: ansible_lsb['codename']

    Debian distribution codename is used for :ref:`template customization <section-overview-customize-templates>`
    feature.

.. envvar:: group_names

    See section *Group memberships* below for details.


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


Example Playbook
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers_griffin]
    localhost

Example content of role playbook file ``playbook.yml``::

    - hosts: servers_griffin
      remote_user: root
      roles:
        - role: honzamach.griffin
      tags:
        - role-griffin

Example usage::

    ansible-playbook -i inventory playbook.yml
    ansible-playbook -i inventory playbook.yml --extra-vars '{"hm_griffin__apt_force_update":"yes"}'


License
--------------------------------------------------------------------------------

MIT


Author Information
--------------------------------------------------------------------------------

Jan Mach <honza.mach.ml@gmail.com>
