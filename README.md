# 🧪 Kubernetes HA Lab

> Laboratório de Kubernetes com Alta Disponibilidade (HA) provisionado com **Vagrant**, automatizado com **Ansible** e construído utilizando práticas de **Infraestrutura como Código (IaC)**.

![Status](https://img.shields.io/badge/status-em%20desenvolvimento-orange)
![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.30-blue)
![Ansible](https://img.shields.io/badge/Ansible-Automation-red)
![Vagrant](https://img.shields.io/badge/Vagrant-IaC-1563FF)
![VirtualBox](https://img.shields.io/badge/VirtualBox-Lab-2F61B4)
![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04-E95420)
![License](https://img.shields.io/badge/license-MIT-green)

---

# 📑 Índice

* [Visão Geral](#-visão-geral)
* [Objetivos](#-objetivos-do-projeto)
* [Arquitetura](#️-arquitetura-do-ambiente)
* [Tecnologias Utilizadas](#️-tecnologias-utilizadas)
* [Estrutura do Projeto](#-estrutura-do-projeto)
* [Pré-requisitos](#-pré-requisitos)
* [Provisionando o Ambiente](#-provisionando-o-ambiente)
* [Validação do Cluster](#-validação-do-cluster)
* [Testes Realizados](#-testes-realizados)
* [Troubleshooting](#-troubleshooting)
* [Próximas Evoluções](#-próximas-evoluções)
* [Aprendizados](#-aprendizados)
* [Evolução do Projeto](#-evolução-do-projeto)
---

# 📌 Visão Geral

Este projeto implementa um ambiente de laboratório que simula um **cluster Kubernetes em Alta Disponibilidade (HA)** executando localmente sobre máquinas virtuais provisionadas com **Vagrant** e **VirtualBox**.

Todo o ambiente é configurado automaticamente utilizando **Ansible**, permitindo recriar o cluster sempre que necessário de forma padronizada e reproduzível.

O principal objetivo deste laboratório é reproduzir uma arquitetura semelhante à utilizada em ambientes corporativos, aplicando conceitos de:

* Infraestrutura como Código (IaC)
* Automação de Provisionamento
* Kubernetes High Availability
* Troubleshooting
* Redes Kubernetes
* Cluster Networking
* Administração de Control Plane
* Boas práticas de automação utilizando Ansible

Este projeto faz parte do meu processo de evolução profissional na área de **DevOps** e representa um ambiente criado para estudo, validação de conceitos e demonstração prática de conhecimentos.

---

# 🎯 Objetivos do Projeto

Este laboratório foi desenvolvido com os seguintes objetivos:

* Provisionar automaticamente toda a infraestrutura utilizando Vagrant.
* Automatizar completamente a configuração do cluster utilizando Ansible.
* Construir um cluster Kubernetes com múltiplos Control Planes.
* Implementar um ambiente com Alta Disponibilidade (HA).
* Simular uma topologia semelhante à encontrada em ambientes de produção.
* Documentar problemas encontrados durante a implementação e suas respectivas soluções.
* Servir como laboratório para futuras implementações envolvendo GitOps, Observabilidade, CI/CD e Cloud.

---

# 🏗️ Arquitetura do Ambiente

O ambiente é composto por:

| Máquina            | Função                   | IP            |
| ------------------ | ------------------------ | ------------- |
| ansible-controller | Controlador Ansible      | 192.168.56.5  |
| k8s-ha             | HAProxy (Load Balancer)  | 192.168.56.13 |
| k8s-master1        | Kubernetes Control Plane | 192.168.56.10 |
| k8s-master2        | Kubernetes Control Plane | 192.168.56.11 |
| k8s-master3        | Kubernetes Control Plane | 192.168.56.12 |
| k8s-worker1        | Worker Node              | 192.168.56.21 |
| k8s-worker2        | Worker Node              | 192.168.56.22 |

```
                     +---------------------------+
                     |       HAProxy             |
                     |     192.168.56.13         |
                     +-------------+-------------+
                                   |
        ---------------------------------------------------------
        |                       |                       |
+---------------+      +---------------+      +---------------+
| k8s-master1   |      | k8s-master2   |      | k8s-master3   |
|192.168.56.10  |      |192.168.56.11  |      |192.168.56.12  |
+---------------+      +---------------+      +---------------+
        |
---------------------------------------------------------------
        |                                     |
+----------------+                  +----------------+
| k8s-worker1    |                  | k8s-worker2    |
|192.168.56.21   |                  |192.168.56.22   |
+----------------+                  +----------------+

                Ansible Controller
                  192.168.56.5
```

---

# ⚙️ Tecnologias Utilizadas

| Tecnologia           | Finalidade                               |
| -------------------- | ---------------------------------------- |
| Vagrant              | Provisionamento das máquinas virtuais    |
| VirtualBox           | Virtualização do ambiente                |
| Ansible              | Automação da configuração dos servidores |
| Kubernetes (kubeadm) | Orquestração de containers               |
| containerd           | Runtime de containers                    |
| Calico               | Plugin de rede (CNI)                     |
| HAProxy              | Balanceamento do Control Plane           |
| Ubuntu Server 22.04  | Sistema Operacional                      |

---

# 📂 Estrutura do Projeto

```
k8s-ha-lab/
│
├── Vagrantfile
├── README.md
│
├── ansible
│   ├── hosts.ini
│   ├── site.yml
│   ├── files
│   └── roles
│       ├── containerd
│       ├── k8s-all-node
│       ├── k8s-master-node
│       ├── k8s-worker-node
│       ├── k8s-HA
│       └── k8s-calico
│
└── key
```

Cada role possui uma responsabilidade específica dentro da construção do cluster, seguindo uma organização modular que facilita manutenção, reutilização e evolução do projeto.

---

# 📋 Pré-requisitos

Para reproduzir este laboratório é necessário possuir instalado:

* VirtualBox
* Vagrant
* Git
* Aproximadamente 10 GB de memória RAM disponível
* Aproximadamente 40 GB de espaço em disco

Sistema operacional utilizado durante o desenvolvimento:

* Windows 11



# 🚀 Provisionando o Ambiente

Esta seção descreve o processo completo para criar o laboratório desde o início.

## Etapa 1 — Clonar o projeto

**Executar na máquina Host (Windows/Linux).**

Clone o repositório:

```bash
git clone https://github.com/SEU_USUARIO/k8s-ha-lab.git
```

Entre na pasta do projeto:

```bash
cd k8s-ha-lab
```

---

# Etapa 2 — Provisionar as Máquinas Virtuais

**Executar na máquina Host.**

O comando abaixo criará todas as máquinas virtuais definidas no `Vagrantfile`.

```bash
vagrant up
```

Ao término da execução, as seguintes VMs deverão estar disponíveis:

| Máquina            | Função              |
| ------------------ | ------------------- |
| ansible-controller | Controlador Ansible |
| k8s-ha             | HAProxy             |
| k8s-master1        | Control Plane       |
| k8s-master2        | Control Plane       |
| k8s-master3        | Control Plane       |
| k8s-worker1        | Worker              |
| k8s-worker2        | Worker              |

Você pode verificar utilizando:

```bash
vagrant status
```

Resultado esperado:

```text
ansible-controller    running
k8s-ha                running
k8s-master1           running
k8s-master2           running
k8s-master3           running
k8s-worker1           running
k8s-worker2           running
```

---

# Etapa 3 — Acessar o Ansible Controller

Toda a automação será executada a partir do controlador Ansible.

**Executar na máquina Host.**

```bash
vagrant ssh ansible-controller
```

---

# Etapa 4 — Acessar os arquivos do projeto

O Vagrant sincroniza automaticamente a pasta do projeto com a máquina virtual.

Dentro do `ansible-controller`, acesse:

```bash
cd /k8s-ha-lab/ansible
```

Verifique os arquivos disponíveis:

```bash
ls
```

Resultado esperado:

```text
files/
hosts.ini
roles/
site.yml
```

---

# Etapa 5 — Verificar conectividade entre os nós

Antes de iniciar o provisionamento, é possível validar a comunicação SSH entre o controlador e os servidores.

```bash
ansible all -i hosts.ini -m ping
```

Resultado esperado:

```text
k8s-master1 | SUCCESS
k8s-master2 | SUCCESS
k8s-master3 | SUCCESS
k8s-worker1 | SUCCESS
k8s-worker2 | SUCCESS
k8s-ha      | SUCCESS
```

---

# Etapa 6 — Executar o Playbook

Ainda no `ansible-controller`, execute:

```bash
ansible-playbook -i hosts.ini site.yml
```

Durante a execução, o Ansible realizará automaticamente:

* Atualização do sistema operacional;
* Instalação dos pacotes básicos;
* Configuração do containerd;
* Instalação do Kubernetes;
* Configuração do kubelet;
* Inicialização do primeiro Control Plane;
* Inclusão dos demais Masters no cluster;
* Inclusão dos Workers;
* Instalação do Calico (CNI);
* Configuração do HAProxy.

Ao final da execução o cluster deverá estar totalmente operacional.

---

# 🔍 Validando o Cluster

Após a conclusão do playbook, saia do controlador:

```bash
exit
```

Agora conecte-se ao primeiro Master.

**Executar na máquina Host.**

```bash
vagrant ssh k8s-master1
```

---

## Verificar os Nodes

```bash
kubectl get nodes
```

Resultado esperado:

```text
NAME          STATUS   ROLES
k8s-master1   Ready    control-plane
k8s-master2   Ready    control-plane
k8s-master3   Ready    control-plane
k8s-worker1   Ready
k8s-worker2   Ready
```

---

## Verificar os endereços IP

```bash
kubectl get nodes -o wide
```

Resultado esperado:

```text
192.168.56.10
192.168.56.11
192.168.56.12
192.168.56.21
192.168.56.22
```

Esses endereços devem corresponder aos IPs definidos no arquivo `hosts.ini`.

---

## Verificar os componentes do Kubernetes

```bash
kubectl get pods -n kube-system -o wide
```

Todos os Pods devem estar no estado:

```text
Running
```

Componentes esperados:

* kube-apiserver
* kube-controller-manager
* kube-scheduler
* etcd
* kube-proxy
* calico-node
* calico-kube-controller
* coredns

Caso todos estejam em execução, o cluster foi provisionado com sucesso.


# ✅ Testes Funcionais

Após a criação do cluster, foram realizados testes para validar o funcionamento dos principais componentes do Kubernetes.

---

# Teste 1 — Criação de um Pod

O primeiro teste consiste em criar um Pod simples utilizando a imagem **BusyBox**.

> **Executar no nó `k8s-master1`.**

```bash
kubectl run teste \
--image=busybox \
--restart=Never \
-- sleep 3600
```

Verifique se o Pod foi criado corretamente.

```bash
kubectl get pods -o wide
```

Resultado esperado:

```text
NAME      READY   STATUS    IP               NODE
teste     1/1     Running   192.168.xxx.xxx  k8s-workerX
```

Este teste confirma que:

* O Scheduler está funcionando;
* O kubelet recebeu a solicitação;
* O containerd iniciou o container;
* O Calico atribuiu um endereço IP ao Pod.

---

# Teste 2 — Acesso ao Pod utilizando kubectl exec

Agora será validada a comunicação entre o API Server e o kubelet.

Entre no Pod:

```bash
kubectl exec -it teste -- sh
```

Resultado esperado:

```text
/ #
```

Este teste confirma que:

* API Server ↔ kubelet
* API Server ↔ container runtime
* Exec remoto funcionando

---

# Teste 3 — Resolução DNS

Ainda dentro do Pod execute:

```bash
nslookup kubernetes.default.svc.cluster.local
```

Resultado esperado:

```text
Server: 10.96.0.10

Name:
kubernetes.default.svc.cluster.local

Address:
10.96.0.1
```

Este teste valida:

* CoreDNS
* Comunicação com Services
* DNS interno do cluster

Saia do Pod.

```bash
exit
```

---

# Teste 4 — Criando um Deployment

Criar um Deployment contendo três réplicas do NGINX.

```bash
kubectl create deployment nginx \
--image=nginx \
--replicas=3
```

Verifique os Pods.

```bash
kubectl get pods -o wide
```

Resultado esperado:

```text
nginx-xxxxxxxx Running
nginx-xxxxxxxx Running
nginx-xxxxxxxx Running
```

Observe que os Pods são distribuídos automaticamente entre os Workers.

Este teste valida:

* Scheduler
* ReplicaSet
* Deployment Controller

---

# Teste 5 — Expondo um Service

Criar um Service ClusterIP.

```bash
kubectl expose deployment nginx \
--port=80 \
--target-port=80 \
--type=ClusterIP
```

Verificar:

```bash
kubectl get svc nginx
```

Resultado esperado:

```text
NAME    TYPE        CLUSTER-IP
nginx   ClusterIP   10.x.x.x
```

---

# Teste 6 — Comunicação entre Pods

Entrar novamente no Pod BusyBox.

```bash
kubectl exec -it teste -- sh
```

Consumir o Service.

```bash
wget -qO- http://nginx
```

Resultado esperado:

```html
Welcome to nginx!
```

Este teste confirma:

* Service
* kube-proxy
* Balanceamento interno
* Comunicação entre Pods
* DNS interno

---

# Teste 7 — Distribuição dos Pods

Verificar onde cada Pod foi criado.

```bash
kubectl get pods -o wide
```

Exemplo:

```text
nginx-xxxxx     Running     k8s-worker1

nginx-xxxxx     Running     k8s-worker2

nginx-xxxxx     Running     k8s-worker1
```

Isso demonstra que o Scheduler distribuiu automaticamente as cargas de trabalho entre os nós disponíveis.

---

# Teste 8 — Validação Final

Verificar o estado geral do cluster.

```bash
kubectl get nodes
```

```bash
kubectl get pods -A
```

```bash
kubectl get svc
```

Todos os componentes devem permanecer em estado **Ready** ou **Running**.

---

# ✔ Resultado

Ao término dos testes foi possível validar:

* Provisionamento automatizado das VMs;
* Configuração automática via Ansible;
* Cluster Kubernetes em Alta Disponibilidade;
* Comunicação entre Control Plane e Workers;
* Funcionamento do Scheduler;
* Funcionamento do containerd;
* Comunicação entre Pods;
* DNS interno;
* Services ClusterIP;
* Balanceamento interno de Pods;
* Execução remota utilizando `kubectl exec`;
* Distribuição automática das cargas entre os Workers.

Esses testes demonstram que o ambiente está operacional e apto para servir como base para estudos e futuras implementações envolvendo GitOps, Observabilidade, CI/CD e ambientes Cloud.



# 🔍 Troubleshooting

Durante o desenvolvimento do laboratório foram encontrados alguns desafios técnicos. Em vez de apenas aplicar correções, cada problema foi investigado até sua causa raiz e solucionado através da automação do Ansible.

---

## Problema: Nodes registrados com endereço IP incorreto

### Sintoma

Após a criação do cluster, algumas operações apresentavam comportamento inconsistente.

O principal sintoma observado era a falha na execução de comandos como:

```bash
kubectl exec -it <pod> -- sh
```

retornando mensagens semelhantes a:

```text
unable to upgrade connection: pod does not exist
```

Apesar disso:

- Os Pods eram criados normalmente;
- Os Services funcionavam corretamente;
- O cluster aparentava estar saudável.

---

### Investigação

A investigação começou verificando o estado geral do cluster.

Foi analisado o registro dos Nodes:

```bash
kubectl get nodes -o wide
```

Em seguida foi verificado como o kubelet estava sendo inicializado:

```bash
cat /etc/default/kubelet

ps aux | grep kubelet
```

Durante essa análise foi identificado que o kubelet estava utilizando automaticamente o endereço IP da interface NAT do VirtualBox, em vez da interface Host-Only utilizada para comunicação entre os Nodes.

---

### Causa raiz

Como as máquinas virtuais possuem duas interfaces de rede (NAT e Host-Only), o kubelet selecionava automaticamente uma interface incorreta.

Isso fazia com que o Node fosse registrado no cluster utilizando um endereço IP diferente daquele utilizado para comunicação interna.

Como consequência, o API Server não conseguia estabelecer corretamente algumas conexões com o kubelet, afetando operações como o `kubectl exec`.

---

### Solução

A solução foi automatizar a configuração do kubelet através do Ansible, utilizando o endereço IP definido no inventário (`hosts.ini`).

Foi adicionada a configuração:

```text
KUBELET_EXTRA_ARGS=--node-ip={{ ansible_host }}
```

Após a alteração, o laboratório foi recriado para garantir que todos os Nodes fossem registrados utilizando seus endereços corretos.

---

### Resultado

Após a reconstrução do cluster:

- Todos os Nodes passaram a registrar corretamente seus endereços IP;
- O `kubectl exec` passou a funcionar normalmente;
- A comunicação entre os componentes do cluster tornou-se estável;
- Não foram necessárias outras alterações para corrigir o problema.

---

### Lições aprendidas

Embora o cluster tenha sido criado com sucesso, configurações automáticas podem introduzir comportamentos difíceis de identificar.

Definir explicitamente o endereço IP utilizado pelo kubelet torna o ambiente previsível, especialmente em laboratórios com múltiplas interfaces de rede, além de evitar problemas de comunicação entre os componentes internos do Kubernetes.

# 📈 Evolução do Projeto

Durante o desenvolvimento o projeto passou por diversas refatorações até atingir sua arquitetura atual.

Algumas melhorias implementadas ao longo da evolução:

- Reestruturação completa para utilização de Ansible Roles;
- Separação da inicialização do cluster e processo de Join dos Nodes;
- Automação da instalação do Containerd;
- Automatização da configuração dos módulos do kernel e parâmetros `sysctl`;
- Organização dos arquivos de Join em diretório dedicado;
- Implementação de um Load Balancer (HAProxy) para o Control Plane;
- Automatização da configuração do kubelet utilizando o inventário do Ansible (`hosts.ini`);
- Integração da instalação do Calico ao fluxo de provisionamento.