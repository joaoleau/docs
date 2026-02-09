
VPC - Virtual Private Cloud
Ambiente que possibilita a criação de redes virtuais privadas, semelhante a uma rede LAN tradicional.
- Por padrão toda conta já possui uma VPC, para cada região, "iguais".
- Não há custo, exceto alguns componentes tal como o NAT GW.

![[Pasted image 20260201221810.png]]
Observações:
 - CIDR 10.0.0.0/16 isso significa que 16 bits serão para endereçamento e os outros hosts, ou seja: 10.0.255.255
- Repare que além de definir manual o bloco de IP, a AWS fornece a opção de IPAM, essa funcionalidade é uma alocação dinâmica, entretanto, paga.
- Tenancy:![[Pasted image 20260201222104.png]]Dedicated: 
	- Select **Dedicated** to ensure that instances launched in this VPC are run on dedicated tenancy instances regardless of the tenancy attribute specified at launch.

Subnet
![[Pasted image 20260201222737.png]]
- Dado minha VPC com CIDR 10.0.0.0/16, temos:
	- subnet-a : 10.0.0.0/20 -> 10.0.15.255
	- subnet-b: 10.0.16.0/20 -> 10.0.31.255
	- subnet-c: 10.0.32.0/20 -> 10.0.47.255
	- subnet-d: 10.0.48.0/20 -> 10.0.63.255
