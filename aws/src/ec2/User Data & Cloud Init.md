Possibilidade de execução de script durante o ciclo de boot da instância. São executados automaticamente no **primeiro boot**.
- **Por padrão, o user data é executado apenas durante o ciclo de boot quando você lança a instância pela primeira vez.**
- **Por padrão, os scripts inseridos como user data são executados com privilégios de usuário root.**

Em **ambientes de desenvolvimento ou produção**, o user data pode automatizar a instalação de agentes de monitoramento, bibliotecas ou configurações necessárias assim que a instância é iniciada.

## Cloud-init
Pacote utilizado em instâncias Linux para processar e executar scripts e configurações durante a inicialização da instância.

