
**Auto Scaling Groups (ASG)** operam dentro de **uma única Região**, mas podem abranger múltiplas Zonas de Disponibilidade para garantir alta disponibilidade e resiliência.
- O ASG lança instâncias EC2 em sub-redes específicas da VPC e distribui as instâncias **de forma equilibrada entre as AZs habilitadas**, evitando pontos únicos de falha.
    
- Afirmações de que um ASG pode conter instâncias em apenas uma AZ ou abranger múltiplas Regiões são **incorretas**:
    - Somente uma AZ → reduz a tolerância a falhas.
    - Múltiplas Regiões → violaria a arquitetura regional da AWS; cada região deve ter seu próprio ASG.