Ferramenta PASS (Platform As Service), provisiona automaticamente banco de dados, security, URL pública, Load Balancer... Simplesmente tudo pronto só pra receber código. 
- Ideal apps WEB.
- Por de baixo dos panos vai criar templates Cloudformation
- Bastante usado pra MVP, por ser muito rápido Beanstalk é útil para esse caso.
- Pode ser exportado para template Cloudformation, para verificar detalhes de cada recurso criado.
- `.ebextensions` pasta que pode conter personalizações do seu app, tal como detalhes de infra, destino de logs (envio de logs para o S#), instalação de pacotes, assim como executar comandos pós deployment (exemplo executar determinada lambda depois do deployment). ~={red}De forma análoga é um UserData, ou seja, dado uma configuração de RDS nessa pasta, cada implantação o banco será recriado.=~
- AWS CLI Commands: `eb init` (inicializa projeto) , `eb create` (cria ambiente), `eb deploy` (faz deploy), `eb terminate` (apaga tudo), `eb open` (abre no navegador)
## Beanstalk Cleanup
Ferramenta para deleção dos recursos criados pelo Elastic Beanstalk
- Encerra ambiente com segurança
- AWS CLI: `eb terminate` 

## Lifecycle Policy 
Útil para controlar versões do Elastic Beanstalk, no final são salvos no S3. Contudo, pode controlar a quantidade de versões salvas, histórico.

## Deployment Strategies
- ~={green}All-at-once:=~ Executa a implantação local em todas as instâncias **(RISCO ALTO)**
	-  Implante a nova versão em todas as instâncias simultaneamente. 
	- Todas as instâncias em seu ambiente ficam fora de serviço por um curto período durante a implantação.
	- All at Once deployments are simple and fast, however, it would lead to downtime and the rollback would take time in case of any issues.
	
- ~={cyan}Rolling:=~ Divide as instância em lotes e implanta em um lote por vez. **(RISCO BAIXO)**
	- During a rolling deployment, part of the instances serves requests with the old version of the application, while instances in completed batches serve other requests with the new version.
	- If a batch of instances does not become healthy within the command timeout, ~={red}the deployment fails=~.
	- ~={red}If a deployment fails after one or more batches are completed successfully, the completed batches run the new version of the application while any pending batches continue to run the old version.=~
	- ~={red}If the instances are terminated from the failed deployment, Elastic Beanstalk replaces them with instances running the application version from the most recent successful deployment.=~

- ~={blue}Rolling with additional batch:=~ Divide as implantações em lotes, mas o primeiro lote cria novas EC2s em vez de implantar nas instâncias existentes. **(RISCO MUITO BAIXO)**
	- Rolling with additional batch deployment does not impact the capacity and ensures full capacity during the deployment process.
	
- Immutable: Se voce precisa fazer deploy com uma nova instância ao invés de usar a existente. **(RISCO MÍNIMO)**
	- All-at-once e Rolling atualizam a instância existente. Já Immutable não.
	- Immutable deployments can prevent issues caused by partially completed rolling deployments. If the new instances don’t pass health checks, Elastic Beanstalk terminates them, leaving the original instances untouched.
	- Immutable updates are performed by launching a second Auto Scaling group is launched in the environment and the new version serves traffic alongside the old version until the new instances pass health checks.
	
- ~={purple}Traffic splitting=~: Prederminado. Se as instância permanecem íntegras, encaminhe todo o trafego para novas instâncias e encerre as antigas. 

- ~={pink}Blue Green Deployment:=~ Blue Green approach is suitable for deployments that depend on incompatible resource configuration changes or a new version that can’t run alongside the old version. **(RISCO QUASE SEMPRE ZERO)**
	- Elastic Beanstalk enables the Blue Green deployment through the **Swap Environment URLs** feature.
	- Blue Green deployment provides an almost zero downtime solution, where a new version is deployed to a separate environment, and then CNAMEs of the two environments are swapped to redirect traffic to the new version.

### Perguntas:

> 1- Você criou uma aplicação Java que utiliza o Amazon RDS como seu principal armazenamento de dados e o ElastiCache para armazenamento de sessões de usuário. A aplicação precisa ser implantada usando o Elastic Beanstalk, e toda nova implantação deve permitir que os servidores da aplicação reutilizem o banco de dados RDS. Por outro lado, os dados de sessão do usuário armazenados no ElastiCache podem ser recriados a cada implantação. Qual das seguintes configurações permitirá alcançar esse objetivo? (Selecione duas.)
		~={green}Banco de dados RDS definido externamente e referenciado por variáveis de ambiente.=~
		~={green}Banco de dados ElastiCache definido externamente e referenciado por variáveis de ambiente=~