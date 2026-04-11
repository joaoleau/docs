
### WCU e RCU Princing Calculator
RCU:
- Uma solicitação de leitura _altamente consistente_ de um item de até 4 KB exige uma unidade de leitura.
- Uma solicitação de leitura _eventual consistente_ de um item de até 4 KB exige meia unidade de leitura.
- Uma solicitação de leitura _transacional_ de um item de até 4 KB exige duas unidades de leitura.

- [GetItem](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html): lê um único item de uma tabela. Para determinar o número de unidades de leitura que `GetItem` consumirá, arredonde o tamanho do item para o próximo limite de 4 KB. Esse será o número de unidades de capacidade necessárias se você tiver especificado uma leitura altamente consistente. Em relação a uma leitura final consistente, que é o padrão, divida esse número por dois.
        Por exemplo, se você ler um item que tem 3,5 KB, o DynamoDB arredondará o tamanho do item para 4 KB. Se você ler um item de 10 KB, o DynamoDB arredondará o tamanho do item para 12 KB.

![[Pasted image 20260228130031.png]]
![[Pasted image 20260228130109.png]]

WCU:
- Uma unidade de gravação representa uma gravação para um item com até 1 KB.
- Se você precisar gravar um item maior que 1 KB, o DynamoDB precisará consumir unidades de gravação adicionais.
- As solicitações de gravação transacional exigem duas unidades de gravação para executar uma gravação para itens de até 1 KB
O número total de unidades de solicitação de gravação necessárias varia de acordo com o tamanho do item. Por exemplo, se o tamanho do item for 2 KB, serão necessárias duas unidades de gravação para comportar uma solicitação de gravação ou quatro unidades de gravação para uma solicitação de gravação transacional.
![[Pasted image 20260228130203.png]]
![[Pasted image 20260228130143.png]]

### DynamoDB Pricing Model
Provisioned Capacity
- You pay for the capcity you provision (= number of reads and writes per second)
- You can use auto-scaling to adjust the provisioned capacity
- Uses Capacity Units: Read Capacity Units (RCU) and Write Capacity Units (WCU)
- Consumption beyond provisioned capcity may result in throttling
- Use Reserved Cacpity for discounts over or 3-year term contrats (you're charged a onetime fee + an houtly fee per 100 RCU and WCU) : ***Use Capacidade Reservada para obter descontos em contratos de 1 ou 3 anos***  
***(você paga uma taxa única + uma taxa por hora para cada 100 RCU e WCU)***

On-demand Capacity
- You pay per request (= number of read and write request your application makes)
- No need to provision capacity unit
- Instantly accommodates your workloads as they ramp up or down
- Uses Request Units: Read Requests Units and Write Request Units
- CAcnnot use reserd capcity with On-Demand mode

- for throughput calculation purpose, request units are equivalent to capacity units
- storage, backups, replication, streams, caching, data transfer out charged additionally 

![[Pasted image 20260228120551.png]]
![[Pasted image 20260228120927.png]]
### DAX
O Amazon DynamoDB Accelerator (DAX) é um cache em memória totalmente gerenciado que melhora a performance das leituras do DynamoDB, reduzindo a latência de milissegundos para microssegundos.

Voce pode montar um Cluster (1 ou mais nós, onde sempre haverá 1 master, podendo haver demais réplicas modo leitura).

Útil pra aplicações que fazem muita requisição de leitura para o Dynamo, colocar o DAX na frente pode ser útil em diminuir latência dessas leitura, agora caso o index não seja encontrado, dai sim vai para o Dynamo.

![[Pasted image 20260227223706.png]]

### Arquitetura
Dynamo tem Partition Key (PK) e Sort Key (SK). A PK serve pra segregar conteúdos em blocos distintos, a ideia é ele ser de forma inicial um inventário, onde estando nele você pode procurar seu item utilizando a SK. 

São permitidos apenas 3 tipos:
S: String
N: number
B: Binary

**Primary Key (PK)**
É o índice que de fato separa inventários no Dynamo. A busca por items na tabela, devem partir da PK, é não é possível utilizar `BEGIN_WITH`. São definidas os tipos de forma inicial durante a criação da tabela.

**Sort Key (SK)**
Índice utilizado para fazer busca no espaço segregado pela PK. Pode ser feito buscas com `BEGINS_WITH` , diferente da PK que não é possível.
- `=`
- `>`
- `<`
- `BETWEEN`
- `BEGINS_WITH`

**Global Secondary Indice (GSI)**
Índice que pode ser criado pós as definições da tabela, através dele você pode fazer rotações, por exeplo, tranforma PK em GSI e realizando buscas com `BEGINS_WITH` semelhante a tratativa de SK.

Assim como GSI podem ser utilizados para buscas bidirecionas, ou seja, supondo que partir de buscar de usuário (username PK) para validar seus produtos, podemos partir de produtos e encontrar os compradores (rotação de PK para GSI.) 
- Pode ser criado depois da tabela definida
- É útil para rotações, transformar PK em SK/GSI

**Local Secondary Index**
O **Local Secondary Index** é um índice **alternativo à Sort Key**, mas **usa a mesma Partition Key da tabela principal**.
- Só pode ser criado durante a criação da tabela
- Compartilha mesma PK que a SK

**Scan x Query**
Scan varre a tabela inteira, literalmente não é recomendado, não faz nenhum uso de índice, e é claro.

Já Query, obrigatoriamente precisa passar a PK para cada busca. Busca otimizada, barata e performatica.

### DynamoDB Consistent

Possui dois tipos de Leituras Consistentes.
Como o Dynamo possui replicação em AZs, isso significa que cada dado gravado deverá ser replicado, a AWS informa que pode demorar até 1s para que o valor seja replicado, isso significa que para a leitura que quer o valor mais recente pode ser perigoso, uma vez que a escolha da réplica do banco é randômica.
Eventual Consistent: 
+ + Perfomance
+ Default
+ Dirty Reads : Pode acontecer de fornecer um valor desatualizado no processo de leitura
Strong Consistent:
- + Cara
- + Latência de Leitura
- Garante que a leitura que esta sendo feita, sempre vai pegar o valor mais recente, uma vez que para cada leitura ele valida as 3 AZs, cada réplica do Dynamo e faz o cruzamento.

### DynamoDB Transactions

"All or Nothing"

Com as Amazon DynamoDB Transactions, você pode agrupar várias ações juntas e enviá-las como uma única operação tudo ou nada `TransactWriteItems` ou `TransactGetItems`

- `TransactWriteItems` é uma operação de gravação síncrona e idempotente que agrupa até 100 ações de gravação em uma única operação tudo ou nada. Essas ações podem ter como destino até 100 itens distintos em uma ou mais tabelas do DynamoDB na mesma conta e região da AWS. O tamanho agregado dos itens na transação não podem exceder 4 MB. As ações são concluídas de forma atômica para que todas sejam bem-sucedidas ou nenhuma tenha êxito.
%% 
- Uma operação `TransactWriteItems` difere de uma operação `BatchWriteItem` em que todas as ações contidas devem ser concluídas com êxito, ou nenhuma alteração foi feita. Com uma operação `BatchWriteItem`, é possível que apenas as ações no lote sejam bem-sucedidas enquanto as outras não.
    
- As transações não podem ser realizadas usando índices. 
%%

Você não pode visar o mesmo item com várias operações dentro da mesma transação. Por exemplo, você não pode executar uma ação `ConditionCheck` e também uma `Update` no mesmo item na mesma transação.

Você pode adicionar os tipos a seguir de ações a uma transação:

- `Put`: inicia uma operação `PutItem` para criar um novo item ou substituir um item antigo com um item novo, de forma condicional ou sem especificar qualquer condição.
    
- `Update`: inicia uma operação `UpdateItem` para editar um atributo de item existente ou adicionar um novo item à tabela se ele já não existir. Use essa ação para adicionar, excluir ou atualizar atributos ou um item existente de forma condicional ou sem uma condição.
    
- `Delete`: inicia uma operação `DeleteItem` para excluir um único item em uma tabela identificada por essa chave principal.
    
- `ConditionCheck`: verifica se um item existe ou verifica a condição de atributos específicos do item.

Para garantir um snapshot atômico dos itens modificados em uma transação, use a operação TransactGetItems para ler todos os itens relevantes juntos. Essa operação fornece uma visão consistente dos dados, garantindo que você veja todas as alterações de uma transação concluída ou nenhuma.
[Amazon DynamoDB Transactions: como funciona - Amazon DynamoDB](https://docs.aws.amazon.com/pt_br/amazondynamodb/latest/developerguide/transaction-apis.html)
### DyanmoDB Stream
Feature that emits events when record modifications occur on a DynamoDB Table. 
- Events can be of types INSERT, UPDATE, and REMOVE.
- Events can carry the content of the row(s) being modified : *Os eventos podem carregar o conteúdo das linhas modificadas*
- Events are guaranteed to be in the same order the modifications took place
- Customizable events  Keys Only, New Img, Old Img, New e Old Img (Nova e Velho conteúdo da linha)
- Batch processing
- No performance impact on source table
- Easy integration with AWS Lambda

Cenário onde uma lambda é trigada pós evento e salva logs no CloudWatch
![[Pasted image 20260228122656.png]]
![[Pasted image 20260228122851.png]]
### Global Tables
