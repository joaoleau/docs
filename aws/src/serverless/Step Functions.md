ComAWS Step Functions, você pode criar fluxos de trabalho, também chamados de[Máquinas de estado](https://docs.aws.amazon.com/pt_br/step-functions/latest/dg/concepts-statemachines.html), para criar aplicativos distribuídos, automatizar processos, orquestrar microsserviços e criar pipelines de dados e aprendizado de máquina.

O Step Functions é baseado em _máquinas de estado_ e _tarefas_. No Step Functions, as máquinas de estado são chamadas de _fluxos de trabalho_, que são uma série de etapas orientadas a eventos. Cada etapa no fluxo de trabalho é chamada de _estado_. Por exemplo, um [estado de tarefa](https://docs.aws.amazon.com/pt_br/step-functions/latest/dg/state-task.html) representa uma unidade de trabalho que outro AWS serviço executa, como chamar outro AWS service (Serviço da AWS) ou API. As instâncias de fluxos de trabalho em andamento que executam tarefas são chamadas de _execuções_ no Step Functions.

![[Pasted image 20260424220302.png]]

**O Step Functions gerencia os componentes e a lógica do aplicativo para que você possa escrever menos código e se concentrar na criação e atualização rápida de aplicativos. A imagem a seguir mostra seis casos de uso de fluxos de trabalho do Step Functions.**
![[Pasted image 20260424220349.png]]