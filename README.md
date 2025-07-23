# clusterkube
Criacao de um Cluster Kubernets em VMWare Workstation

1. Criar 3 VMs com 1 processador de 2 cores e 4 GB RAM
2. Ter conectividade entre as 3 maquinas
3. Configurar Hostname, IP, para cada uma das maquinas
4. Pare o Firewall do Sistema Operacional
  systemctl disable firewalld
  systemctl stop firewalld
5. Desative o SWAP do Sistema Operacional
  Edite o /etc/fstab e 'comente' a linha de swap. Apos isso reinicie a VM
6. Atualizar o Sistema Operacional
  sudo yum -y update
7. Instalar o epel-realease
sudo yum -y install epel-release
8.  Adicione os repositórios do CRI-O
    sudo curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable.repo https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/CentOS_9_Stream/devel:kubic:libcontainers:stable.repo
    sudo curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable:cri-o:${VERSION}.repo https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/1.28/CentOS_9_Stream/devel:kubic:libcontainers:stable:cri-o:1.28.repo
9.  Adicionar o








11. https://github.com/jedchaves/youtube/wiki/cluster_k8s
12. https://www.youtube.com/watch?v=ovOWA8YTcxI
13. https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd
14. https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
15. https://github.com/containerd/containerd/blob/main/docs/getting-started.md
16. https://github.com/containerd/containerd/releases
17. 




