$# AnsibleTest
Teste de Conhecimentos

# Criei um ambiente com 2 maquinas virtuais Linux e meu notebook Windows para laboratorio e execução do Teste
# Utilizei o CentOS 8 , sendo 1 máquina sendo meu Principal "Master" onde instalei o Ansible. 

#rodei o comando abaixo para instalar um repositorio externo e habilitar pacotes para depois instalar o ansible
yum install epel-release -y

# apos instalar o repositorio instalei o ansible

yum install ansible -y

#Tambem estudei que pode-se cruar arquivos com listas de ips de servidores, tanto no etc/ansible/hosts quanto criar um arquivo de inventario pelo vim inventorylist e dentro dele 
#colocar os ips dos hosts

Exemplo
#criei um arquivo chamado inventoryLIst com a lista de ip dos servers com o comando
vim invetoryList

192.168.1.3 #linux
192.168.1.4 #windows

ou podese editar o arquivo /etc/ansible/hosts criando um grupo de maquinas ou lista de serves nesse caso nomeiei de datacenter_serasa
vim /etc/ansible/hosts
[datacenter_serasa]
192.168.1.3
192.168.1.4

#Pesquisei e pode-se executar comandos ansible AD HOC ou via playbook , em ad hoc é a execução do comando passando os parametros e os modulos (ja existem muitos modulos ja criados que pode-se usar) , já via playbook é um arquivo de configuracao para orquestrar uma ou mais  tafefas ou "plays" como é chamado.

# 1 - Ansible
# Utilizando o Ansible pedimos que desenvolva códigos para solucionar os seguintes cenários:
# A.	Efetuar a alteração de IP, máscara de sub rede e gateway em servidores Linux e Windows

# com ad hoc executei o seguinte comando para mudar o ip do server linux

ansible -i /home/dhief/inventoryList 192.168.1.3 -m nmcli -a "conn_name=enp0s3  ifname=enp0s3 type=ethernet ip4=192.168.1.11/24 gw4=192.168.1.1 state=present "  -k

# ou via playbook o arquivo changeNetwork.yml esta anexo no git

ansible-playbook -e "host=192.168.1.3 ip_class=192.168.1.25/24 gat=192.168.1.1" changeNetwork.yml -k

#Para esta tarefa no Windows usando playbook com o modulo winrm no arquivo  changeNetworkwin.yml anexo no git

ansible-playbook -e "win_server=192.168.1.4 ip_class=192.168.1.40/24 gat=192.168.1.1" certificado=/path/to/certificate/public/key.pem certificado_chave=/path/to/certificate/private/key.pem changeNetworkwin.yml -k

# B.	Instalação e configuração
# o	Instalar zabbix-agent 
# o	Criar uma condicional para preencher o parâmetro “Server” com diferentes valores no arquivo de configuração, baseado no terceiro octeto do IP, onde está sendo instalado zabbix-agent 

# Para esta tarefa utilizei playbook que com ele é possivel orquestrar as tarefas por exemplo primeiro instalar repositorio do zabbix depois instalar , pegar o terceiro octeto  # da faixa de ip e de acordo com a faixa mudar o parametro server dentro do /etc/zabbix/zabbix_agentd.conf e depois reiniciar o serviço do zabbix-agent


ansible-playbook -e host=localhost installzabbix.yml -k

#segue anexo o arquivo no git installzabbix.yml


# 2 - Identificação de problemas
# No seguinte cenário, foi executado o playbook RequestCert.yml, cujo retorno está a abaixo. Identifique o erro.
# Execução:
ansible-playbook RequestCert.yml --extra-vars 'domain= PLAINTEXT host_ip=PLAINTEXT ansible_fqdn=PLAINTEXT hostname=PLAINTEXT’ 

#Analisando o erro apresentado , eu Primeiro valido se existe uma regra no firewall para a porta , segundo se o servidor for mesmo linux , o erro de conexão deve-se ao executar #o modelo winrm (modulo para windows para um servidor Linux "serasa.linux"), ou seja uma conexao windows para um server linux, ja sendo windows verificaria o certificado. 









