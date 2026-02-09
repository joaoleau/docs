Elastic Load Balancer pode distribuir cargas entre zonas de disponibilidade pertencentes a sua mesma região. Mas não para múltiplas regiões.

Contato que esse modelo esteja ativado "cross-zone load balancing", a porcentagem de distribuição de carga é a mesma para cada target/instância, ou seja, supondo:
~={purple}	Uma organização possui suas instâncias EC2 hospedadas em duas Zonas de Disponibilidade (AZs). A AZ1 possui 2 instâncias e a AZ2 possui 8 instâncias. O Elastic Load Balancer que gerencia as instâncias nas duas AZs tem o balanceamento de carga entre zonas (cross-zone load balancing) habilitado em sua configuração. Qual porcentagem do tráfego cada instância na AZ1 receberá?=~
R: ~={green}Correta. Com o balanceamento de carga entre zonas habilitado, o tráfego é distribuído igualmente entre todas as instâncias registradas, independentemente da AZ em que estejam. Como há 10 instâncias no total (2 na AZ1 e 8 na AZ2), cada instância recebe 10% do tráfego=~

Benefícios de se usar o ELB:
- Construir um sistema altamente disponível
- Separar o tráfego público do tráfego privado:
		Os nós de um load balancer com acesso à internet possuem endereços IP públicos, mas o balanceador encaminha as requisições para os alvos (como instâncias EC2) usando seus IPs privados, permitindo que os alvos não precisem de IPs públicos para receber tráfego externo.