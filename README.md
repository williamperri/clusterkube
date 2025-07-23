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
6.




