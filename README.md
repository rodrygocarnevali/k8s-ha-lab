# k8s-ha-lab

🧪 **Laboratório de Kubernetes com Alta Disponibilidade (HA)**

Este projeto está sendo desenvolvido com o objetivo de simular um ambiente realista de Kubernetes com alta disponibilidade, utilizando **Vagrant** para provisionamento das VMs e **Ansible** para automação da configuração.

A ideia é criar um cluster Kubernetes com múltiplos masters e workers, além de um nó controlador com Ansible, para testes, aprendizado e demonstração de práticas DevOps.

> 🚧 *Projeto em desenvolvimento. Atualizações frequentes serão realizadas conforme a evolução do ambiente.*

---

## 📌 Objetivo final

Provisionar e configurar automaticamente um cluster Kubernetes com:

- 🧠 3 nós master
- 🧱 2 nós worker
- 🎛️ 1 nó controlador (Ansible)

---

## 🔧 Tecnologias utilizadas (até o momento)

- **Vagrant + VirtualBox** – Criação e gerenciamento de máquinas virtuais
- **Ansible** – Automação da configuração dos nós
- **Kubernetes** – Orquestração de contêineres
- **Linux (Ubuntu)** – Sistema base dos nós

---

## 📁 Estrutura inicial

```bash
k8s-ha-lab/
├── ansible/                # Estrutura do Ansible
│   ├── files/              # Arquivos estáticos
│   ├── group_vars/         # Variáveis por grupo de hosts
│   ├── host_vars/          # Variáveis por host
│   ├── playbooks/          # Playbooks organizados
│   ├── roles/              # Roles reutilizáveis do Ansible
│   ├── templates/          # Arquivos Jinja2 com variáveis
│   ├── hosts.ini           # Inventário dos hosts do Ansible
│   └── site.yml            # Playbook principal
├── Vagrantfile             # Arquivo principal para criação das VMs
└── README.md               # Este arquivo

## 🚀 Nota sobre as "Mordomias" do Vagrant

Para simplificação e agilidade neste primeiro projeto, usamos todos os recursos que facilitam a implantação com o Vagrant:

- **synced_folder**
- **Geração automática de chaves SSH** 
- **Integração direta com o VirtualBox** – Não é necessário configurar redes, storage ou VMs manualmente. O Vagrant cuida disso automaticamente.

