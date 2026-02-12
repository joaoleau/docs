O AWS X-Ray ajuda desenvolvedores a analisar e depurar aplicações distribuídas em produção, como as baseadas em arquitetura de microsserviços. Ele oferece uma visão completa das requisições enquanto percorrem a aplicação, mostrando um mapa dos componentes subjacentes. Além disso, o X-Ray permite coletar dados entre múltiplas contas AWS, pois o agente pode assumir uma role para publicar dados em uma conta centralizada diferente daquela onde está executando, facilitando o rastreamento e a visualização centralizada.
Ref: [Conhecendo o AWS X-Ray | Service map na AWS](https://www.youtube.com/watch?v=RXxy7EMh7C8)

![[Pasted image 20260211234414.png]]
![[Pasted image 20260211235033.png]]
~={red}Note que há uma agente/daemon!=~


## X-Ray Sampling
Técnica que controla a quantidade de dados de rastreamento coletados e enviados, permitindo ajustes sem alterar o código da aplicação.