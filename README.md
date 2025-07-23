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
6. Instalar o containerd
7. https://github.com/jedchaves/youtube/wiki/cluster_k8s
8. https://www.youtube.com/watch?v=ovOWA8YTcxI
9. https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd
10. https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
11. https://github.com/containerd/containerd/blob/main/docs/getting-started.md
12. https://github.com/containerd/containerd/releases
13. 




