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
9.  Instalar o cri-o
    sudo yum install cri-o cri-tools
10. Configuracao do Crio no startup da maquina
    sudo systemctl enable --now crio
11. Verificacao do CRIO se esta inicializado
    systemctl status crio
12. Instalacao do Chrony para sincronismo de horario
    yum install chrony
13. Instalar o servidor de certificado
    yum install ca-certificates
15. Instalar o lsb-release
    yum install lsb-realease
17. Instalar o gnupg
    yum install gnupg
19. Instalar o nfs-utils
    yum install nfs-utils
20. Subir os servicoes do NFS
    sudo systemctl enable --now rpcbind
    sudo systemctl enable --now nfs-client.target
21. Criar um diretorio para as chaves publicas e dar permissoes
    sudo mkdir -p /etc/keyrings
    sudo chmod 755 -R /etc/keyrings
22. Criar um repositorio do kubernets
    cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
       https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
23. Baixar as chaves publicas do kubernets

24. dsdds


    

kubeadm --cri-socket /var/run/crio/crio.sock


 








12. https://github.com/jedchaves/youtube/wiki/cluster_k8s
13. https://www.youtube.com/watch?v=ovOWA8YTcxI
14. https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd
15. https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
16. https://github.com/containerd/containerd/blob/main/docs/getting-started.md
17. https://github.com/containerd/containerd/releases
18. 




