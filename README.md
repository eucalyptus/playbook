Eucalyptus Playbook
========

This is a playbook for deploying Eucalyptus via packages in accordance with reference Architectures. Depends on ansible 1.3 or above. There are a number of inventory files which aim to mimic the topology of Eucalyptus per the reference architectures found here: http://www.eucalyptus.com/eucalyptus-cloud/reference-architectures/dev-test-large  (minus SAN).

## Configuration

Note that variables for the installation should be configured in `vars/main.yml`.  Here you can change the software version, network settings etc.

Some variable definitions are deprecated so you might want to set this parameter in /etc/ansible/ansible.cfg to remove warnings:
 
`deprecation_warnings=False`

... and due to this legacy syntax this playbook does not work with 1.6-devel (i.e. from checkout). You will definately need this too, for now:

`error_on_undefined_vars = False`

You might also want to consider (if you rebuild a lot) disabling host key checking by enabling the line:

`host_key_checking = False`

## Getting Started

Edit the inventory file template you wish to use.  You'll need to put your host systems into here:

```bash
[clc]
myclc.mydomain.com

[walrus]
mywalrus.mydomain.com

[sc]
mysc.mydomain.com

[cc]
mycc.mydomain.com

[nc]
mynode.mydomain.com
```

Test by making sure you can reach your hosts, using the ping module.

```bash
# ansible clc:walrus:sc:cc:nc -m ping
# ansible cc:nc -m ping
192.168.249.65 | success >> {
    "changed": false, 
    "ping": "pong"
}

192.168.249.66 | success >> {
    "changed": false, 
    "ping": "pong"
}

192.168.249.18 | success >> {
    "changed": false, 
    "ping": "pong"
}

192.168.249.67 | success >> {
    "changed": false, 
    "ping": "pong"
}

192.168.249.64 | success >> {
    "changed": false, 
    "ping": "pong"
}

192.168.249.69 | success >> {
    "changed": false, 
    "ping": "pong"
}

192.168.249.70 | success >> {
    "changed": false, 
    "ping": "pong"
}

192.168.249.71 | success >> {
    "changed": false, 
    "ping": "pong"
}

192.168.249.68 | success >> {
    "changed": false, 
    "ping": "pong"
}
```

Now edit vars/main.yml and change your desired variables. You'll definately want to check the version and network parameter variables.  Of particular interest, outside of Eucalyptus' configuration is the bridging section.  The playbooks have only been tested in MANAGED-NOVLAN and, to a lesser extent, MANAGED modes.  As such, you need a bridged interface on your Node Controllers.  This section controls that:

```bash
bridge: br0
bridge_iface: em2
```

`bridge` is the name of your network bridge that the playbook will create.  `bridge_iface` is the interface which will be bridged to this.  The `bridge_iface` interface needs to be on the same network as your CC.  Please consult the Eucalyptus docuemtnation for more information: www.eucalyptus.com/docs.

Next, you can go ahead and run your playbook: 
```bash
ansible-playbook -i dev-test-pilot-hosts site.yml
```

Use the `-u` option to specify the username, `-k` if you want to pass a common SSH password (consider using key based auth) and `-s` for sudo and `-S` for passing the sudo password.




