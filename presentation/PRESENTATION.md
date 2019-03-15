class: title, middle, center
<!-- css classes -->

# Deploy your workstation with Ansible

<!-- Notes -->
???
Keep it short.

---
## Introduction

#### <i class="fas fa-user fa-fw"></i> Daniel Schier
#### <i class="fas fa-birthday-cake fa-fw"></i> Born before the internet
#### <i class="fas fa-home fa-fw"></i> Dresden, Germany
----
#### <i class="fas fa-graduation-cap fa-fw"></i> Watched "IT Crowd"
#### <i class="fas fa-lightbulb fa-fw"></i> Inspired by these cool "Hacker guys" in TV
#### <i class="fas fa-hammer fa-fw"></i> Having fun coding, operating and managing IT

---
## <i class="fas fa-table fa-fw"></i> Table of Contents

-   Ansible
-   Use Case
-   Installing Ansible
-   Creating the Playbook
-   User and Groups
-   Packages
-   Services
-   Configuration
-   Flatpak
-   Demo
-   Propaganda

You can find the content at https://github.com/while-true-do/clt2019-lecture

---
class: title, middle, center

## Ansible

Ansible loves repetitive work, that you hate.

---

## Ansible

-   Cloud Provisioning
-   Configuration Management
-   Infrastructure Automation
-   Container (Docker, CRI-O, ...)
-   Orchestration
-   Workstation Post Installations(!!!)

---
class: title, middle, center

## Use Case(s)

You can automate windows, mac and linux.

???
Since this is CLT, we focus on Linux.

---
class: title, middle, center

## Installing Ansible

Ansible is built for most major distributions.

---
## <i class="fas fa-terminal fa-fw"></i> Installing Ansible

In most distributions, you just need to use the proper package manager.

```
# Fedora
sudo dnf install ansible
# RedHat, CentOs, SCL, Oracle Linux
sudo yum install ansible
# Debian / Ubuntu
sudo apt-get install ansible
# SuSE Linux
sudo zypper install ansible
```

???
Recommendation: pip + virtualenv

---
## <i class="fas fa-terminal fa-fw"></i> Installing Ansible

You can use virtualenv / pip, too.

```
# Via virtualenv/pip
# Additional steps may be needed for selinux
virtualenv --no-site-packages <ENV>
source <ENV>/bin/activate
pip install ansible
```

---
class: title, middle, center

## Creating the Playbook

Playbook => technical term for a \*.yaml file

---
## <i class="fas fa-file-code fa-fw"></i> Creating the Playbook

You only need one file for now.

```
touch workstation.yml
```

Add a few lines of [YAML](https://yaml.org/) code.

```
# workstation.yml
---
- hosts: localhost
  connection: local
  become: yes
  tasks:
```

---
class: title, middle, center

## User

User and group management is included and easy.

---
## <i class="fas fa-user fa-fw"></i> User

You can manage
[user](https://docs.ansible.com/ansible/latest/modules/user_module.html) accounts.

```
# workstation.yml
---
..snip..

  tasks:
    - name: Create User ds
      user:
        name: ds
        comment: Daniel
        groups: wheel
        append: True
        state: present

..snap..
```

---
## <i class="fas fa-user fa-fw"></i> User

You can also manage
[groups](https://docs.ansible.com/ansible/latest/modules/group_module.html).

```
# workstation.yml
---
..snip..

  tasks:
    - name: Create Group developers
      group:
        name: developers
        state: present

..snap..
```

---
class: title, middle, center

## Packages

Install, Remove and Update your software.

---
## <i class="fas fa-cube fa-fw"></i> Packages

Easily update
[packages](https://docs.ansible.com/ansible/latest/modules/package_module.html).

```
# workstation.yml
---
..snip..

  tasks:
    - name: Update System
      package:
        name: "*"
        state: latest

..snap..
```

---
## <i class="fas fa-cube fa-fw"></i> Packages

Let's remove some leftovers and unused packages.

```
# workstation.yml
---
..snip..

  tasks:
    - name: Remove Gnome Classic
      package:
        name: gnome-classic-session
        state: absent

    - name: Remove Anaconda Leftovers
      package:
        name: anaconda
        state: absent

..snap..
```
---

## <i class="fas fa-cube fa-fw"></i> Packages

It's a piece of cake to use
[variables](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html).

```
# workstation.yml
---
..snip..

  vars:
    vm_guest_packages:
      - open-vm-tools
      - virtualbox-guest-additions
      - qemu-guest-agent

  tasks:
    - name: Remove VM Guest Packages
      package:
        name: "{{ vm_guest_packages }}"
        state: absent

..snap..
```

---

## <i class="fas fa-cube fa-fw"></i> Packages

Installing additional packages is easy, too.

```
# workstation.yml
---
..snip..

  tasks:
    - name: Install tuned
      package:
        name: tuned
        state: present

..snap..
```

---
class: title, middle, center

## Services

Managing services is easy, too.

---
## <i class="fas fa-spinner fa-fw"></i> Services

Sometimes, you want to start and enable a
[service](https://docs.ansible.com/ansible/latest/modules/service_module.html).

```
# workstation.yml
---
..snip..

  tasks:
    - name: Start and Enable tuned
      service:
        name: tuned
        state: started
        enabled: true

..snap..
```

---
## <i class="fas fa-spinner fa-fw"></i> Services

Sometimes, you want to have a service restarted.

```
# workstation.yml
---
..snip..

  tasks:
    - name: Restart tuned
      service:
        name: tuned
        state: restarted

..snap..
```

---
## <i class="fas fa-spinner fa-fw"></i> Services

And sometimes, you want to do something after some other task with
[handlers](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#handlers-running-operations-on-change).

```
# workstation.yml
---
..snip..

  tasks:
    - name: Install tuned
      package:
        name: tuned
        state: present
      notify: Start and Enable tuned

  handlers:
    - name: Start and Enable tuned
      service:
        name: tuned
        state: started
        enabled: true

..snap..
```

---
class: title, middle, center

## Configuration

Doing Configurations is also quite easy.

---
## <i class="fas fa-wrench fa-fw"></i> Configuration

Sometimes, you need to perform
[commands](https://docs.ansible.com/ansible/latest/modules/command_module.html)
or [copy](https://docs.ansible.com/ansible/latest/modules/copy_module.html) a
file.

```
# workstation.yml
---
..snip..

  tasks:
    - name: Backup current dconf
      command: dconf dump / > .config/dconf.dump

    - name: Copy Wallpaper
      copy:
        src: files/wallpaper.jpg
        dest: /home/ds/pictures/wallpaper.jpg
        owner: ds
        group: ds

..snap..
```

---
## <i class="fas fa-wrench fa-fw"></i> Configuration

Maybe, you want to enable Gnomes Night Light Feature via
[dconf](https://docs.ansible.com/ansible/latest/modules/dconf_module.html).

```
# workstation.yml
---
..snip..

  tasks:
    - name: Enable Night Light
      dconf:
        key: "/org/gnome/settings-daemon/plugins/color/night-light-enabled"
        value: "true"
..snap..
```
---
## <i class="fas fa-wrench fa-fw"></i> Configuration

Maybe, you want to set a wallpaper.

```
# workstation.yml
---
..snip..

  tasks:
    - name: Set Gnome Wallpaper
      dconf:
        key: "/org/gnome/desktop/background/picture-uri"
        value: "'file:///home/ds/Pictures/wallpaper.jpg'"

..snap..
```

---
class: title, middle, center

## Flatpak

There is this thingy, everybody is talking about.

---
## <i class="fas fa-cube fa-fw"></i> Flatpak

First, you want to add a [repository](https://docs.ansible.com/ansible/latest/modules/flatpak_remote_module.html).

```
# workstation.yml
---
..snip..

  tasks:
    - name: Add the flathub repository
      flatpak_remote:
        name: flathub
        state: present
        flatpakrepo_url: https://dl.flathub.org/repo/flathub.flatpakrepo

..snap..
```

---
## <i class="fas fa-cube fa-fw"></i> Flatpak

Afterwars, install a
[flatpak](https://docs.ansible.com/ansible/latest/modules/flatpak_module.html)
from flathub.

```
# workstation.yml
---
..snip..

  tasks:
    - name: Install Atom Flatpak
      flatpak:
        name: io.atom.Atom
        state: present

..snap..
```

---
class: title, middle, center

## Demo

Let's see this in action. Running the [workstation.yml](./material/workstation.yml)

---
## <i class="fas fa-hands-helping fa-fw"></i> Propaganda

#### Ansible

-   [ansible website](https://www.ansible.com)
-   [ansible community](https://www.ansible.com/community)
-   [ansible github](https://github.com/ansible)
-   [Meetup Dresden](https://www.meetup.com/de-DE/Ansible-Dresden/)

#### while-true-do.io

-   [WTD website](https://while-true-do.io)
-   [WTD github](https://github.com/while-true-do)
-   [WTD twitter](https://twitter.com/wtd_news)

#### Profi.com AG (Sponsor)

-   [proficom.de](https://www.proficom.de/)

---
class: title, middle, center

## Fin

Thank you so much for participating and please feel free to join us at the
ansible community booth.
