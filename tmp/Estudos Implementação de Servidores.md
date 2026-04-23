### Anotações

Aula 2 -

ifconfig - comando pra ver/conf interfaces de redes - útil pra sistemas Unix
Flags:
 - a : Ver todos as interfaces de redes, inclusive as não montadas
 
Montar nova interface

ifconfig < nome da interface > < ip > < netmaks > up
Exemplo:
  ifconfig eth0 192.168.0.1 255.255.255.0 up
Da ping no ip da interface pra ver se ta pegando:
  ping 192.168.0.1


Aula 3 -
/etc/rc.d -> diretorio onde scripts de execução na inicialização ficam
/etc/rc.d/rc.local -> Arquivo onde executa comandos durante inicialização
/etc/rc.d/rc.local -> Só existe no contexto do Slack


Existe em qq Linux:
/etc/hosts
ifconfig
/etc/resolv.conf

Comando netconfig -> Configura já /etc/hosts e /etc/resolv.conf
Conteúdo de /etc/resolv.conf

#NETCONFIG
static IPv4
Dominio: ifsp.edu.br
IPv4: 192.168.0.1/24
Gateway: 192.168.0.1
search ifsp.edu.br  -> dominio configurado no netconfig
nameserver 192.168.0.254 -> servidor DNS configurado no netconfig

Diferença entre "/etc/hosts" e "/etc/resolv.conf":
- "/etc/hosts" : mapeia localmente nomes de host para endereços IP (estático, alta prioridade), funcionando como uma lista de contatos manual.
- "/etc/resolv.conf" :  configura quais servidores DNS externos o sistema consulta para resolver nomes

Arquivo de configuramento de placas de redes /etc/rc.d/rc.inet1.conf
/etc/rc.d/rc.inet1 -> Scripts de placas de redes, serviços gerais de rede, tal como:
/etc/rc.d/rc.inet1 stop -> Stop em todas as placas, ou seja, quando der ifconfig não vao ter montadas
/etc/rc.d/rc.inet1 start -> Inicia/Monta as placas de redes
assim como restart

Comandos pra criar user:

adduser -> Script de criação de usuário usando Slack, proprio script da dsitru
useradd -> Comando para qualquer Linux

Aula 4 -

SSH -> É um shell seguro criptografado, porta 22.
No Windows é Win-putty (cliente shell)

Servidor Shell <- Cliente Shell

Bash é um Shell local.

Não é possivel logar direto como root via ssh. Tem que ser usuario comum dai depois eleva. Ou caso voce altere o arquivo de configuração:
 1- Acesse com vim /etc/ssh/sshd_config
 2- Altere o "PermitRootLogin yes"
 3- Faça restart do daemon: /etc/rc.d/rc.sshd restart

Stop de SSH (start|stop|restart)
/etc/rc.d/rc.sshd stop

Aula 5 -
Servidor FTP - File Transfer Protocol
 - Very Secure FTP (Login Anonimo para todos)
 - Professional FTP

Arquivo /etc/inetd.conf
Inicia o Daemon do InetD:
chmod +x /etc/rc.d/rc.inetd
/etc/rc.d/rc.inetd restart

nmap localhost -> Valida se o serviço FTP subiu na porta (21)
ps aux | grep ftp

Logar no aluno1 e usar o binário do ftp client:
 ftp servidor (ou somente ftp e depois da open <ip/hostname>)

Mandar arquivo:

put < arquivo > < destino >. Exemplo: put ifsp.txt /home/aluno1/ifsp.txt


Arquivo com padrões de portas e serviços
/etc/services

Aula 6 - DHCP

Arquivo /etc/dhcpd.conf

(tempo de concessao)
default-lease-time 600; # 10minutos

max-lease-time 7200;  #maximo de renew 2h

option subnet-mask 255.255.255.0; #mascara de rede
option broadcast-address 255.255.255.255; #broadcast 
option routers 192.168.0.1; #ip do gateway
option domain-name-servers 200.106.80.11, 200.106.1.5; #confgiurando servidores DNS
option domain-name "ifsp.edu.br";
subnet 192.168.0.0 netmask 255.255.255.0 {
range 192.168.0.10 192.168.0.100;
range 192.168.0.150 192.168.0.200;
} #Configura range de IPs disponvieis para alocação

Binario de gerenciamento
dhcpd start eth1  #mostra que o servidor dhcp vai ver interface eth1


client dhcp
Configura com netconfig
> altera hostname pra client
> domain ifsp.edu.br
> configura DHCP

reboot

Altera o /etc/rc.d/rc.local removendo a inicialização do eth1 na mão

reboot

ifconfig e veja se o IP veio do range

Configura no /etc/rc.d/rc.inet1.conf e muda pra yes no USE_DHCP[1]  cujo é a eth1

### Atividade FTP:

1- Alterar porta FTP (Professional). É possível? Como?

vim /etc/inetd.conf 
  -> Editei a linha do Professional retirando o comentario e habilitando o serviço de proftp

vim /etc/proftpd.conf
   -> Editei a linha "Port" pra 2121

vim /etc/services
   -> Alterei a porta ftp 21 tcp para a minha 2121

/etc/rc.d/rc.inetd restart
   -> Restart no serviço do InetD

Por fim rodei "nmap servidor" e validei que estava aberta a 2121.

Acessando com o aluno1:
ftp servidor 2121

Com aluno1 logado
put ifsp.txt /home/aluno1/envio/arquivo_passando_por_2121.txt

Transfer Complete!

Por fim rodei cat no arquivo "cat /home/aluno1/envio/arquivo_passando_por_2121.txt". Estava "Ok"!

É possível!!

2- Habilitar o acesso root no FTP. É possível? Como?

É possível!

vim /etc/ftpusers
  -> Retirei o "root" de usuários não permitidos

vim /etc/proftpd.conf
  -> Adcionei "RootLogin on" no arquivo. 

/etc/rc.d/rc.inetd restart
   -> Restart no serviço do InetD

3- Listar nomes de clientes FTP para o Windows.

FileZilla
WinSCP
Cyberduck
CuteFTP
Free FTP
WS_FTP Professional
SmartFTP

Referência: https://www.hostinger.com/br/tutoriais/cliente-ftp

### Exercicios Praticos

Implementação de Servidores

DHCP
-Subir um servidor e dois clientes (clonar o cliente já existente)
-Um dos clientes receberá um IP fixo baseado em seu MAC addr.
-O outro cliente não receberá IP pois só recebem IPs os hosts com MAC cadastrado

Usuários
-Crie um usuário chamado 'bsi' no servidor
-Esse usuário deve utiliuzar o diretório '/home/bsi' como seu diretório padrão
-O shell para esse usuário deverá ser o Korn Shell (ksh)
-Esse usuário não tem senha para se logar no sistema

FTP
-Verifique se consegue logar com o usuário 'bsi'
-Mude a porta do FTP para outra que não seja a padrão

SSH
-Habilite o login com o usuário 'root' no servidor
-Tente também logar com o usuário 'bsi'
-Mude a porta do SSH para outra que não seja a padrão

#### **1 - Crie um usuário e explique o processo e comandos:**


Para criar um usuário basta executar o comando:
useradd --create-home --home-dir < path do home > --shell < path do bin do shell > < nome do user >

Para adicionar senha basta:
passwd < nome do user >

Para que o usuário não tenha senha:
passwd --delete < nome do user >

A criação do usuário pode ser validada no arquivo: /etc/passwd

#### **2 - Criar um servidor FTP com porta diferente da tradicional:**

Editar o arquivo abaixo e descomentar a linha de configuração do PROFTP
/etc/inetd.conf

*Obs: Caso não encontre a linha o conteúdo é o seguinte: *
ftp stream tcp nowait root /usr/sbin/tcpd proftpd

/etc/proftpd.conf
Nesse arquivo voce pode alterar os campos:

Port < porta de sua escolha >

Para que o servidor rode de fato nessa porta edite o service:
/etc/services : Procure ftp tcp e mude a porta para sua escolha

Para que de tudo certo faça restart do inetd: /etc/rc.d/rc.inetd restart

Por fim rode "nmap < hostname >" para validar que o serviço esta escutando na porta correta


////

**Na visão do cliente:**

Para transferir arquivo para o servidor FTP:

ftp < servidor > < porta >
put
local-file < path do arquivo que sera transferido >
remote-file < path do arquivo transferido para o servidor >


Há também o comando de "get".


#### 3 - Criar servidor SSH e alterar porta:

Alterar /etc/services procurar por ssh e alterar a porta 22 para uma de sua escolha

Iniciar o serviço do SSHD (SSH Daemon): /etc/rc.d/rc.sshd start

Pode alterar as configurações dele em /etc/ssh/sshd_config como por exemplo:

PermitRootLogin yes
Port < nova porta >


/etc/rc.d/rc.sshd restart


////
**Na visão do cliente:**
ssh < ip / hostname > -p < porta >

#### 4 - Configurar servidor DHCP



Primeiro editar o arquivo "/etc/dhcpd.conf"

option routers < ip do gateway >;
option subnet-mask < mascara  / 255.255.255.0 >;
option broadcast-address 255.255.255.255 ;
option domain ifsp.edu.br;
options domain-name-servers < ip do servidor dns 1 >, < ip do servidor dns 2 >;

subnet < ip da subnet  / 192.168.0.0 > netmask < mascara / 255.255.255.0 > {
range 192.168.0.10 192.168.0.15;
}

host < host > {
hardware ethernet < MAC > 
fixed-address < ip fixo >
}

Pode tambem adiconar um para bloquear clients q nao estajam permitios (nao existe host configurado para tal):

deny unknown-clients; 

dhcpd start < interface / eth0 >

Pode ate dar um dhcpd start eht0 no /etc/rc.d/rc.local ja para iniciar o serviço dhcp


////
Na visao do cliente

Acessar o netconfig
ir em Use DHCP
Pode dar um Skip no HOSTNAME DHCP
Por fim ir em /etc/rc.d/rc.inet1.conf 

Editar USE_DHCP [ < interfcae de rede > ] : yes
Fazer reboot

#### Outra versao

Prática para Prova - 15/04/2026

**
Primeiro subi o servidor com a imagem do Slack crua (disponivel no Moodle)
root 123456

Configurei a rede básica, IP, Gateway, DNS etc..
Executei "netconfig"
 - static IP
 - Hostname: servidor
 - Dominio: ifsp.edu.br
 - Ipv4: 192.168.0.1/24
 - Ipv6 Skip
 - Gateway: 192.168.0.1
 - Nameserver (Servidor DNS): 192.168.0.254
**

Usuário:
- Criar 1 usuário "bsi" no servidor
- Esse usuário deve utilizar o dir /home/bsi como home e o koin shell (ksh) como shell padrão
- Esse usuário não tem senha pra logar

**
Para criar o usuário executei: 
  "useradd --home-dir /home/bsi --create-home --shell /bin/ksh bsi"
  Note-se:
   - Não exige definir senha, caso fosse preciso teria que executar: "sudo passwd bsi"

Para tirar a senha tem que digitar "passwd --delete bsi"
**

FTP:
- Verificar se consegue logar com o usuário "bsi"
- Mude a porta do FTP para outro que não seja padrão

vim /etc/inetd.conf 
  -> Editei a linha do Professional retirando o comentario e habilitando o serviço de proftp

vim /etc/proftpd.conf
   -> Editei a linha "Port" pra 2121

vim /etc/services
   -> Alterei a porta ftp 21 tcp para a minha 2121

/etc/rc.d/rc.inetd restart
   -> Restart no serviço do InetD

Por fim rodei "nmap servidor" e validei que estava aberta a 2121.

Acessando com o aluno1:
ftp servidor 2121

Com aluno1 logado
put ifsp.txt /home/aluno1/envio/arquivo_passando_por_2121.txt

Transfer Complete!

Por fim rodei cat no arquivo "cat /home/aluno1/envio/arquivo_passando_por_2121.txt". Estava "Ok"!
**

**
SSH:
- Habilite o login como usuário "root" no servidor
- Tente também logar com o usuário "bsi
- Mude a porta do SSH para outra que não seja padrão

**
 1- Acesse com vim /etc/ssh/sshd_config
 2- Altere o "PermitRootLogin yes"
 3- Faça restart do daemon: /etc/rc.d/rc.sshd restart
**

DHCP:
- Subir 1 Servidor e 2 Clientes (clonar o cliente já existente)
- Um dos clientes receberá um IP fixo baseado em seu MAC
- O outro cliente não recebrá IP pois só receberão IP os MAC cadastrados


**
Configurar no Servidor DHCP:

/etc/dhcpd.conf

option subnet-mask 255.255.255.0; #mascara de rede
option broadcast-address 255.255.255.255; #broadcast 
option routers 192.168.0.1; #ip do gateway
option domain-name-servers 200.106.80.11, 200.106.1.5; #confgiurando servidores DNS
option domain-name "ifsp.edu.br";
subnet 192.168.0.0 netmask 255.255.255.0 {
range 192.168.0.10 192.168.0.100;
range 192.168.0.150 192.168.0.200;
}

host client1 {
hardware ethernet < mac >;
fixed-address 192.168.19;
}

deny unknown-clients;


**






