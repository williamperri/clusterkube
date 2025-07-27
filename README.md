# clusterkube
Criacao de um Cluster Kubernets em VMWare Workstation

# 1.  Criar 1 VMs com 1 processador de 2 cores e 4 GB RAM
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
8.  Instalar o epel-realease
    sudo yum -y install epel-release

19. Instalar o servidor de certificado
    yum install ca-certificates
20. Instalar o lsb-release
    yum install lsb-realease
21. Instalar o gnupg
    yum install gnupg
22. Instalar o nfs-utils
    yum install nfs-utils
23. Subir os servicoes do NFS
    sudo systemctl enable --now rpcbind
    sudo systemctl enable --now nfs-client.target

27. Instalar o selinux
    dnf install -y container-selinux

16. Instalacao do Chrony para sincronismo de horario
    yum install chrony
    
7.  Atualizar o Sistema Operacional
    sudo yum -y update

    

9.  Adicione os repositórios do CRI-O
    cat <<EOF | tee /etc/yum.repos.d/cri-o.repo
    [cri-o]
    name=CRI-O
    baseurl=https://pkgs.k8s.io/addons:/cri-o:/prerelease:/main/rpm/
    enabled=1
    gpgcheck=1
    gpgkey=https://pkgs.k8s.io/addons:/cri-o:/prerelease:/main/rpm/repodata/repomd.xml.key
    EOF

10. Instalar o cri-o
    sudo yum install cri-o cri-tools
    
12. Configuracao do Crio no startup da maquina
    sudo systemctl enable --now crio
    
14. Verificacao do CRIO se esta inicializado
    systemctl status crio
    

17. Configurar o Chrony
    sed -i -e 's|^pool 2.centos.pool.ntp.org iburst$|pool a.ntp.br iburst|' -e '$a pool b.ntp.br iburst' -e '$a pool c.ntp.br iburst' /etc/chrony.conf



25. Criar um repositorio do kubernets

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


    
26. Baixar as chaves publicas do kubernets
    curl -fsSL https://packages.cloud.google.com/yum/doc/yum-key.gpg | gpg --import
    curl -fsSL https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg | gpg --import

33. Configuracao do NetFilter
    modprobe br_netfilter
34. Desativar o SELINUX
    sudo sed -i 's|^SELINUX=enforcing$|SELINUX=disabled|' /etc/selinux/config

26. Configurar o SELinux
    sudo setenforce 0
    sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
27. Listar as versoes do kubernets
    sudo yum list kubelet --showduplicates --disableexcludes=kubernetes
28. Instalar os pacotes do Kubernets
    sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
29. Configurar o IPv4 Forward
    sysctl params required by setup, params persist across reboots
    cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
    net.ipv4.ip_forward = 1
    EOF

sudo sysctl --system
Verify that net.ipv4.ip_forward is set to 1 with:

sysctl net.ipv4.ip_forward
31. Configurar os módulos de kernel necessários para o K8S/Containerd.
cat << EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
nf_nat
xt_REDIRECT
xt_owner
iptable_nat
iptable_mangle
iptable_filter
EOF

32. Criar o arquivo de configuracao do CRIO
    vi /etc/crio/crio-config.toml

[crio]
runtime = "runc"
storage_driver = "overlay"
log_level = "info"

[crio.network]
plugin_dirs = ["/opt/cni/bin"]
network_dir = "/etc/cni/net.d"

[crio.metrics]
enable_metrics = true
metrics_port = 9090

vi /etc/crio/crio.conf

[crio]
root = "/var/lib/containers/storage"
runroot = "/var/run/containers/storage"
storage_driver = "overlay"
log_level = "info"
log_dir = "/var/log/crio/pods"

[crio.api]
listen = "/var/run/crio/crio.sock"
stream_address = "127.0.0.1"
stream_port = "0"

[crio.runtime]
default_runtime = "runc"
conmon = "/usr/bin/conmon"
cgroup_manager = "systemd"
default_capabilities = ["CHOWN", "DAC_OVERRIDE", "FSETID", "FOWNER", "SETGID", "SETUID", "SETPCAP", "NET_BIND_SERVICE", "KILL"]
selinux = true
seccomp_profile = "/etc/crio/seccomp.json"

[crio.image]
pause_image = "registry.k8s.io/pause:3.10"


36. Instalar o Kubernets
    sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
    sudo kubeadm config images pull
    sudo systemctl enable --now kubelet

    
38. Desligar e Clonar 3x
39. Primeiro Clone, colocar como nome de IMAGEKUBE
40. Segundo e Terceiro Clone, sendo WORKER1 e WORKER2
41. Ligar os dois Workers e renomea-los (nmtui) e anotar os IPs
42. Configurar o arquivo Hosts das 3 maquinas com os nomes e IPs e FQDN
    Ex.
    x.x.x.x master01 master01.kube.local
    x.x.x.x worker01 worker01.kube.local
    x.x.x.x worker02 worker02.kube.local
43. Inicializacao do Master01
sudo kubeadm init \
  --node-name $(hostname -s) \
  --pod-network-cidr=10.244.0.0/16 \
  --service-cidr=10.245.0.0/16 \
  --apiserver-advertise-address=10.1.10.137

44. Configuracao do kubeconfig para gerenciar o cluster
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config

46. Configuracao do flannel CDN do Cluster
curl -O https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
kubectl apply -f kube-flannel.yml

    

47. adicionar os workers
kubeadm token create

openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt \
  | openssl rsa -pubin -outform der 2>/dev/null \
  | openssl dgst -sha256 -hex \
  | sed 's/^.* //'

  kubeadm join 10.1.10.137:6443 \
  --token abcdef.0123456789abcdef \
  --discovery-token-ca-cert-hash sha256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

48. configurar o node role
    kubectl label node <hostname node> node-role.kubernetes.io/worker= --overwrite

50. Verificar qual Container Run-time esta usando
    kubectl get nodes -o wide
51. 

sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

kubeadm --cri-socket /var/run/crio/crio.sock

=== Anotacoes ================================================================================================================================

9.  Adicione os repositórios do CRI-O
    sudo curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable.repo https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/CentOS_9_Stream/devel:kubic:libcontainers:stable.repo
    sudo curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable:cri-o:1.28.repo https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/1.28/CentOS_9_Stream/devel:kubic:libcontainers:stable:cri-o:1.28.repo

24. Criar um diretorio para as chaves publicas e dar permissoes
    sudo mkdir -p /etc/keyrings
    sudo chmod 755 -R /etc/keyrings


12. https://github.com/jedchaves/youtube/wiki/cluster_k8s
13. https://www.youtube.com/watch?v=ovOWA8YTcxI
14. https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd
15. https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
16. https://github.com/containerd/containerd/blob/main/docs/getting-started.md
17. https://github.com/containerd/containerd/releases
18. 




