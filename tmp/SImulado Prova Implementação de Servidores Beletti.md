
### 1.

Explique o fluxo completo do DHCP desde o momento que o cliente liga.

Dado que existe um servidor DHCP naquela subnet onde o cliente esta, ou seja, isso significa que o servidor esta "escutando" no endereço de broadcast da subnet solicitações de pacotes DHCP, o esquema seria o seguinte:

Cliente esta iniciando -> Solicita IP/NETMASK/GATEWAY/DNS para o endereço broadcast da subnet -> Recebe certas informações e obtém pra si (caso, não receba o IP, ele adotará o APIPA).
- **DHCPDISCOVER** (cliente → broadcast)
- **DHCPOFFER** (servidor → cliente)
- **DHCPREQUEST** (cliente → servidor)
- **DHCPACK** (servidor → cliente)
### 2.

Qual a diferença entre:

- `/etc/hosts`
- `/etc/resolv.conf`

O arquivo "/etc/hosts" é como se fosse uma agenda, onde pode informar "apelidos alfanumericos" para endereços IP, de forma analoga a servidor DNS, porém totalmente local, isso facilita chamadas/conexoes com demais servidores e não precisa decorar IP.

Ja o "/etc/resolv.conf" diz respeito configurações de dominios e apontamentos pra servidores DNS. Isso significa que ao pesquisar "google.com" esse nome será buscado nos servidores DNS configurados.

### 3.

Por que um cliente pode não receber IP do DHCP?

Pode ser que o servidor DHCP daquela subnet existe restrições, por exemplo, por endereço MAC. Dessa forma, a maquina que não receberá IP adotará o APIPA.

Resposta ideal teria 3 causas:

- DHCP não está rodando
- interface errada
- `deny unknown-clients`
- problema de rede
- conflito de IP

### 4.

Qual a função da opção abaixo no DHCP:

option routers 192.168.0.1;

Essa opção serve para configurar o Gateway nos pacotes DHCP do servidor DHCP enviados aos clients,

### 5.

Qual a diferença entre:

- `range`
- `fixed-address`

Range diz respeito a um intervalo de IPs possiveis para alocação, ja fixed são IP estático, ous eja, fixo.

### 6.

O DHCP utiliza DNS para encontrar o servidor? Explique.

Não utiliza, o DHCP pode utiizar o dominio para encontrar servidores na mesma rede.

> "pode utilizar o dominio para encontrar servidores"

👉 ❌ **Isso está errado**

✔ Correto:

- DHCP **NÃO usa DNS**
- DHCP usa **broadcast (camada 2/3)**

# 🧠 🔹 PARTE 2 — Prática

### 7.

Crie um usuário `bsi` com:

- home `/home/bsi`
- shell `/bin/ksh`
- sem senha

useradd --create-home --home-dir /home/bsi --shell /bin/ksh bsi
passwd --delete bsi
### 8.

Configure SSH para:

- permitir root login
- rodar na porta 2222

Editar /etc/ssh/sshd_config 
Alterar o campo Port e PermitRootLogin para yes.
Reiniciar serviço do SSHD /etc/rc.d/rc.sshd restart
### 9.

Configure DHCP para:

- rede 192.168.1.0/24
- range 100–150
- gateway 192.168.1.1
- DNS 8.8.8.8
- cliente com MAC `00:11:22:33:44:55` → IP 192.168.1.10
- bloquear desconhecidos

option subnet-mask 255.255.255.0
option router 192.168.1.1
option domain-name-servers 8.8.8.8;
subnet 192.168.1.0 netmask 255.255.255.0 {
range 192.168.1.100 192.168.1.150;
}
host cliente {
hardware ethernet `00:11:22:33:44:55`;
fixed-address 192.168.1.10;
}
deny unknown-clients;

### 10.

Configure um cliente manualmente (SEM netconfig) para usar DHCP

Nao tenho certeza mas acho que sera:

Habilitar USE_DHCP[0]: yes em "/etc/rc.d/rc.inet1.conf"
Adicionar em /etc/resolv.conf

search ifsp.edu.br (dominio)

### ✔ Parcialmente correto

👉 O essencial era:

/etc/rc.d/rc.inet1.conf

USE_DHCP[0]="yes"

Depois:

/etc/rc.d/rc.inet1 restart

---

### ❗ Erro:

- Não precisa mexer no `/etc/resolv.conf` manualmente (DHCP faz isso)
