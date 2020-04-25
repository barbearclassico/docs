Tools
=====

Simple Machines Forum
---------------------

Barbearclassico website runs on third party software

OVH
---

OVH is a French cloud computing company that offers VPS, dedicated
servers and other web services.

Git Server
----------

For this we’re using gitolite and gitlist.

http://git.barbearclassico.com

Ansible
-------

All the infrastructure is deployed and maintained through ansible playbooks


PasswordStore
-------------

A simple and secure password manager using gpg tools

nci-ansible-ui
--------------

A node tool designed for continuous integration.

Databases
---------

For barbearclassico website we count with 3 databases:

-  production
-  staging
-  tests

Package repository
------------------

Using aptly to handle packages.

References:

.. _Aptly:  https://www.aptly.info/
.. `https://blog.plista.com/aptly-debian-repositories/`

Backups
-------

.. image:: ../images/Dropbox_logo.svg
    :width: 200px
    :align: center
    :height: 100px
    :alt: alternate text


All the data is stored at a dropbox account, using a command line 
interface to perform backup and restore operations.

Production backups are made and pruned on a daily basis.

Data is deleted on a daily basis using the following logic:

+--------------------------------------+----------+
| Action                               | Criteria |
+======================================+==========+
| keep daily backups                   |  7 days  |
+--------------------------------------+----------+
| keep only mondays                    |  1 month |
+--------------------------------------+----------+
| keep first monday of the month       |  1 year  |
+--------------------------------------+----------+
| keep the first monday of the quarter |  5 years |
+--------------------------------------+----------+

Workflow
========

Barbearclassico runs on a third party software

