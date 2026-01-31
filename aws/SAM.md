
Serverless Application Model 
Ferramenta para deploy de aplicações serverless via YAML, por base roda CloudFormation, mas a fim de não complicar a criação/provisionamento dessas ferramentas, criou-se o SAM.

Legal do SAM é que dar pra rodar local via Docker.
```
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: >
  Muquirango Local

Globals:
  Function:
    Timeout: 5
    MemorySize: 128

Resources:
  Muquirango:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: src/
      Handler: bootstrap
      Runtime: provided.al2023
      Architectures: [x86_64]
      Events:
        EntryByID:
          Type: Api
          Properties:
            Path: /api/transaction/{id}
            Method: ANY

        Entry:
          Type: Api
          Properties:
            Path: /api/transaction
            Method: ANY
```
