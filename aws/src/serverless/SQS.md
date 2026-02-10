Possui dois tipos: Standard e FIFO.
- A fila de dead-letter (DLQ) de uma fila FIFO deve ser também FIFO. Assim como, já uma DLQ de uma fila Standard deve ser também fila Standard.
- Filas não podem ser alteradas de tipo pós criação.
### SQS Standard

![[Pasted image 20260209222433.png]]

- **Throughput ilimitado**/~={green}**Unlimited throughput**=~: as filas Standard são compatíveis com um número muito alto e quase ilimitado de chamadas de API por sec, por ação (~={blue}SendMessage=~, ~={blue}ReceiveMessage=~ ou ~={blue}DeleteMessage=~). Contudo, são bons para casos de uso de grandes volumes, processamento rápido, fluxo de dados em realtime ou aplicações de grande escala.
- São escaladas automaticamente de acordo com a demanda.
- **Entrega pelo menos uma vez=~/~={orange}**At-least-once delivery**=~/~={green}**Guaranteed at-least-once**=~: Isso significa que cada mensagem é entregue pelo menos uma vez.
- **Ordenação com melhor esforço**=~/~={green}**Best-effort ordering**=~: Tenta entregar mensagens na ordem em que foram enviadas, porém isso não é garantia.
- **Durabilidade e redundância**/~={green}**Durability and redundancy**=~: Armazena varias cópias de cada mensagem em várias AZs. Garantindo que as mensagens não sejam perdidas.

- **Tempo limite de visibilidade**/~={green}**Visibility timeout**=~: Permite configurar um tempo limite de visibilidade para controlar por quanto tempo uma mensagem permanece oculta após ser recebida, garantindo que outros consumers não processem a mensagem até que ela tenha sido totalmente gerenciado ou que o tempo limite expire. ~={blue}**(Default 30s max 12h, pode ser configurado na fila assim como alterado pelo consumer com ChangeMessageVisibility** para correção de tempo de processamento)=~
![[Pasted image 20260209223531.png]]
Quando você recebe uma mensagem de uma fila do Amazon SQS, ela permanece na fila, mas fica temporariamente invisível para outros consumidores. Essa invisibilidade é controlada pelo tempo limite de visibilidade, o que garante que outros consumidores não possam processar a mesma mensagem enquanto você estiver trabalhando nela. O Amazon SQS oferece duas opções para excluir mensagens após o processamento:
- **Exclusão manual**: você exclui mensagens explicitamente usando a ação ~={blue}DeleteMessage=~.
- **Exclusão automática**: compatível com determinados AWS SDKs, as mensagens são excluídas automaticamente após o processamento bem-sucedido, simplificando os fluxos de trabalho.
Contudo, pós o consumer processar a mensagem da fila, no fluxo normal ela é apagada, se não, ainda permanece na fila e pode ser processada por outro.

**Casos de Usos:**
- **Desacoplar solicitações do usuário em realtime de trabalhos em segundo plano:**  permita que os usuários façam upload de mídia rapidamente enquanto você processa tarefas como redimensionamento ou codificação em segundo plano, garantindo um tempo de resposta rápido sem sobrecarregar o sistema.
- **Alocar tarefas para vários nós de processamento:** distribua um alto número de solicitações de validação de cartão de crédito em vários nós de processamento e gerencie mensagens duplicadas com operações idempotentes para evitar erros de processamento.
- **Agrupar mensagens em lote para processamento:**  coloque várias entradas na fila para adições em lote a um banco de dados. Como a ordem das mensagens não é garantida, projete o sistema para gerenciar o processamento fora de ordem, se necessário.

### SQS FIFO (First-In First-Out)
![[Pasted image 20260209224245.png]]

- ~={green} **High throughput**=~ : com agrupamento em lote, as FIFO processam até 3k mensagens p/sec, por método de API (~={blue}SendMessageBatch=~, ~={blue}ReceiveMessage=~ ou ~={blue}DeleteMessageBatch=~). Esse throughput depende de 300 chamadas de API por segundo, em que cada uma processe um lote de 10 mensagens. Ao habilitar o modo de throughput alto, é possível aumentar a escala verticalmente para até **30 mil transações por segundo (TPS)** com ordenação relaxada em grupos de mensagens. Sem o agrupamento em lote, as filas FIFO permitem até 300 chamadas de API por segundo, por método de API (~={blue}SendMessage=~, ~={blue}ReceiveMessage=~ ou ~={blue}DeleteMessage=~).
- Processamento apenas uma vez/~={green}**Exactly-once processing**=~: as filas FIFO entregam cada mensagem uma vez e a mantêm disponível até que ela seja processada e excluída. Ao usar recursos como ~={blue}MessageDeduplicationId=~ ou a desduplicação baseada em conteúdo, você evita mensagens duplicadas, mesmo em novas tentativas devido a problemas de rede ou tempo limite.
- ~={green}FIFO=~: as filas FIFO garantem que você receba as mensagens na ordem em que são enviadas em cada grupo de mensagens. Ao distribuir mensagens em vários grupos, você pode processá-las paralelamente, mantendo a ordem em cada grupo.

**Casos de Uso**s:
- **Garantir que os comandos inseridos pelo usuário sejam executados na ordem correta**: Esse é um caso de uso importante para filas FIFO, em que a ordem dos comandos é crucial. Por exemplo, se um usuário executa uma sequência de ações em uma aplicação, as filas FIFO garantem que as ações sejam processadas na mesma ordem em que foram inseridas.
- 