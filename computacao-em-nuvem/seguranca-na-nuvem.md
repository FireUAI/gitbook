#Segurança na nuvem
 
**Por Luiz Gontijo**

O artigo tem como objetivo aprofundar na segurança de ambientes na nuvem, para isso, iremos abordar os principais serviços da AWS (uma das principais plataformas de nuvem no cenário global) que envolvem cibersegurança e como medir o nível de segurança de um sistema com base no framework de cibersegurança do NIST (National Institute of Standards and Technology).

## Segurança na nuvem

A grande maioria das empresas tende a seguir o framework de cibersegurança NIST como padrão para os seus recursos na nuvem, de forma com que a segurança na nuvem é composta das seguintes categorias: 

![NISTframework](../Imagens/NISTframework.png)

- Segurança de dados (Protect)
- Gerenciamento de acesso e identidade (Identify)
- Prevenção de ameaças (Detect)
- Retenção de dados (Recover)
- Compliance jurídico (Respond).

- É importante enfatizar que a segurança na nuvem é uma responsabilidade compartilhada entre o provedor e o cliente, ou seja, por mais tecnológicos e avançados sejam os serviços oferecidos pelo provedor, o arquiteto de soluções também tem que ter conhecimento sobre boas práticas de segurança.

### Segurança de dados

É notável que os dados na nuvem têm que ser protegidos a todo momento, ou seja, em trânsito ou em repouso, uma boa prática é classificar os dados conforme o nível de criticidade/ confidencialidade e, a partir disso, usar mecanismos, como criptografia e controle de acesso. Abordando um pouco mais a prática do controle de acesso, é importante tentar manter as pessoas, até mesmo desenvolvedores, afastados dos dados, isso reduz o erro de processamento ou modificação e erro humano.

### Gerenciamento de acesso e identidade

No que tange ao acesso aos serviços computacionais e identidade, é importante se adequar ao princípio do menor privilégio (POLP), que é uma prática de segurança onde o usuário ou recurso deve ter acesso somente ao que é absolutamente necessário e nada mais. Dessa forma, quanto menos serviços um usuário possuir acesso, menor será o impacto negativo se sua conta for comprometida, tópico que abordaremos a seguir./n Além disso, outra importante prática de segurança envolvendo o acesso de usuários à conta é a autenticação segura, ou seja, ativar a autenticação em multifatores (MFA) de maneira com que todos os usuários que possuem acesso aquela conta precisem do MFA para poder acessar os seus recursos e serviços, programar uma senha forte (contendo caracteres especiais, letras e números), alterando-a de tempos em tempos.

### Prevenção de ameaças

A prevenção de ameaças na nuvem é uma prática que visa a identificação de lacunas no ambiente, que podem trazer algum risco aos serviços ou dados hospedados na nuvem. Essa busca por possíveis ameaças deve abranger diferentes “camadas” da nuvem, como a camada de identidade e a camada de rede. Na camada de identidade, é essencial aplicar o princípio do menor privilégio, que foi supracitado no artigo para garantir que usuários e serviços acessem apenas o que é necessário, enquanto na camada de rede é importante isolar os recursos e possuir serviços que atuam como firewall para filtrar o tráfego.

### Retenção de dados

A retenção de dados tem como foco armazenar informações de forma durável, tendo em vista principalmente as necessidades do negócio. É importante notar que, por mais confiável e durável que a infraestrutura de nuvem seja (a AWS informa que apenas 0,000000001% dos arquivos são perdidos por ano), isso não elimina a necessidade de uma estratégia de backup robusta. A durabilidade nativa da plataforma protege contra falhas de hardware, mas o backup é o que possibilita a restauração de arquivos essenciais em cenários de falhas lógicas, como exclusão acidental, corrupção de dados ou ataques cibernéticos. Logo, nota-se que a prevenção contra perda de dados, garantida por rotinas de backup, é uma prática de segurança indispensável.

### Compliance jurídico

Ao abordar o compliance jurídico é perceptível que, a operação na nuvem se baseia em um modelo de responsabilidade compartilhada. O provedor garante que a infraestrutura física global atenda a rigorosos padrões e certificações internacionais, o que constitui a "conformidade da nuvem". Por sua vez, o cliente é responsável por como seus dados e aplicações são configurados, devendo garantir a "conformidade na nuvem". Dessa forma, cabe ao cliente implementar os controles necessários para atender às regulamentações específicas do seu setor, como leis de privacidade de dados. Isso exige a manutenção de registros de auditoria detalhados de todas as atividades, o monitoramento contínuo das configurações para detectar desvios de política e o uso de criptografia para proteger informações sensíveis contra acesso não autorizado.

### Pilares da Segurança na AWS

No que tange ao ecossistema de segurança da AWS, a proteção é fundamentada em serviços principais que cobrem os diferentes domínios que foram citados anteriormente. Na camada de identidade, o **AWS IAM** é o alicerce, sendo essencial para aplicar o princípio do menor privilégio. Para a detecção proativa de ameaças, o **Amazon GuardDuty** monitora continuamente o ambiente em busca de atividades maliciosas, enquanto o **AWS Security Hub** centraliza os alertas de segurança. Além disso, para a proteção de dados e aplicações, o **AWS KMS** gerencia a criptografia de forma centralizada e o **AWS WAF** atua como um firewall para filtrar o tráfego web malicioso. Logo, nota-se que a estratégia de segurança combina serviços de identidade, detecção e proteção para criar uma defesa robusta e em profundidade.

![Principais Serviços de segurança AWS](../Imagens/servicesAWS.png)

## Referências bibliográficas 

- https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-145.pdf

- https://www.nist.gov/cyberframework 

- https://docs.aws.amazon.com/pt_br/whitepapers/latest/aws-overview/security-services.html 