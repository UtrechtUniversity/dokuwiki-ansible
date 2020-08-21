# dokuwiki-ansible
Ansible-module for Dokuwiki

This is an Ansible playbook for configuring Dokuwiki on a CentOS 7 server.

It is partially based on the [EPOS-MSL playbook](https://github.com/UtrechtUniversity/epos-msl).

== Testing locally

The repository contains a Vagrantfile to deploy a local VM running Dokuwiki. It requires Vagrant 2.x,
as well as a VirtualBox version that is compatible with your Vagrant version. Deploy the VM with
Vagrant: _vagrant up_

== Adding accounts and changing admin credentials

When deploying the Dokuwiki server, you'll want to change the default admin credentials (admin/admin)

In order to do this, define the dokuwiki\_users variable. This file supports bcrypt hashed passwords. You can
calculate bcrypt hashes using Python 3. For example:

'''
pip3 install bcrypt
python3 -c 'import bcrypt; print(bcrypt.hashpw(b"mypassword", bcrypt.gensalt(rounds=15)).decode("ascii"))' | sed 's/\$2b/\$2a/'
'''

The dokuwiki\_users variable should contain a list of user names, passwords, and groups, like so:

'''
dokuwiki_users:
  admin:
    password: $2a$15$tavJPYxA3TkWow9biRifBuOzqLEOsSxSpswgejPATuYRawcRTluZO
    fullname: Admin User
    email: example@example.org
    groups: users
'''
