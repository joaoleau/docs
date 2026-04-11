O Amazon Cognito é uma plataforma de identidade para aplicações web e aplicativos móveis. É um diretório de usuários, um servidor de autenticação e um serviço de autorização para tokens e AWS credenciais de acesso OAuth 2.0. Com o Amazon Cognito, você pode autenticar e autorizar usuários do diretório de usuários integrado, de seu diretório corporativo e de provedores de identidades de consumidores, como Google e Facebook.

> ### Lite
> Lite provides basic user registration, authentication, and management capabilities, including social identity and SAML/OIDC provider integration, and password-based authentication. Lite is targeted for value-oriented use-cases. It includes all Cognito user pool capabilities (without advanced security features) available before November 22, 2024.

> ### Essentials
> Essentials offers comprehensive and flexible user authentication and access control features, allowing customers to implement secure, scalable, and customized sign-up and sign-in experiences for their application within minutes. It includes all capabilities in Lite along with supporting Managed Login and passwordless login options using passkeys, email, or SMS. Essentials also supports customizing access tokens and disallowing password reuse.

> ### Plus
> Plus is geared toward customers with elevated security needs for their applications by offering threat protection capabilities against suspicious log-ins. It includes all Essentials tier features and additionally supports risk-based adaptive authentication, compromised credentials detection, and exporting user authentication event logs to analyze threat signals.

|Categoria|🟢 Lite|🟡 Essentials|🔴 Plus|
|---|---|---|---|
|**Objetivo**|Básico / MVP|Produção padrão|Enterprise / Alta segurança|
|**Autenticação (login/signup)**|✔ Básica (user/password)|✔ Completa|✔ Completa + adaptativa|
|**JWT Tokens**|✔|✔|✔|
|**Grupos (RBAC simples)**|✔|✔|✔|
|**Custom Attributes**|✔|✔|✔|
|**Login social (Google, etc.)**|Limitado|✔ Completo|✔ Completo|
|**SAML / OIDC corporativo**|❌|✔|✔|
|**MFA (multi-factor auth)**|Básico|✔|✔ Avançado|
|**Adaptive Authentication**|❌|❌|✔|
|**Detecção de risco (login suspeito)**|❌|❌|✔|
|**Proteção contra bots/brute force**|❌|Limitado|✔|
|**Customização de fluxo (Lambda triggers)**|Limitado|✔|✔|
|**Custom UI / Hosted UI**|Básico|✔|✔|
|**Monitoramento de segurança**|❌|Limitado|✔ Avançado|
|**Escalabilidade**|Média|Alta|Alta|
|**Integração com Identity Pool**|✔|✔|✔|
## User pools
![[Pasted image 20260410230242.png]]
Create a user pool when you want to authenticate and authorize users to your app or API. User pools are a user directory with both self-service and administrator-driven user creation, management, and authentication **(Os grupos de usuários são um diretório de usuários com criação, gerenciamento e autenticação de usuários por autoatendimento e orientados pelo administrador.)**. Your user pool can be an independent directory and OIDC identity provider (IdP), and an intermediate service provider (SP) to third-party providers of workforce and customer identities. You can provide single sign-on (SSO) in your app for your organization's workforce identities in SAML 2.0 and OIDC IdPs with user pools. You can also provide SSO in your app for your organization's customer identities in the public OAuth 2.0 identity stores Amazon, Google, Apple and Facebook. For more information about customer identity and access management (CIAM), see [What is CIAM?](https://aws.amazon.com/what-is/ciam/).

~={red}User pools don’t require integration with an identity pool. From a user pool, you can issue authenticated JSON web tokens (JWTs) directly to an app, a web server, or an API.=~

## Identity pools
![[Pasted image 20260410230752.png]]
Configure um pool de identidade do Amazon Cognito quando quiser autorizar usuários autenticados ou anônimos a acessar seus recursos. AWS Um grupo de identidades emite AWS credenciais para que seu aplicativo forneça recursos aos usuários. Você pode autenticar usuários com um provedor de identidades confiável, como um grupo de usuários ou um serviço SAML 2.0. Ele também pode emitir credenciais para usuários convidados. Os grupos de identidades usam controle de acesso baseado em funções e atributos para gerenciar a autorização dos usuários para acessar seus recursos. AWS

**Contudo, troca identidade autenticada → por credenciais AWS temporárias.**

~={red}O Identity Pool não autentica!!=~