# clusterkube
Criacao de um Cluster Kubernets em VMWare Workstation

1.  Criar 3 VMs com 1 processador de 2 cores e 4 GB RAM
2.  Ter conectividade entre as 3 maquinas
3.  Configurar Hostname, IP, para cada uma das maquinas
4.  Pare o Firewall do Sistema Operacional
    systemctl disable firewalld
    systemctl stop firewalld
5.  Desative o SWAP do Sistema Operacional
    Edite o /etc/fstab e 'comente' a linha de swap. Apos isso reinicie a VM
6.  Instalar Ferramentas adicionais
dnf install -y \
    conntrack \
    container-selinux \
    ebtables \
    ethtool \
    iptables \
    socat
    
7.  Atualizar o Sistema Operacional
    sudo yum -y update
8.  Instalar o epel-realease
    sudo yum -y install epel-release
9.  Adicione os repositórios do CRI-O


    cat <<EOF | tee /etc/yum.repos.d/cri-o.repo
    [cri-o]
    name=CRI-O
    baseurl=https://pkgs.k8s.io/addons:/cri-o:/prerelease:/main/rpm/
    enabled=1
    gpgcheck=1
    gpgkey=https://pkgs.k8s.io/addons:/cri-o:/prerelease:/main/rpm/repodata/repomd.xml.key
    EOF


    sudo curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable.repo https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/CentOS_9_Stream/devel:kubic:libcontainers:stable.repo
    sudo curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable:cri-o:1.28.repo https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/1.28/CentOS_9_Stream/devel:kubic:libcontainers:stable:cri-o:1.28.repo
10.  Instalar o cri-o
    sudo yum install cri-o cri-tools
11. Configuracao do Crio no startup da maquina
    sudo systemctl enable --now crio
12. Verificacao do CRIO se esta inicializado
    systemctl status crio
13. Instalacao do Chrony para sincronismo de horario
    yum install chrony
14. Instalar o servidor de certificado
    yum install ca-certificates
15. Instalar o lsb-release
    yum install lsb-realease
16. Instalar o gnupg
    yum install gnupg
19. Instalar o nfs-utils
    yum install nfs-utils
20. Subir os servicoes do NFS
    sudo systemctl enable --now rpcbind
    sudo systemctl enable --now nfs-client.target
21. Criar um diretorio para as chaves publicas e dar permissoes
    sudo mkdir -p /etc/keyrings
    sudo chmod 755 -R /etc/keyrings
23. Criar um repositorio do kubernets

    KUBERNETES_VERSION=v1.33
    CRIO_VERSION=v1.33

    cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
    [kubernetes]
    name=Kubernetes
    baseurl=https://pkgs.k8s.io/core:/stable:/v1.33/rpm/
    enabled=1
    gpgcheck=1
    gpgkey=https://pkgs.k8s.io/core:/stable:/v1.33/rpm/repodata/repomd.xml.key
    exclude=kubelet kubeadm kubectl cri-tools kubernetes-cni
    EOF


    
25. Baixar as chaves publicas do kubernets
    curl -fsSL https://packages.cloud.google.com/yum/doc/yum-key.gpg | gpg --import
    curl -fsSL https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg | gpg --import

26. Instalar o selinux
    dnf install -y container-selinux


26. Configurar o SELinux
    sudo setenforce 0
    sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
27. Listar as versoes do kubernets
    sudo yum list kubelet --showduplicates --disableexcludes=kubernetes
28. Instalar os pacotes do Kubernets
    sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
29. 


    

kubeadm --cri-socket /var/run/crio/crio.sock


 








12. https://github.com/jedchaves/youtube/wiki/cluster_k8s
13. https://www.youtube.com/watch?v=ovOWA8YTcxI
14. https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd
15. https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
16. https://github.com/containerd/containerd/blob/main/docs/getting-started.md
17. https://github.com/containerd/containerd/releases
18. 




