# Ansible

### **Core Concepts**

* **Agentless**: Works over SSH/WinRM. No agent installed on target hosts.
* **Idempotent**: Running the same play multiple times leads to the same state.
* **Declarative**: You describe the desired final state; modules handle the how.
* **YAML-based Playbooks**: Tasks written as YAML documents.
* **Inventory**: List of hosts — static file, dynamic script, cloud plugin.
* **Modules**: Reusable units of work (`apt`, `yum`, `copy`, `template`, `service`).
* **Roles**: Standard structure for reusable automation (tasks, handlers, templates).
* **Facts**: System information gathered automatically (`ansible_facts`).

***

### **Inventory**

* Default: `/etc/ansible/hosts`.
* Supports **groups**, **nested groups**, **variables per host/group**.
* Supports **dynamic inventory** (AWS, GCP, Azure, Kubernetes…).
* Host vars override group vars → group vars override global vars.

***

### **Playbooks**

* YAML file describing plays: hosts + tasks.
* Each **task** uses a module.
* Use **handlers** for notification-based tasks (restart service only when needed).
* Use **when** for conditionals.
* Use **loops** for repetitive tasks.
* Use **become: yes** for privilege escalation.

***

### **Variables**

* Several places: playbooks, inventory, `group_vars/`, `host_vars/`, vars\_files, extra vars.
* **Precedence**: extra vars > task vars > block vars > play vars > inventory.
* Use `{{ variable }}` syntax.
* **Jinja2 templating** for complex expressions.

***

### **Roles**

* Roles are reusable automation units.
* Standard directories: `tasks`, `handlers`, `templates`, `files`, `vars`, `defaults`, `meta`.
* **defaults** have lowest priority; **vars** have higher.
* Galaxy roles installed using `ansible-galaxy install`.

***

### **Modules**

Most common modules for interviews:

* **Package management**: `apt`, `yum`, `package`.
* **Service control**: `service`, `systemd`.
* **User management**: `user`, `group`.
* **File ops**: `copy`, `file`, `template`, `synchronize`, `unarchive`.
* **Networking**: `uri`, `iptables`, `docker_*`, `k8s`.
* **System**: `shell`, `command`, `raw` (last resort).

**Key difference:**

* `shell` → runs through shell (supports pipes, redirects).
* `command` → no shell, safer, no pipes.

***

### **Handlers**

* Run only when notified from a task.
* Used for restart/reload actions.
* Executed at the end of a play by default.

***

### **Templates**

* Use `template:` + Jinja2.
* Supports variables, filters, loops.
* Useful for dynamic configs: Nginx, HAProxy, Prometheus, etc.

***

### **Ansible Vault**

* Encrypts sensitive files/vars.
* Use `ansible-vault encrypt file.yml`.
* Use `--ask-vault-pass` or `--vault-password-file`.
* Vault files can be used inside playbooks like normal vars.

***

### **Execution**

* `ansible all -m ping -i inventory.ini` → test hosts.
* `ansible-playbook site.yml` → run playbook.
* `ansible-playbook site.yml --check` → dry-run (no changes).
* `ansible-playbook site.yml --diff` → show config changes.
* `ansible-playbook site.yml -e var=value` → extra vars.
* `ansible-galaxy init role_name` → create a new role skeleton.

***

### **Idempotency**

* Modules should report **changed** only when state changed.
* Using `shell` or `command` breaks idempotency unless manually controlled (`creates=`, `removes=`).

***

### **Common Interview Questions**

* Difference between **Playbook** and **Role**.
* Difference between **Vars**, **defaults**, **extra vars**, **host\_vars/group\_vars**.
* How Ansible achieves **idempotency**.
* When to use **shell** vs **command**.
* What is **Ansible Vault** and how to use it.
* How **handlers** work and why they are important.
* Static vs **dynamic inventory**.
* How to structure a large project (roles, group\_vars, host\_vars).
* How to troubleshoot Ansible (verbosity: `-vvv`).
* What is **check mode** (`--check`) and why it's critical.

***

### **Best Practices**

* Always use roles for non-trivial projects.
* Keep secrets in Vault.
* Avoid `shell` unless necessary.
* Use `tags:` to speed up runs.
* Use `become: yes` instead of `sudo` in commands.
* Keep playbooks small; reuse roles.
* Use dynamic inventory for cloud environments.
* Test with `--check` before you break production.
