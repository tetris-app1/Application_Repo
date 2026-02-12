Role Name
=========

helm

A role to install Helm (Kubernetes package manager) on a target Linux host.

---

Requirements
------------

- Ansible 2.9+
- Linux-based system
- Privilege escalation enabled (`become: true`)
- Internet access on the target host to download Helm from:
  https://get.helm.sh/

No additional Python libraries are required.

---

Role Variables
--------------

The following variables can be configured:

        helm_version--> Specifies the Helm version to install.
        
        helm_os--> Operating system (linux, darwin, windows).
        
        helm_arch--> System architecture (amd64, arm64).
        
        helm_tarball--> Generated Helm archive name.
        
        helm_url--> Download URL for the Helm release.


Dependencies
------------

This role has no external role dependencies.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

       - hosts: servers
         become: true
         roles:
            - role: helm

Run with:

    ansible-playbook -i inventory install-helm.yml
            


License
-------

BSD

Author Information
------------------

Lina Mohamed
