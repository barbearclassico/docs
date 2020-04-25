# Tools

## Simple Machines Forum 

Barbearclassico website runs on third party software

<img src="../images/smf-logo.png" alt="Dropbox" style="zoom:100%;" />

## OVH

OVH is a French cloud computing company that offers VPS, dedicated servers and other web services.

## Git Server

For this we're using gitolite and gitlist.

http://git.barbearclassico.com

## Databases

For barbearclassico website we count with 3 databases:

- production
- staging
- tests

## Package repository

Using aptly to handle packages.

References:

* https://www.aptly.info/
* https://blog.plista.com/aptly-debian-repositories/

## Backups

<img src="../images/Dropbox_logo.svg" alt="Dropbox" style="zoom:100%;" />

All the data is stored at a dropbox account, using 

Production backups are made and pruned on a daily basis.

Data is deleted on a daily basis using the following logic:

| keep daily backups                   | Rule    |
| ------------------------------------ | ------- |
| keep daily backups                   | 7 days  |
| keep only mondays                    | 1 month |
| keep first monday of the month       | 1 year  |
| keep the first monday of the quarter | 5 years |

Workflow
====================

Barbearclassico runs on a third party software 

<img src="../images/smf-logo.png" alt="Dropbox" style="zoom:100%;" />



Sceptre is written in a way that aims to be unopinionated in how it is used. It
is designed to work equally well with simple and complex infrastructures.
Sceptre’s flexible nature, and the variation in how people organise their AWS
accounts, makes it difficult to give generic advice on how best to use it, as
it can be use-case specific. However, the following patterns have emerged from
our use of Sceptre at Cloudreach.

