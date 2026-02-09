
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

