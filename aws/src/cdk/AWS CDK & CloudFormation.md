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

### StackSets
Recurso que permite criar, atualizar ou deletar stack em múltiplas contas e regiões simultaneamente, facilitando implantação centralizada de infra.
