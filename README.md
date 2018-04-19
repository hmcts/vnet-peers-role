Role Name
=========

vnet-peers-role is used to create listed peer-pairs from a vars structure:

```
  <FriendlyName>:
    local_vnet: "<Name-of-local-vnet>"
    r_vnet_aid: "<ID-of-Remote-Subscription>"
    r_vnet_rg: "<Name-of-remote-RG>"
    r_vnet: "<Name-of-remote-vnet>"
    resource_group_name: "<Name-of-local-RG>"
    subscription: "<Name-of-subscription>"
```

Note that at least two will be needed, one side, then the inverse.

Requirements
------------

This role requires an az account that has the appropriate permissions 
to enact changes on peers. Internally this is controlled by Jenkins.

Dependencies
------------

Runs locally, so `az` needs to be installed.

Ansible is also required.

About
-----

Primarily, there are two tasks of note `See if the Vnet Peer Exists` which 
checks if the Vnet peer exists already, and registers the output. It 
highlights a peer as 'changed' if it doesn't exist.

There is then a second task `Create nonexistent peers` which specifically 
only creates the peer if it's been highlighted as 'changed' 
(doesn't exist) above.)

An entire first run can take a while with a lot of peers, but subsequent runs
should be fairly quick.

Example Playbook
----------------

Updated your vars/main.yml file with your peerings. They should come in pairs.

Ensure the vars file listed in 'tasks/main.yml' is accurate, internally this
is pulled in from elsewhere.

```
$ ansible-playbook tasks/main.yml
```

License
-------

MIT

Author Information
------------------

adam@terrafoundry.net for HMCTS Reform Programme

TODO
====

* Include a way of testing this using foo vars.
* Include a check which uses dockerized az command.
* Write 'absent' component.
* Rewrite logic into loops, following Ansible deprecation of dict elements.
