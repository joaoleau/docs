### AWS CodeCommit
É um serviço de controle totalmente gerenciado que hospeda repositórios Git privados, diretamente na nuvem. Contudo, pode criar repositórios e fazer controle de versão.
-  **Armazene seu código com segurança**. CodeCommit os repositórios são criptografados tanto em repouso quanto em trânsito.
-  CodeCommit suporta comandos Git, bem como seus próprios AWS CLI comandos e. APIs.
- Não há limites arbitrários no tamanho dos arquivos, tipos de arquivo e tamanho do repositório.
![[Pasted image 20260410220201.png]]
#### Como é CodeCommit diferente do controle de versão de arquivos no Amazon S3?
CodeCommit é otimizado para o desenvolvimento de software em equipe. Ele gerencia lotes de alterações em vários arquivos, que podem ocorrer paralelamente às alterações feitas por outros desenvolvedores. O versionamento do Amazon S3 é compatível com a recuperação de versões anteriores de arquivos, mas não se concentra nos recursos colaborativos de rastreamento de arquivos que as equipes de desenvolvimento de software precisam.

### AWS CodeBuild
É um serviço de compilação totalmente gerenciado que compila o código-fonte, **executa testes em unidades e produz artefatos prontos para implantação**. Ele fornece ambientes de compilação com pacotes predefinidos para linguagens populares de programação e ferramentas de compilação, como Apache Maven, Gradle, entre outras. 
- Você também pode personalizar ambientes de compilação CodeBuild para usar suas próprias ferramentas de compilação.
- **Sob demanda** — CodeBuild escala sob demanda para atender às suas necessidades de construção. Você paga somente pela quantidade de minutos de compilação que consumir. **(EC2, Lambda e Reserved Capacity)**
- **Pronto para uso** — CodeBuild fornece ambientes de construção pré-configurados para as linguagens de programação mais populares. Tudo o que você precisa fazer é apontar para o seu script de compilação para iniciar sua primeira compilação.
![[Pasted image 20260410220537.png]]
Como mostra o diagrama a seguir, você pode adicionar CodeBuild como uma ação de construção ou teste ao estágio de construção ou teste de um pipeline em **AWS CodePipeline**. AWS CodePipeline é um serviço de entrega contínua que você pode usar para modelar, visualizar e automatizar as etapas necessárias para liberar seu código. Isso inclui a compilação de seu código. Um _pipeline_ é uma construção de fluxo de trabalho que descreve como as alterações de código atravessam um processo de lançamento.

> **Onde o código-fonte é armazenado?** O CodeBuild no momento é compatível com a compilação pelos provedores de repositórios de código-fonte a seguir. O código-fonte deve conter um arquivo de especificação de compilação (buildspec). _buildspec_ é uma coleção de comandos de compilação e configurações relacionadas, no formato YAML, que o CodeBuild usa para executar uma compilação. É possível declarar um **buildspec** em uma definição de projeto de compilação.

![[Pasted image 20260410221125.png]]

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
