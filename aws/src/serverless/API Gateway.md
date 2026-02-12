Ref: [Versionamentos de APIs e Lambda Aliases](https://www.youtube.com/watch?v=IUD_XHkkdQw)
## Usage Plans
Definem quem pode acessar quais APIs, em quais estágios e métodos, além de controlar a quantidade e a velocidade de acesso. 

**Eles usam chaves de API para identificar clientes e aplicar limites de uso, permitindo oferecer APIs públicas como produtos com controle de consumo.**

 Com **Usage Plans**:
	- Cada cliente recebe uma chave de API com limites específicos.
    - É possível oferecer diferentes planos de acesso: gratuito com limite baixo ou pago com maior capacidade.
    - Monitora-se consumo e evita abusos, garantindo escalabilidade e qualidade do serviço.
Ref:
- [AWS API Gateway with API Key / Usage Plan (LATEST)](https://www.youtube.com/watch?v=AxhbAx4PTow)

## Stages
Útil pra criar uma outra referência lógica da API, são identificados por API ID, Stage Name e são incluidos na URL. Ajuda para separar principalmente configurações entre ambientes, observe que o esperado são os mesmos methods/endpoints (mesma versão, porém ambientes separados). Servem:
- Configurações únicas de ratelimit, cache, configuração de log ou anexar uma versão canary, para cada Stage.

~={red}Se o seu objetivo é criar novos methods/endpoint para a API atual, o recomendado é que se crie uma nova API e não Stage.=~
