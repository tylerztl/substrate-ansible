# Elara Ansible

## Requirements
- [Ansible >=2.9](https://docs.ansible.com/ansible/latest/installation_guide/index.html)

## Examples
### 1. 启动 kusama 节点
```
// ssh pub key
ansible-playbook -i inventory.yml playbooks/kusama-validator.yml -b

// ssh password
ansible-playbook -i inventory.yml playbooks/kusama-validator.yml -b --extra-vars "ansible_become_pass='$SUDO_PW'"
```

## Ansible Guide
ansible是一种自动化运维工具，基于Python开发，集合了众多运维工具（puppet、cfengine、chef、func、fabric）的优点，实现了批量系统配置、批量程序部署、批量运行命令等功能。

### Playbooks
playbook是ansible用于配置，部署，和管理被控节点的剧本。通过playbook的详细描述，执行其中的一系列tasks，可以让远端主机达到预期的状态。

### Roles

- defaults：角色会使用到的变量可以写入到此目录中的main.yml文件中，通常，defaults/main.yml文件中的变量都用于设置默认值，以便在你没有设置对应变量值时，变量有默认的值可以使用，定义在defaults/main.yml文件中的变量的优先级是最低的。

- files：角色可能会用到的一些其他文件可以放置在此目录中，比如，当你定义nginx角色时，需要配置https，那么相关的证书文件即可放置在此目录中。

- handlers：当角色需要调用handlers时，默认会在此目录中的main.yml文件中查找对应的handler

- meta：如果你想要赋予这个角色一些元数据，则可以将元数据写入到meta/main.yml文件中，这些元数据用于描述角色的相关属性，比如 作者信息、角色主要作用等等，你也可以在meta/main.yml文件中定义这个角色依赖于哪些其他角色，或者改变角色的默认调用设定，在之后会有一些实际的示例，此处不用纠结。

- tasks：角色需要执行的主任务文件放置在此目录中，默认的主任务文件名为main.yml，当调用角色时，默认会执行main.yml文件中的任务，你也可以将其他需要执行的任务文件通过include的方式包含在tasks/main.yml文件中。

- templates： 角色相关的模板文件可以放置在此目录中，当使用角色相关的模板时，如果没有指定路径，会默认从此目录中查找对应名称的模板文件。

- vars：角色会使用到的变量可以写入到此目录中的main.yml文件中，vars/main.yml文件和defaults/main.yml文件的区别在哪里呢？区别就是，defaults/main.yml文件中的变量的优先级是最低的，而vars/main.yml文件中的变量的优先级非常高，如果你只是想提供一个默认的配置，那么你可以把对应的变量定义在defaults/main.yml中，如果你想要确保别人在调用角色时，使用的值就是你指定的值，则可以将变量定义在vars/main.yml中，因为定义在vars/main.yml文件中的变量的优先级非常高，所以其值比较难以覆盖。

### Inventory

Ansible 可同时操作属于一个组的多台主机,组和主机之间的关系通过 `inventory` 文件配置. 默认的文件路径为 /etc/ansible/hosts,也可通过`-i`命令指定。目的就是存储主机与组之间的关系。

### Molecule （optional）
molecule旨在帮助开发和测试Ansible的Role。通过在虚拟机或容器上为正在运行的Ansible Role的测试构
建脚手架，我们无需再手工创建这些测试环境。Molecule利用Vagrant，Docker和OpenStack来管理虚拟机或容器，并支持Serverspec、Testinfra或Goss来运行测试。在sequence facility model中的默认步骤包括：虚拟机管理，Ansible语法静态检查，幂等性测试和收敛性测试。
