# AnsibleTest
#Teste de Conhecimentos

# Executei o comando abaixo para instalar um repositório externo e habilitar pacotes para depois instalar o ansible

yum install epel-release -y

# após instalar o repositorio instalei o ansible

yum install ansible -y

# Também estudei que pode-se criar arquivos com listas de ips de servidores, tanto no etc/ansible/hosts quanto criar um arquivo de inventário e dentro dele colocar os ips dos 
# hosts por exemplo criar uma lista de um park de máquinas;

#Exemplo

#Criar arquivo chamado inventoryLIst com a lista de ip dos servers

vim invetoryList

192.168.1.3 

192.168.1.4 

#ou pode-se editar o arquivo /etc/ansible/hosts criando um grupo de máquinas ou lista de serves exemplo datacenter_serasa

vim /etc/ansible/hosts

[datacenter_serasa]
192.168.1.3

192.168.1.5

#Pesquisei e pode-se executar comandos ansible AD HOC ou via playbook , em ad hoc é a execução do comando passando os parametros e os modulos (ja existem muitos modulos ja criados na documentação do ansible que pode-se usar) , já via playbook é um arquivo de configuração para orquestrar uma ou mais tafefas.

# 1 - Ansible
# Utilizando o Ansible pedimos que desenvolva códigos para solucionar os seguintes cenários:
# A.	Efetuar a alteração de IP, máscara de sub rede e gateway em servidores Linux e Windows

# via ad hoc executei o seguinte comando para mudar o ip do server linux

ansible -i /home/dhief/inventoryList 192.168.1.3 -m nmcli -a "conn_name=enp0s3  ifname=enp0s3 type=ethernet ip4=192.168.1.11/24 gw4=192.168.1.1 state=present "  -k

# ou via playbook o arquivo changeNetwork.yml esta anexo no git

ansible-playbook -e "host=192.168.1.3 ip_class=192.168.1.25/24 gat=192.168.1.1" changeNetwork.yml -k

# Para esta tarefa no Windows usando playbook com o modulo winrm no arquivo changeNetworkwin.yml anexo no git

#Para gerar um certificado pode-se usar OpenSSL ou PowerShell usando o New-SelfSignedCertificate cmdlet ou ainda Active Directory Certificate Services

ansible-playbook -e "win_server=192.168.1.4 ip_class=192.168.1.40/24 gat=192.168.1.1" certificado=/path/to/certificate/public/key.pem certificado_chave=/path/to/certificate/private/key.pem changeNetworkwin.yml -k

# B.	Instalação e configuração
# o	Instalar zabbix-agent 
# o	Criar uma condicional para preencher o parâmetro “Server” com diferentes valores no arquivo de configuração, baseado no terceiro octeto do IP, onde está sendo instalado 
# zabbix-agent 

# Para esta tarefa utilizei playbook que com ele é possivel orquestrar as tarefas em uma sequencia por exemplo:
#instalar repositorio do zabbix

#Instalar zabbix-agent

#Obter o terceiro octeto da faixa de ip

#Condicional de acordo o terceiro octeto obtido configurar o parametro server dentro do /etc/zabbix/zabbix_agentd.conf

#Reiniciar o serviço do zabbix-agent

ansible-playbook -e host=localhost installzabbix.yml -k

# segue anexo o arquivo no git installzabbix.yml


# 2 - Identificação de problemas
# No seguinte cenário, foi executado o playbook RequestCert.yml, cujo retorno está a abaixo. Identifique o erro.
# Execução:

ansible-playbook RequestCert.yml --extra-vars 'domain= PLAINTEXT host_ip=PLAINTEXT ansible_fqdn=PLAINTEXT hostname=PLAINTEXT’ 


#Analisando o erro apresentado, eu validaria os seguintes itens:
#A conexão é mesmo para um servidor linux ? caso seja o erro de conexão deve-se ao executar o modulo winrm (modulo para windows para um servidor Linux "serasa.linux"), ou seja #uma conexao windows para um server linux.
#Se a conexão for para um servidor windows com nome de linux eu verificaria regras de firewal, serviço do winrm e configurações de certificados.









