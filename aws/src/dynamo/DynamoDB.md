### DAX
O Amazon DynamoDB Accelerator (DAX) é um cache em memória totalmente gerenciado que melhora a performance das leituras do DynamoDB, reduzindo a latência de milissegundos para microssegundos.

### **Arquitetura**
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

**Gloabl Secondary Indice (GSI)**
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