## AWS CDK
Criar infra como código/linguagem de programação, ou seja, usar Javascript/Go/Java... para criar Infra. No final isso vai virar CloudFormation, porém é de forma inicial recomendado a usar Template pré prontos de AWS CDK: [class Template · AWS CDK](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.assertions.Template.html)
![[Pasted image 20260208214748.png]]
![[Pasted image 20260208214843.png]]
● **Stack**: conjunto lógico de recursos AWS gerenciados e implantados juntos como uma unidade.  
● **Sintetizar (Synthesize)**: processo de conversão do código CDK em templates CloudFormation, que descrevem os recursos a serem criados na AWS.  
● **Build**: processo opcional de compilação do código-fonte, utilizado para verificar erros antes da implantação e garantir que o código esteja correto.  
● **Deploy**: processo de implantação dos recursos definidos na stack na conta AWS, criando ou atualizando recursos conforme especificado.
## CloudFormation
Ferramenta de provisionamento de recursos. É a base de "Infra as code" na AWS, SAM, CDK tudo no final vira CloudFormation, diferente do Terraform não temos tfstate, o state é gerenciado diretamente pela AWS.

#### Seções no Template:

**Conditions**: Serve pra **criar lógica condicional** no template.Você define expressões booleanas (`true/false`) que depois podem ser usadas em:
- `Resources`
- `Outputs`
- algumas `Properties`
Casos:
- Criar um recurso **só em prod**
- Escolher tipo de instância baseado em parâmetro
- Ativar/desativar features
`Conditions:   IsProd: !Equals [ !Ref Env, "prod" ]`

Depois você usa assim:
`Condition: IsProd`

**Resources**: Tudo que a stack cria fica em Resources. Exemplo: EC2, S3, IAM Role, ALB e Lambda.
Cada resource tem:
- **Logical ID** (nome interno do template)
- **Type** (AWS::Service::Resource)
- **Properties**
```YAML
Resources:
  MyBucket:
    Type: AWS::S3::Bucket

```

**Mappings**: Dicionário estático para o template. Exemplo:
```
Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-123
    sa-east-1:
      AMI: ami-456

```
**Parameters**: Parâmetros passados via Console, CLI, CI/CD.
```
Parameters:
  InstanceType:
    Type: String
    Default: t3.micro

```

**Outputs**: Expose valores da stack **pra fora**.
```
Outputs:
  LoadBalancerDNS:
    Value: !GetAtt MyALB.DNSName

```
#### StackSets
Recurso que permite criar, atualizar ou deletar stack em múltiplas contas e regiões simultaneamente, facilitando implantação centralizada de infra.
