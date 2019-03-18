<!-- Header (auto generated) -->
![](https://img.shields.io/github/license/while-true-do/clt2019-lecture.svg?style=flat)
![](https://img.shields.io/github/issues/while-true-do/clt2019-lecture.svg?style=flat)

# clt2019-lecture
<!-- ./Header (auto generated) -->
Repository to publish the lecture "Setup your workstation with Ansible" for CLT2019.

## Motivation

[while-true-do.io](https://while-true-do.io) is participating at
[Chemnitzer Linux-Tage 2019](https://chemnitzer.linux-tage.de/2019/de).

## Description

Our lecture participation is stored in this repository. Additional information
can be found on the CLT Pages. In 2019 we are having one lecture "Deploy your
workstation with Ansible".

-   [CLT Website](https://chemnitzer.linux-tage.de/2019/de)
-   [Lecture Link](https://chemnitzer.linux-tage.de/2019/de/programm/beitrag/confirm/268/d3316590de44588a7529400f3e277e44)

## Requirements

To read this lecture, you need a browser.

The following tools are used to create the presentation.

-  [markdown](https://daringfireball.net/projects/markdown/)
-  [remark](https://github.com/gnab/remark)
-  [style-cheat](https://github.com/while-true-do/style-cheat)
-  [font-awesome](https://fontawesome.com/)

To execute the Demo, you will need [Ansible](https://www.ansible.com). The
installation of Ansible is described in the presentation.

## Installation

Install from git:

```
git clone https://github.com/while-true-do/clt2019-lecture.git
```

## Usage

The repository contains 2 parts. The presentation and a demo.

### Presentation

Open the [PRESENTATION.html](./presentation/PRESENTATION.html) in your browser.

### Demo

Adjust user and paths beforehand in [workstation.yml](./material/workstation.yml).
The playbook is written for a [Fedora workstation](https://getfedora.org), but
can be adjusted easily for other distributions.

```
ansible-playbook -K workstation.yml
```

## Testing

Most of the "generic" tests are located our
[Test Library](https://github.com/while-true-do/test-library). Tests and
instructions for a single repository are located in the
[Test Directory](./tests).

## Contribute

Thank you so much for considering to contribute. We are very happy, when somebody
is joining the hard work. Please fell free to open
[Bugs, Feature Requests](https://github.com/while-true-do/clt2019-lecture/issues) or
[Pull Requests](https://github.com/while-true-do/clt2019-lecture/pulls) after reading the [Contribution Guideline](https://github.com/while-true-do/doc-library/blob/master/documents/CONTRIBUTING.md).

## License

This work is licensed under a [BSD-3-Clause License](https://opensource.org/licenses/BSD-3-Clause).

## Contact

-   Site <https://while-true-do.io>
-   Twitter <https://twitter.com/wtd_news>
-   Code <https://github.com/while-true-do>
-   Mail [hello@while-true-do.io](mailto:hello@while-true-do.io)
-   IRC [freenode, #while-true-do](https://webchat.freenode.net/?channels=while-true-do)
-   Telegram <https://t.me/while_true_do>

<!-- ./Footer (auto generated) -->
