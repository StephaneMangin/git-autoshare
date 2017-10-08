git-autoshare
=============

.. image:: https://img.shields.io/badge/license-GPL--3-blue.svg
   :target: http://www.gnu.org/licenses/gpl-3.0-standalone.html
   :alt: License: GPL-3
.. image:: https://badge.fury.io/py/git-autoshare.svg
    :target: http://badge.fury.io/py/git-autoshare
.. image:: https://travis-ci.org/acsone/git-autoshare.svg?branch=master
   :target: https://travis-ci.org/acsone/git-autoshare
.. image:: https://codecov.io/gh/acsone/git-autoshare/branch/master/graph/badge.svg
   :target: https://codecov.io/gh/acsone/git-autoshare

``git-autoshare`` is a git wrapper that automatically uses the `--reference 
<https://git-scm.com/docs/git-clone#git-clone---reference-if-ableltrepositorygt>`_
option of ``git clone`` to save disk space and download time.

Installation
~~~~~~~~~~~~

To install git-autoshare in a fancy way, we recommend using `pipsi <https://github.com/mitsuhiko/pipsi>`_.

Pipsi is a powerful tool which allows you to install Python scripts into isolated virtual environments.

To install pipsi, first run this::

    $ curl https://raw.githubusercontent.com/mitsuhiko/pipsi/master/get-pipsi.py | python

Follow the instructions, you'll have to update your ``PATH`` and make sure the script directory
comes before your regular ``git`` in your ``PATH``.

Then simply run::

    $ pipsi install git-autoshare

To upgrade git-autoshare at any time::

    $ pipsi upgrade git-autoshare

``git-autoshare`` installs itself both as a ``git`` executable, and a ``git-autoshare`` executable.

Usage
~~~~~

By default, the git-autoshare provided ``git`` executable should be completely transparent and 
behave exactly like your regular git.

To configure it, create a file named ``git-autoshare/repos.yml`` in your user configuration 
directory (often ``~/.config`` on Linux). This file must have the following structre::

    host:
        repo:
            - organization
            - ...
        ...:
    ...:

It lists all git hosts, repositories, and organizations that are subject to the sharing
of git objects. Here is an example::

    github.com:
        odoo:
            - odoo
            - OCA
        mis-builder:
            - OCA
            - acsone

git clone
---------

When configuring like this, when you git clone the odoo or mis-builder repositories, 
from one of these github organizations, git-autoshare will automatically insert the
``--reference`` option in the git command.

For instance::

    $ git clone https://github.com/odoo/odoo

will be transformed into::

    $ /usr/bin/git clone --reference ~/.cache/git-autoshare/github.com/odoo https://github.com/odoo/odoo


git prefetch
------------

git-autoshare adds a prefetch command that is mostly meant to be run in a cron job::

    $ git prefetch

will update the cache directory by fetching all repositories mentioned in repos.yml.

It can also prefetch one single repository, for example::

    $ git prefetch https://github.com/odoo/odoo.git

Configuration
~~~~~~~~~~~~~

The cache directory is named `git-autoshare` where `appdirs <https://pypi.python.org/pypi/appdirs>`_.user_cache_dir is.
This location can be configured with the `GIT_AUTOSHARE_CACHE_DIR` environment variable.

The default configuration file is named `repos.yml` where `appdirs <https://pypi.python.org/pypi/appdirs>`_.user_config_dir is.
This location can be configured with the `GIT_AUTOSHARE_CONFIG_DIR` environment variable.

Credits
~~~~~~~

Author:

  * Stéphane Bidoul (`ACSONE <http://acsone.eu/>`_)
