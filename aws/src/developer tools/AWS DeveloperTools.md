### AWS CodeCommit
É um serviço de controle totalmente gerenciado que hospeda repositórios Git privados, diretamente na nuvem. Contudo, pode criar repositórios e fazer controle de versão.
-  **Armazene seu código com segurança**. **CodeCommit os repositórios são criptografados tanto em repouso quanto em trânsito.**: 
	- CodeCommit repositório em um novo Região da AWS em sua conta da Amazon Web Services, se você não especificar uma chave gerenciada pelo cliente, **CodeCommit cria uma Chave gerenciada pela AWS (a aws/codecommit chave) na mesma Região da AWS em AWS Key Management Service (AWS KMS). Essa aws/codecommit chave é usada somente por CodeCommit.**
	- CodeCommit usa duas abordagens diferentes para criptografar dados. Objetos Git individuais com menos de 6 MB são criptografados usando AES-GCM-256, que fornece validação de integridade de dados. Objetos entre 6 MB e o máximo de 2 GB para um único blob são criptografados usando AES CBC-256. CodeCommit sempre valida o contexto de criptografia.
-  CodeCommit suporta comandos Git, bem como seus próprios AWS CLI comandos e. APIs.
- **Não há limites arbitrários no tamanho dos arquivos, tipos de arquivo e tamanho do repositório.**
- **Os repositórios são específicos para um Região da AWS.**
![[Pasted image 20260410220201.png]]

> Quando você cria um repositório no CodeCommit, dois endpoints são gerados: um para conexões HTTPS e outro para conexões SSH. Ambos fornecem conexões seguras em uma rede. Os usuários podem usar um dos dois protocolos. Ambos os endpoints se mantêm ativos, independentemente do protocolo que você recomenda aos usuários. Antes de compartilhar o repositório com outros, é necessário **criar políticas do IAM que permitem o acesso de outros usuários ao seu repositório.**

![[Pasted image 20260425104011.png]]
##### Use Cases:
- Arquivos grandes que são alterados com frequência recomenda-se a não usar o CodeCommit e sim o S3.
- Repositórios baseados em Git e afim de facilitar já esta na AWS e possui integração nativa com outros serviços

##### Credenciais Git 
**A maneira mais fácil de configurar o CodeCommit é configurar as credenciais do Git HTTPS para o AWS CodeCommit:** Com conexões HTTPS e credenciais do Git, você gera um nome de usuário e senha estáticos no IAM. Essas credenciais são usadas com o Git e qualquer outra ferramenta de terceiros com suporte para a autenticação usando o nome de usuário e a senha do Git.

**Você pode usar o protocolo SSH em vez de HTTPS para se conectar ao repositório do CodeCommit**. Com conexões SSH, você cria arquivos de chave pública e privada em sua máquina local que o Git e o CodeCommit usam para a autenticação SSH. A chave pública deve ser associada ao seu usuário do IAM. e a chave privada é armazenada na sua máquina local.

**Você pode acessar repositórios do CodeCommit sem a necessidade de um usuário do IAM (with git credentials), ou seja, se conectar a repositórios usando acesso federado e credenciais temporárias.**:
- Instale e use git-remote-codecommit (recomendado). 
- Instale e use o assistente de credenciais incluído na AWS CLI.

##### Migrar para CodeCommit
**A maneira mais fácil de configurar o CodeCommit é configurar as credenciais do Git HTTPS para o AWS CodeCommit:** Com conexões HTTPS e credenciais do Git, você gera um nome de usuário e senha estáticos no IAM. Essas credenciais são usadas com o Git e qualquer outra ferramenta de terceiros com suporte para a autenticação usando o nome de usuário e a senha do Git.


![[Pasted image 20260425103736.png]]

##### Integrações de produtos e serviços ao AWS CodeCommit

| AWS Amplify              | O Amplify facilita a criação, configura ção e implementação de aplicativos móveis escaláveis desenvolvidos pela AWS. Além disso, o Amplify automatiza o processo de lançamento de aplicativos para front-ends e back-ends, o que permite acelerar a entrega de recursos.<br><br><br>**Você pode conectar seu repositório do CodeCommit ao console do Amplify.** Depois que você autorizar o console do Amplify, o Amplify obterá um token de acesso no provedor do repositório, mas não armazenar á o token nos servidores da AWS. O Amplify acessa seu repositório usando chaves de implantação instaladas somente em um repositório específico<br><br> |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CloudWatch Events        | Você pode configurar o CloudWatch Events para monitorar repositórios do CodeCommi t e responder a eventos do repositório direcionando fluxos, funções, tarefas ou outros processos em outros serviços da AWS, como o Amazon Simple Queue Service, o Amazon Kinesis, o AWS Lambda e muitos outros.                                                                                                                                                                                                                                                                                                                                                        |
| Amazon CodeGuru Reviewer | O Amazon CodeGuru Reviewer é um serviço automatizado de revisão de código que usa análise de programas e machine learning para detectar problemas comuns e recomenda r correções em seu código Java ou Python.<br><br>Você pode associar repositórios na sua conta da Amazon Web Services com o CodeGuru Reviewer. **Ao fazer isso, o CodeGuru Reviewer cria uma função vinculada ao serviço que permite que o CodeGuru Reviewer analise códigos em todas as solicitações pull criadas depois que a associação é feita.**<br><br>                                                                                                                        |
| Lambda                   | Você pode criar um gatilho para um CodeCommit repositório para que os eventos no repositório invoquem uma função Lambda.<br><br>**CONTUDO, NÃO PRECISA PASSAR POR UM CLOUDWATCH EVENTS, É UMA INTEGRAÇÃO DIRETA!**<br><br><br>![[Pasted image 20260425105329.png]]                                                                                                                                                                                                                                                                                                                                                                                       |
| Amazon SNS               | **Você pode configurar os acionadores para repositórios do CodeCommi t que enviam notificações do Amazon SNS em resposta a eventos do repositório. Você também pode usar notificações do Amazon SNS para integrar com outros serviços da AWS. Por exemplo, você pode usar uma notificação do Amazon SNS para enviar mensagens para uma fila do Amazon Simple Queue Service**                                                                                                                                                                                                                                                                             |



> AWS CodeStar Notificações: Você pode usar o recurso de notificações para notificar rapidamente os usuários sobre eventos nos repositórios, projetos de compilação, aplicativos de implantação e pipelines que são mais importantes para seu trabalho. Um gerenciador de notificações ajuda os usuários a saber quais eventos ocorrem em repositórios, compilações, implantações ou pipelines para que possam agir rapidamente, como aprovar alterações ou corrigir erros


#### Como é CodeCommit diferente do controle de versão de arquivos no Amazon S3?
CodeCommit é otimizado para o desenvolvimento de software em equipe. Ele gerencia lotes de alterações em vários arquivos, que podem ocorrer paralelamente às alterações feitas por outros desenvolvedores. O versionamento do Amazon S3 é compatível com a recuperação de versões anteriores de arquivos, mas não se concentra nos recursos colaborativos de rastreamento de arquivos que as equipes de desenvolvimento de software precisam.

### AWS CodeBuild
É um serviço de compilação totalmente gerenciado que compila o código-fonte, **executa testes em unidades e produz artefatos prontos para implantação**. **Ele fornece ambientes de compilação com pacotes predefinidos para linguagens populares de programação e ferramentas de compilação, como Apache Maven, Gradle, entre outras.** 
- Você também pode personalizar ambientes de compilação CodeBuild para usar suas próprias ferramentas de compilação.
- **Sob demanda** — CodeBuild escala sob demanda para atender às suas necessidades de construção. Você paga somente pela quantidade de minutos de compilação que consumir. **(EC2, Lambda e Reserved Capacity)**
- **Pronto para uso** — CodeBuild fornece ambientes de construção pré-configurados para as linguagens de programação mais populares. Tudo o que você precisa fazer é apontar para o seu script de compilação para iniciar sua primeira compilação.

> O CodeBuild oferece modos de computação AWS Lambda e do EC2. O EC2 oferece flexibilidade otimizada durante a compilação e o AWS Lambda oferece velocidades de inicialização otimizadas. O AWS Lambda é compatível com compilações mais rápidas devido a uma menor latência de inicialização. O AWS Lambda também é escalado automaticamente, para que as compilações não fiquem esperando na fila para serem executadas.
> **O ambiente CodeBuild sandbox fornece uma sessão de depuração interativ**a em um ambiente seguro e isolado. Você pode interagir com o ambiente diretamente por meio do Console de gerenciamento da AWS or AWS CLI, executar comandos e validar seu processo de criação passo a passo. Ele usa um modelo econômico de cobrança por segundo e oferece suporte à mesma integração nativa com provedores e AWS serviços de origem que seu ambiente de construção. Você também pode se conectar a um ambiente sandbox usando clientes SSH ou de seus ambientes de desenvolvimento integrados IDEs.
> **AWS O Systems Manager Session Manager permite acesso direto às compilações em execução em seu ambiente de execução real. Essa abordagem permite se conectar a contêineres de compilação ativos e inspecionar o processo de compilação em tempo real.** Você pode examinar o sistema de arquivos, monitorar os processos em execução e solucionar problemas à medida que eles ocorrem.

![[Pasted image 20260410220537.png]]
Como mostra o diagrama a seguir, você pode adicionar CodeBuild como uma ação de construção ou teste ao estágio de construção ou teste de um pipeline em **AWS CodePipeline**. AWS CodePipeline é um serviço de entrega contínua que você pode usar para modelar, visualizar e automatizar as etapas necessárias para liberar seu código. Isso inclui a compilação de seu código. Um _pipeline_ é uma construção de fluxo de trabalho que descreve como as alterações de código atravessam um processo de lançamento.

**Voce pode configurar o CodeBuild pra iniciar build apartir de push numa branch do GitHub e gerar artefato estático para o S3. Pipeline de site estático! Porém os artefatos não devem ser criptografados.** 
##### CodeBuild e Docker

O CodeBuild também foi projetado para buildar imagens Docker, realziar comandos tradicionais Docker e levar para o ECR.

> Por padrão, o daemon do Docker está habilitado para compilações não VPC. Se você quiser usar contêineres do Docker para compilações da VPC, consulte Privilégio de tempo de execução e funcionalidades do Linux no site do Docker Docs e ative o modo privilegiado. Além disso, o Windows não é compatível com o modo privilegiado

![[Pasted image 20260425112148.png]]



> **Onde o código-fonte é armazenado?** O CodeBuild no momento é compatível com a compilação pelos provedores de repositórios de código-fonte a seguir. O código-fonte deve conter um arquivo de especificação de compilação (buildspec). _buildspec_ é uma coleção de comandos de compilação e configurações relacionadas, no formato YAML, que o CodeBuild usa para executar uma compilação. É possível declarar um **buildspec** em uma definição de projeto de compilação.

![[Pasted image 20260410221125.png]]

##### Nome do arquivo buildspec e local de armazenamento

**Se você incluir uma especificação de compilação como parte do código-fonte, por padrão, o arquivo buildspec deverá se chamar buildspec.yml e ser colocado na raiz do diretório de origem.**

É possível substituir o nome e o local do arquivo buildspec padrão. Por exemplo, você pode: 
- **Use um arquivo buildspec diferente para compilações diferentes no mesmo repositório, como buildspec_debug.yml e buildspec_release.yml.**
- **Armazene um arquivo buildspec em um local que não seja a raiz de seu diretório de origem, como config/buildspec.yml, ou em um bucket do S3. O bucket do S3 deve estar na mesma AWS região do seu projeto de compilação. Especifique o arquivo buildspec usando seu ARN (por exemplo, arn:aws:s3:::/buildspec.yml). É possível especificar somente uma buildspec para um projeto de compilação, independentemente do nome do arquivo buildspec**

Para substituir o nome do arquivo buildspec padrão, o local ou ambos, faça o seguinte: 
- Execute o update-project comando AWS CLI create-project or, definindo o buildspec valor do caminho para o arquivo buildspec alternativo em relação ao valor da variável de ambiente integrada. CODEBUILD_SRC_DIR Você também pode fazer o equivalente com a create project operação no AWS SDKs.
- Em um AWS CloudFormation modelo, defina a BuildSpec propriedade de Source em um recurso do tipo AWS::CodeBuild::Project para o caminho para o arquivo buildspec alternativo em relação ao valor da variável de ambiente integrada.

##### Sintaxe de buildspec
```
version: 0.2  # Versão do schema do buildspec (atualmente 0.2 é o padrão mais usado)

env:
  variables:
    APP_ENV: "production"   # Variáveis de ambiente simples
  parameter-store:
    DB_PASSWORD: "/myapp/db/password"  # Busca do AWS SSM Parameter Store
  secrets-manager:
    API_KEY: "my/secret:apiKey"  # Busca do AWS Secrets Manager

phases:
  install:
    runtime-versions:
      nodejs: 18   # Define runtime (Node, Python, Java, etc.)
    commands:
      - echo "Instalando dependências..."
      - npm install

  pre_build:
    commands:
      - echo "Executando antes do build"
      - npm run lint

  build:
    commands:
      - echo "Buildando aplicação"
      - npm run build

  post_build:
    commands:
      - echo "Executando pós-build"
      - npm test

artifacts:
  files:
    - '**/*'  # Quais arquivos serão exportados como artefato
  base-directory: dist  # Diretório base dos artefatos (ex: build final)
  discard-paths: no     # Mantém estrutura de pastas

cache:
  paths:
    - node_modules/**/*  # Cache para acelerar builds futuros
```


##### Compilações em Cache
Você pode economizar tempo quando seu projeto é compilado usando um cache. Um cache pode armazenar partes reutilizáveis do seu ambiente de build e usá-las em vários builds. **O projeto de compilação pode usar um dos dois tipos de armazenamento em cache: Amazon S3 ou local**. Se usar um cache local, você deverá escolher um ou mais dos três modos de cache: cache de origem, cache de camada do Docker e cache personalizado.
	**O armazenamento em cache do Amazon S3 armazena o cache em um bucket do Amazon S3 que está disponível em vários hosts de compilação. Esta é uma boa opção para artefatos de compilação pequenos a intermediários que são mais caras para criar do que para baixar.**


É possível usar o campo da API cacheNamespace na seção cache para compartilhar um cache entre vários projetos. Esse campo define o escopo do cache. Para compartilhar um cache, faça o seguinte:
- Use o mesmo cache Namespace.
- Especifique o mesmo cache key.
- Defina caminhos de cache idênticos. 
- Use os mesmos buckets do Amazon S3 e pathPrefix, se definido.
![[Pasted image 20260425113439.png]]
###### Cache local
**O armazenamento em cache local armazena um cache localmente em um host de compilação que está disponível somente para esse host de compilação**. Esta é uma boa opção para artefatos de compilação grandes a intermediários porque o cache fica imediatamente disponível no host de compilação. Essa não é a melhor opção se suas compilações são pouco frequentes. Isso significa que o desempenho da compilação não é afetado pelo tempo de transferência na rede. O cache local fica **no host do build (máquina do CodeBuild)**:
 - `LOCAL_SOURCE_CACHE`
- `LOCAL_DOCKER_LAYER_CACHE`
- `LOCAL_CUSTOM_CACHE`
### AWS CodeDeploy
É um serviço de implantação totalmente gerenciado que **automatiza implantações de software em serviços de computação, como Amazon EC2 AWS Lambda, ECS e seus servidores locais.** Ele pode ajudá-lo a liberar rapidamente novos recursos, evitar tempo de inatividade durante a implantação de aplicativos e lidar com a complexidade da atualização de seus aplicativos.
CodeDeploy pode implantar conteúdo de aplicativo executado em um servidor e armazenado em buckets, GitHub repositórios ou repositórios Bitbucket do Amazon S3. CodeDeploy também pode implantar uma função Lambda sem servidor. Você não precisa fazer alterações no seu código existente antes de poder usar o CodeDeploy.

CodeDeploy torna mais fácil para você:
- Lançar novos recursos rapidamente.
- Atualize as versões da AWS Lambda função.
- Evite tempo de inatividade durante a implantação do aplicativo.
- Processar a complexidade da atualização de seus aplicativos sem muitos dos riscos associados a implantações manuais propensas a erro.

> Se seu aplicativo usa a plataforma de computação EC2/On-Premises, CodeDeploy ajuda a maximizar a disponibilidade do seu aplicativo. Durante uma implantação local, CodeDeploy executa uma atualização contínua em todas as instâncias do Amazon EC2. Você pode especificar o número de instâncias a serem colocadas offline de cada vez para atualizações. Durante uma blue/green implantação, a revisão mais recente do aplicativo é instalada nas instâncias substitutas. O tráfego é roteado novamente para essas instâncias no momento que você escolher, seja imediatamente ou assim que o teste do novo ambiente terminar. Para ambos os tipos de implantação, o CodeDeploy controla a integridade do aplicativo de acordo com regras que você configura.
 
 ![[Pasted image 20260410224024.png]]

~={red}Se você usa uma plataforma computacional EC2/local, esteja ciente de que as blue/green implantações funcionam somente com instâncias do Amazon EC2.=~

● **Redeploy** – Ação de **implantar novamente uma versão anterior bem-sucedida**, criando uma nova implantação com novo ID, usada para restaurar rapidamente um estado funcional.

Ordem de execução dos hooks em implantações in-place utilizando o CodeDeploy:
**In-place deployment:** tipo de implantação em que a aplicação é atualizada **na mesma instância EC2**, sem criar novas instâncias..

**Application Stop -> Download Bundle -> Before Install -> Install -> After Install -> Application Start -> Validate Service**
> Essa é a ordem oficial dos hooks para implantações in-place no CodeDeploy. Primeiro, o aplicativo é parado (Application Stop), os arquivos são baixados (Download Bundle), depois são preparados antes da instalação (Before Install), a instalação é realizada (Install), seguida por ações pós-instalação (After Install), o aplicativo é iniciado novamente (Application Start) e, por fim, a validação do serviço é realizada (Validate Service).

---
Você possui uma aplicação baseada em Java rodando em instâncias EC2 com agentes do AWS CodeDeploy instalados.
Você está considerando diferentes opções de implantação, sendo uma delas a flexibilidade que permite a **implantação incremental das novas versões da aplicação, substituindo as versões existentes** nas instâncias EC2.
A outra opção é uma estratégia na qual um grupo de Auto Scaling é utilizado para realizar a implantação.
Quais das seguintes opções permitirão que você realize a implantação dessa forma? (Selecione duas.)

1- **Warm Standby não é uma opção válida do CodeDeploy**. É um termo usado em cenários de Recuperação de Desastres, onde um ambiente funcional em escala reduzida está sempre ativo para rápida ativação em caso de falha, não para implantações de aplicações.

2- **Correta. Na implantação In-place, a aplicação em cada instância do grupo de implantação é parada, a nova versão é instalada e a aplicação é reiniciada e validada.** Pode-se usar um balanceador de carga para remover a instância do tráfego durante a atualização, garantindo implantação incremental e substituição das versões existentes

3- Incorreta. ""Cattle Deployment"" não é um termo oficial do CodeDeploy e não representa uma estratégia válida para implantações. É um termo coloquial que se refere a tratar servidores como recursos descartáveis, mas não é uma opção de implantação reconhecida.

4- **Correta. Na implantação Blue/Green, um novo conjunto de instâncias é provisionado, e o CodeDeploy instala a versão mais recente da aplicação nelas.** O tráfego do balanceador de carga é então redirecionado do conjunto antigo para o novo, permitindo uma troca segura e controlada, ideal para grupos de Auto Scaling.

5- Incorreta. Implantação Pilot Light não é uma opção válida do CodeDeploy. O termo ""Pilot Light"" refere-se a uma estratégia de Recuperação de Desastres onde uma parte limitada da infraestrutura crítica é replicada para garantir rápida recuperação, não para implantações incrementais ou uso com Auto Scaling.

---

### AWS CodePipeline
É um serviço de integração contínua e entrega contínua que pode ser usado para modelar, visualizar e automatizar as etapas necessárias para lançar seu software. É possível modelar e configurar rapidamente os diferentes estágios de um processo de lançamento de software. É possível compilar, testar e implantar o código sempre que ocorre uma alteração de código, de acordo com os modelos definidos do processo de lançamento.
![[Pasted image 20260410224707.png]]
CodePipeline fornece os seguintes tipos de pipeline, que diferem em características e preços, para que você possa adaptar os recursos e o custo do pipeline às necessidades de seus aplicativos.
- Os pipelines do tipo V1 têm uma estrutura JSON que contém parâmetros padrão no nível do pipeline, do estágio e da ação.
- Os pipelines do tipo V2 têm a mesma estrutura do tipo V1, junto com parâmetros adicionais para segurança de lançamento e configuração de gatilhos.

> **For V1-type pipelines**: You pay $1.00 per active pipeline (a pipeline that has existed for more than 30 days and has at least one code change that runs through it during the month) per month. There is no charge for pipelines that have no new code changes running through them during the month. An active pipeline is not prorated for partial months. Pipelines are free for the first 30 days after creation.
> 
> **For V2-type pipelines**: You pay $0.002 per action execution minute. Action execution duration is calculated in minutes, from the time an action in your pipeline starts executing until that action reaches a completion state, rounded up to the nearest minute. You are charged for all action types except for manual approval and custom action types.

| Aspecto      | V1             | V2                         |
| ------------ | -------------- | -------------------------- |
| Execução     | Sequencial     | Paralela / flexível        |
| Triggers     | Polling        | Event-driven (EventBridge) |
| Performance  | Média          | Alta                       |
| Estrutura    | Rígida         | Modular                    |
| Atualizações | Mais limitadas | Mais seguras               |
| Uso moderno  | Limitado       | Recomendado                |


![[Pasted image 20260410225111.png]]

**O AWS CodePipeline pode ser configurado para ser disparado automaticamente por eventos do Amazon S3, iniciando o processo de implantação imediatamente após uma atualização no bucket, o que é eficiente e recomendado.**