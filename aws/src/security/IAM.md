## IAM Variables
É possível definir valores dinâmicos dentro das políticas usando _variáveis de política_ que definem espaços reservados em uma política.
~={yellow}As variáveis são marcadas usando um prefixo **`$`** seguido por um par de chaves (**`{ }`**) que incluem o nome da variável do valor da solicitação.=~

Quando a política for avaliada, suas variáveis serão substituídas por valores provenientes das chaves de contexto condicionais passadas na solicitação. As variáveis podem ser usadas em [políticas baseadas em identidade, políticas de recursos, políticas de controle de serviços, políticas de sessão](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) e [políticas de endpoint da VPC](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html). As políticas baseadas em identidade usadas como limites de permissões também são compatíveis com variáveis de política.

~={red}Não é case `sensitvie`: aws:CurrentTime equivale a `AWS:currenttime`=~

Exemplo:
```JSON
{
  "Version":"2012-10-17",		 	 	 
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:ListBucket"],      
      "Resource": ["arn:aws:s3:::amzn-s3-demo-bucket"],
      "Condition": {"StringLike": {"s3:prefix": ["${aws:PrincipalTag/team}/*"]}}
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],      
      "Resource": ["arn:aws:s3:::amzn-s3-demo-bucket/${aws:PrincipalTag/team}/*"]
    }
  ]
}

```

## IAM Identity-based Policy
**Identity-based policies** are attached to an IAM user, group, or role. These policies let you specify what that identity can do (its permissions). For example, you can attach the policy to the IAM user named John, stating that he is allowed to perform the Amazon EC2 `RunInstances` action. The policy could further state that John is allowed to get items from an Amazon DynamoDB table named `MyCompany`. You can also allow John to manage his own IAM security credentials.
![[Pasted image 20260208212305.png]]
## IAM Resource-based Policy
**Resource-based policies** are attached to a resource. For example, you can attach resource-based policies to Amazon S3 buckets, Amazon SQS queues, VPC endpoints, AWS Key Management Service encryption keys, and Amazon DynamoDB tables and streams.
![[Pasted image 20260208212412.png]]
![[Pasted image 20260208212705.png]]
![[Pasted image 20260208212843.png]]

## ~={pink}Identity X Resource-based Policy=~
A maior diferença entre essas policy é que a resource possui a configuração de um campo "Principal".

## ~={red}Trust Policy=~
Definem quais entidade principais (contas, users e roles) podem assumir uma Role. O IAM suporta apenas um tipo de politica baseada em recurso, chamado de trust policy, é anexada a uma Role IAM.
![[Pasted image 20260208180010.png]]

## ~={orange}Permissions Boundary=~
Restrição que define o limite máximo de permissões que uma politica baseada em identidade pode conceder a uma entidade IAM. Sempre possui mais prioridade de política.
- Define limite de permissões
- +Prioridade
- Sempre relacionado a uma Identity-based (users, contas e roles)
![[Pasted image 20260208205115.png]]

Exemplo:
![[Pasted image 20260208211223.png]]
Exemplo 2:
Supondo a devida Permission Boundary:
```JSON
{
    "Version":"2012-10-17",		 	 	 
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:*",
                "cloudwatch:*",
                "ec2:*"
            ],
            "Resource": "*"
        }
    ]
}
```
Para a Role abaixo:
```JSON
{
  "Version":"2012-10-17",		 	 	 
  "Statement": {
    "Effect": "Allow",
    "Action": "iam:CreateUser",
    "Resource": "*"
  }
}
```

Se a Role tentar criar um User IAM ela será bloqueado por mais que há essa policy na Role, porém na Boundary não há essa policy permitida. Contudo, ela só vai poder manusear S3, Cloudwacth e EC2.

## IAM Access Analyzer
Ferramenta capaz de identificar Roles IAM não utilizadas e removê-las sem interromper serviços. Ele faz a busca baseada em logs do AWS CloudTrail, ajudando a identificar Role / Policy que não estão mais em uso, implicando no princípio do menor privilégio.

Vale ressaltar, que o IAM Access Analyzer também é a principal ferramenta quando falamos de detectar S3 públicos erroneamente, tal como outros recursos. Permitindo detectar acessos não intencionais aos seus recursos e dados, reduzindorisco de segurança. Contudo, é útil para IAM Resource-based Policy.

O IAM Access Analyzer analisa acesso externo aos seguintes tipos de recursos:
- [Buckets do Amazon Simple Storage Service](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-s3)
- [Buckets de diretório do Amazon Simple Storage Service](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-s3-directory)
- [AWS Identity and Access ManagementFunções do para o](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-iam-role)
- [AWS Key Management ServiceChaves do](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-kms-key)
- [AWS LambdaFunções e camadas do](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-lambda)
- [Filas do Amazon Simple Queue Service](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-sqs)
- [AWS Secrets Managersegredos do](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-secrets-manager)
- [Tópicos do Amazon Simple Notification Service](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-sns)
- [Snapshots de volume do Amazon Elastic Block Store](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-ebs)
- [Snapshots de banco de dados do Amazon Relational Database Service](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-rds-db)
- [Snapshots de cluster de banco de dados do Amazon Relational Database Service](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-rds-db-cluster)
- [Repositórios do Amazon Elastic Container Registry](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-ecr)
- [Sistemas de arquivos do Amazon Elastic File System](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-efs)
- [Amazon DynamoDB Streams](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-ddb-stream)
- [Tabelas do Amazon DynamoDB](https://docs.aws.amazon.com/pt_br/IAM/latest/UserGuide/access-analyzer-resources.html#access-analyzer-ddb-table)

> O IAM Access Analyzer não identifica o acesso público ou entre contas configurado diretamente no próprio banco de dados. O IAM Access Analyzer identifica apenas descobertas para acesso público ou entre contas configurado no snapshot de banco de dados do Amazon RDS.

> ~={red}Importante=~
> ~={red}Para acesso externo, o IAM Access Analyzer analisa somente políticas aplicadas a recursos na mesma região da AWS em que está habilitado. Para monitorar todos os recursos do seu ambiente da AWS, é necessário criar um analisador de acesso externo para habilitar o IAM Access Analyzer em cada região em que você está usando recursos da AWS compatíveis.=~
>  ~={red}Para acessos não utilizados, as descobertas do analisador não mudam com base na região. Não é necessário criar um analisador de acesso sem uso em cada região em que você tem recursos.=~

