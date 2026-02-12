
- A AWS Lambda aloca capacidade de CPU proporcionalmente à quantidade de memória configurada para a função. Portanto, se é desejo aumentar capacidade de CPU, deverá ser feito ajuste na de memória.~={red} Não tem como ajustar apenas configuração de CPU na Lambda.=~

## Lambda Authorizer
Função Lambda personalizada que autentica e autoriza requisições ao API Gateway, validando tokens externos. Camada de autorização para chamadas do API Gateway.

Você cria para controlar o acesso à sua API. Ele permite implementar estratégias de autenticação com tokens bearer, como OAuth ou SAML, possibilitando integrar mecanismos de autorização de terceiros antes do acesso à API.


## Alias

![[Pasted image 20260211232941.png]]
![[Pasted image 20260211233127.png]]