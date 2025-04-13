# WireGuard

Ansible role that installs `wireguard`.

## Requirements

There are no requirements for this, it should work out of the box with Ansible.

## Role Variables

Set `install_wireguard` to `true` and it will be installed.

Then, you need to add at least one entry to `wireguard_configs`, like this:

```yaml
wireguard_configs:
  - name: wg0
    path: "/etc/wireguard/wg0.conf"
    enabled: true
    state: started
    become: true
    config: |
      [Interface]
      PrivateKey = +PZHixMi/1t9G7C7G98CY17q+sbNFdtojQdvOko6q3E=
      Address = 192.168.5.2/32
      MTU = 1280

      [Peer]
      PublicKey = 0PKP3VoNxU4OovAo+lG4vy/LnV60JP4ektQsFw4SNjA=
      PresharedKey = o7RNGfXlgtij0FpSCx5Y6Rvt9TVCBIeBlZoIHTKNr60=
      Endpoint = 1.2.3.4:51820
      AllowedIPs = 192.168.5.0/24
```

You can get creative with Ansible and environment variables by tweaking the
`enabled` and `state` flags:

```yaml
    enabled: "{{ lookup('ansible.builtin.env', 'WG0_ENABLED', default='true') }}"
    state: "{{ lookup('ansible.builtin.env', 'WG0_STATE', default='started') }}"
```

## Dependencies

None at the moment.

## Usage

First, create a `requirements.yml` in your Ansible setup if you haven't already,
here's an example:

```yaml
---
collections:
  - name: community.general
  - name: ansible.posix
  - name: community.crypto

roles:
  - name: charles-m-knox.wireguard
    src: https://github.com/charles-m-knox/ansible-role-wireguard.git
    scm: git
    version: main
```

Next, install it:

```bash
# include the -f flag to forceably re-install and get the latest (equivalent to
# upgrading)
ansible-galaxy role install -f -r requirements.yml
```

Now, in your `site.yml` (or whatever your playbook is named), use the role -
note that root access is required for some of the steps:

```yaml
- name: wireguard
  hosts: all
  roles:
    - charles-m-knox.wireguard
  tags:
    - wireguard
```

```bash
ansible-playbook site.yml --tags wireguard
```

## License

MIT

## Author Information

<https://charlesmknox.com>
