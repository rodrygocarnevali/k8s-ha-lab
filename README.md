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
│   .gitignore
│   README.md
│   Vagrantfile
│
│
└───ansible
    │   hosts.ini
    │   site.yml
    │
    ├───files
    │       .gitkeep
    │     
    │
    └───roles
        ├───containerd
        │   ├───tasks
        │   │       main.yml
        │   │
        │   └───vars
        │           main.yml
        │
        ├───k8s-all-node
        │   ├───tasks
        │   │       main.yml
        │   │
        │   └───vars
        │           main.yml
        │
        ├───k8s-cplane-node
        │   └───tasks
        │           main.yml
        │
        ├───k8s-HA
        │   └───tasks
        │           main.yml
        │
        ├───k8s-master-node
        │   ├───tasks
        │   │       main.yml
        │   │
        │   └───vars
        │           main.yml
        │
        └───k8s-worker-node
            └───tasks
                    main.yml