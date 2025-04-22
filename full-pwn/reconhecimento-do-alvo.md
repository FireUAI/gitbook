# Reconhecimento do Alvo
**Escrito por Lucas Albano** - *Revisado com uso do GPT-4o*

A etapa de reconhecimento do alvo representa uma das fases mais críticas em qualquer abordagem de segurança cibernética, seja em contextos ofensivos — como testes de intrusão, atividades de _red teaming_ ou exploração real conduzida por agentes maliciosos —, seja em estratégias defensivas voltadas à identificação proativa de vulnerabilidades e exposição indevida. Trata-se de um processo sistemático de coleta e análise de informações públicas ou acessíveis sobre a infraestrutura de um alvo, com o objetivo de mapear sua superfície de ataque e compreender como seus componentes se relacionam.

Ao levantar dados sobre domínios, subdomínios, blocos de IP, dispositivos conectados, servidores ativos, _Virtual Hosts_ e serviços expostos, é possível construir uma visão ampla da estrutura do alvo. Este artigo apresenta um panorama das principais técnicas e ferramentas utilizadas para essa finalidade, abordando desde métodos passivos e discretos até estratégias ativas e intrusivas. Compreender os fundamentos dessas abordagens é essencial tanto para quem atua em segurança ofensiva, como para equipes de defesa que desejam pensar como um atacante, antecipar suas ações e reduzir a superfície de ataque de sua organização.

---
## Superfície de Ataque

A superfície de ataque representa o conjunto de todos os pontos ou caminhos em um sistema ou rede que um invasor pode explorar para obter acesso não autorizado ou causar danos. Em cibersegurança, trata-se de um conceito essencial para compreender como os sistemas estão expostos a riscos. Ela inclui qualquer interface, como aplicações, dispositivos e até mesmo pessoas que possam ser alvos de um ataque. Quanto maior e mais complexa for a superfície de ataque, mais oportunidades existem para um invasor explorar potenciais brechas de segurança.

Existem três tipos principais de superfícies de ataque: **superfície de ataque digital**, **superfície de ataque física** e **superfície de ataque de engenharia social**. A superfície de ataque digital inclui todas as interfaces que podem ser acessadas remotamente, como endereços IP, portas de rede, APIs, e aplicações web. Já a superfície de ataque física refere-se aos pontos de acesso que exigem proximidade física, como dispositivos IoT ou computadores da organização. Por outro lado, a superfície de ataque de engenharia social engloba os funcionários da organização que podem ser visadas por ataques como phishing e manipulação. Todos os tipos devem ser considerados para um mapeamento abrangente, pois, dependendo do ambiente, uma falha em um dos tipos pode levar a comprometer o outro. Dessa forma, o mapeamento da superfície de ataque visa identificar esses pontos de entrada para encontrar possíveis vulnerabilidades.

---
## Componentes do Alvo

Para o reconhecimento do alvo, é fundamental analisar os diferentes componentes que compõem sua superfície de ataque. Cada um desses componentes — domínios, subdomínios, blocos de IP, servidores web, VHosts, aplicações web e dispositivos expostos — representa um ponto potencial de entrada que pode ser explorado em um ataque ou que necessita de medidas de proteção. A seguir, cada componente será descrito para que, ao final, seja possível compor uma visão abrangente da infraestrutura do alvo.
### Domínios
O domínio é o nome principal associado ao alvo, representando o ponto de entrada para a infraestrutura digital de uma organização. Os domínios são traduzidos para endereços IP através do **Sistema de Nomes de Domínio (DNS)**, um sistema que funciona como uma "agenda telefônica" da internet. Quando um usuário digita um domínio (por exemplo, `linkedin.com`) no navegador, o DNS converte esse nome em um endereço IP (como `13.107.42.14`), que é o endereço de rede real do servidor onde o site está hospedado. Essa conversão é essencial porque computadores e roteadores na internet se comunicam diretamente por meio de endereços IP, enquanto domínios facilitam o acesso para os usuários humanos.

O reconhecimento de domínios geralmente começa com o nome de domínio principal e visa identificar a presença da organização na internet. Organizações podem ter mais de um domínio associado, e geralmente quanto maior a empresa mais domínios ela possui. Conhecer os domínios ajuda a entender a abrangência da presença online do alvo e é o primeiro passo para identificar os recursos conectados à sua infraestrutura.
### Subdomínios
Subdomínios são extensões do domínio principal que apontam para partes específicas do site ou aplicações associadas, como `blog.example.com` ou `api.example.com`. A descoberta de subdomínios revela possíveis aplicações e serviços adicionais que podem ser acessados diretamente. Cada subdomínio pode oferecer um caminho de ataque específico, principalmente se algum estiver desatualizado ou vulnerável.
### Blocos de IP
Blocos de IP referem-se aos intervalos de endereços IP associados ao domínio ou aos subdomínios do alvo. Identificar esses blocos permite mapear a infraestrutura física ou virtual e verificar serviços rodando em diferentes máquinas. Além disso, ajuda a definir os limites da rede e a área que potencialmente precisa ser monitorada ou defendida. Em um ataque, esses IPs podem ser alvos para varreduras de portas e de serviços, indicando vulnerabilidades ou entradas desprotegidas.
### Servidores Web
Os servidores web hospedam as páginas e aplicações que os usuários acessam. Eles podem ser configurados de forma variada, e algumas configurações inadequadas (como versões desatualizadas ou módulos desnecessários) tornam o servidor vulnerável a ataques. Conhecer a versão e os serviços habilitados nos servidores pode ajudar a identificar vetores de ataque.
### VHosts (Virtual Hosts)
VHosts são configurações que permitem que um servidor web hospede múltiplos domínios ou subdomínios em um único endereço IP. Identificar VHosts revela sites e aplicações adicionais sob o mesmo servidor que, por estarem hospedados juntos, podem expor vulnerabilidades interconectadas. Se um VHost tiver falhas de segurança, ele pode comprometer a segurança de outros sites hospedados na mesma infraestrutura.
### Aplicações Web
As aplicações web são os sistemas interativos que os usuários acessam. Elas podem variar em complexidade e segurança e são um dos principais componentes da superfície de ataque. O reconhecimento detalhado de cada aplicação, incluindo frameworks, tecnologias, e endpoints expostos, é fundamental para identificar potenciais vulnerabilidades e caminhos de exploração.
### Dispositivos Expostos
Dispositivos expostos, como dispositivos IoT, representam uma categoria adicional e importante na superfície de ataque. Esses dispositivos, que podem incluir câmeras de segurança, sensores, controladores industriais e outros equipamentos conectados, são especialmente suscetíveis a ataques devido à sua arquitetura simplificada e, muitas vezes, à falta de atualizações de segurança adequadas. No reconhecimento do alvo, identificar dispositivos conectados e acessíveis permite avaliar se estão expostos à internet e verificar se possuem configurações de segurança robustas. Por serem dispositivos de fácil exploração, a presença de um IoT exposto é um fator crítico na análise da superfície de ataque e exige atenção especial, tanto no mapeamento quanto na mitigação de vulnerabilidades que possam comprometer a segurança da rede como um todo.
### Funcionários
Os funcionários de uma organização também compõem a superfície de ataque. Quanto maior o nível de acesso deste funcionário a sistemas internos, maior o impacto causado caso seus acessos sejam comprometidos. Atacantes frequentemente recorrem à engenharia social para obter credenciais ou acesso a dispositivos de funcionários. Isso pode ser feito através de emails, ligações se passando por outro funcionário, contatos em redes social, entre outros. Portanto, descobrir quem são as pessoas de uma organização, seus papéis, privilégios de acesso e meios de contato, também é uma etapa indispensável em testes de invasão e uma preliminar realizada por atacantes reais.

---
## Técnicas de mapeamento da superfície de ataque

A partir de agora vamos explorar algumas técnicas que podem ser utilizadas para identificar a superfície de ataque de um alvo. Grande parte dos métodos explorados tem uso legítimo, como controle de registros e ferramentas de busca, mas podemos explorar essas ferramentas para nossa finalidade de mapear um alvo. 

### OSINT 

OSINT (Open Source Intelligence) é o termo utilizado para descrever a coleta de informações a partir de fontes públicas e acessíveis, como websites, redes sociais, bases de dados abertas e documentos disponíveis na internet. Em cibersegurança, o uso de OSINT é uma prática valiosa para reunir dados sobre um alvo sem recorrer a técnicas invasivas, aproveitando o vasto conjunto de informações publicamente disponíveis para identificar pontos críticos de uma infraestrutura.

Uma abordagem eficaz para encontrar os domínios principais e adicionais de uma organização envolve diversas estratégias. Uma das táticas é a análise de e-mails de contato expostos, que muitas vezes são publicados em páginas de contato ou documentos oficiais. Por exemplo, ao encontrar um e-mail como "contato@empresa.com", é possível identificar o domínio principal, “empresa.com”, e explorar variações que possam indicar subdomínios relevantes.

Além disso, páginas de relação com investidores frequentemente expõem domínios relacionados a filiais ou subsidiárias da empresa. Esses locais são repositórios de documentos e relatórios financeiros que podem conter informações sobre outros domínios associados ao grupo empresarial. A busca no Google também pode ser utilizada de forma estratégica; ao inserir comandos como `site:empresa.com`, é possível listar páginas e subdomínios indexados. Para refinar ainda mais essa pesquisa, operadores adicionais, como `filetype:pdf`, podem ajudar a localizar documentos específicos que frequentemente contêm domínios ou endereços de contato.

Outra técnica útil é realizar buscas por produtos e marcas relacionadas à organização, uma vez que muitas empresas possuem produtos ou marcas secundárias que têm domínios próprios. Essa abordagem permite descobrir domínios adicionais conectados ao alvo, ampliando a superfície de ataque. Ao combinar esses métodos de busca, é possível mapear com precisão os domínios e subdomínios de uma organização, criando uma visão detalhada da infraestrutura online e das interconexões de seus ativos públicos. Para organizações maiores, o próprio Google e a Wikipédia são fontes valiosas de informações. Além disso, para empresas estrangeiras, plataformas como a Crunchbase podem ser particularmente úteis para identificar aquisições e fornecer novas fontes de dados.

---
### Reverse Whois

A técnica de Reverse Whois é outra ferramenta poderosa durante o reconhecimento. Com ela, é possível reverter a busca convencional do Whois, permitindo identificar domínios relacionados a um determinado titular, em vez de buscar informações sobre um único domínio específico. Mas primeiro, o que é esse tal Whois?
#### O que é Whois?
Whois é um protocolo de consulta utilizado para obter informações sobre o registro de domínios na internet. Quando um domínio é registrado, informações essenciais como o nome do registrante, endereço, e-mail e data de expiração são armazenadas em bancos de dados públicos. Essas informações são acessíveis a qualquer pessoa que utilize um serviço de consulta Whois, permitindo que os usuários verifiquem a disponibilidade de um domínio e identifiquem quem está por trás dele. Essa transparência é fundamental para garantir a responsabilidade e a rastreabilidade na internet, embora também possa ser explorada para outros fins.

Existem vários sites que realizam consultas Whois de forma gratuita, dispensando o uso direto do protocolo. No entanto, atualmente o Whois tem retornado cada vez menos informações devido a questões de privacidade e para evitar consultas automatizadas por bots, resultando em campos que retornam apenas "REDACTED FOR PRIVACY". Nessas situações, podemos recorrer a formulários fornecidos pelas entidades registradoras. Para domínios .br, por exemplo, é possível utilizar o Whois do registro.br. Além disso, para descobrir qual entidade é responsável por um domínio, a Internet Assigned Numbers Authority (iana.org) é uma excelente fonte de informação.
#### O que é Reverse Whois?
O Reverse Whois é uma técnica que permite a pesquisa de registros de domínios associados a um determinado registrante ou proprietário. Em vez de buscar informações sobre um único domínio, a pesquisa Reverse Whois permite inserir o nome, e-mail ou outro dado de contato de um registrante para descobrir todos os domínios que pertencem a essa entidade. Essa abordagem é extremamente útil para mapear a infraestrutura de uma organização, identificar domínios associados a uma mesma pessoa ou grupo, e revelar padrões que podem não ser evidentes em uma busca convencional. Além disso, o Reverse Whois pode ajudar a descobrir potenciais vínculos entre diferentes domínios, permitindo uma análise mais profunda da superfície de ataque e das conexões dentro do ecossistema de um alvo.

Existem diversos serviços de Reverse Whois disponíveis na internet, cada um com bases de dados distintas, o que pode resultar em variações na quantidade de domínios retornados para cada consulta e informação base. Entre os serviços populares, destaca-se o Whoxy, que é um serviço pago, mas oferece uma demonstração gratuita. O ViewDNS.info, por sua vez, disponibiliza consultas totalmente gratuitas. O Domain Research Suite oferece até 500 consultas gratuitas, enquanto o ReverseWhois.io é uma opção completamente gratuita. Claro, existem vários outros provedores disponíveis.

---
### Reverse Name Server

A técnica de _Reverse Name Server_ (rNS) é utilizada para identificar domínios adicionais associados a um servidor DNS específico, funcionando de forma similar à técnica de _Reverse Whois_. No rNS, parte-se de um endereço IP ou do nome de um servidor DNS conhecido e realiza-se uma consulta a serviços de DNS reverso que retornam domínios e subdomínios vinculados ao mesmo _Name Server_. Essa técnica permite mapear o ambiente de um alvo e descobrir ativos de rede não documentados, como subdomínios ocultos, ambientes de teste e outras instâncias não indexadas.

Para identificar o _Name Server_ de um domínio, ferramentas como `nslookup` no Windows ou o comando `host` em distribuições Linux são úteis, assim como outras soluções de consulta DNS. Já para as consultas de DNS reverso, várias ferramentas e provedores oferecem suporte. Ferramentas como `dnsrecon`, `Fierce`, e `Sublist3r` são comuns nesta etapa, pois automatizam o processo de consulta e coleta de resultados. Além disso, serviços web como _SecurityTrails_, _ViewDNS.info_, _Domain Research Suite_, _Domain Tools_, entre outros, oferecem recursos avançados de rNS e retornam informações detalhadas sobre domínios e subdomínios associados a servidores DNS específicos.

Contudo, a técnica apresenta algumas limitações. A disponibilidade dos domínios depende da configuração do DNS, das bases de dados das ferramentas e das políticas de segurança do alvo, que podem restringir o acesso a informações de DNS reverso. Em alguns casos, o uso de redes de proteção DNS, como o Cloudflare, pode dificultar o processo de rNS, ocultando registros DNS e limitando as informações disponíveis para coleta.

---
### Expansão de TLD

A técnica de _Expansão de TLD_ (Top-Level Domain) busca descobrir variantes de um domínio alvo utilizando diferentes extensões de domínio. Essa abordagem é especialmente útil para identificar domínios adicionais pertencentes ao alvo, distribuídos em várias extensões além do TLD primário. Dessa forma, a expansão de TLD pode revelar domínios com TLDs alternativos, como `.net`, `.org`, `.info`, `.biz`, e até TLDs específicos de países, como `.br`, `.uk` e `.cn`. A expansão de TLD pode ser realizada por meio de buscas por palavras-chave em plataformas como Whoxy, ferramentas automatizadas como o `dnsrecon`, além do uso do Google.

Utilizando operadores de pesquisa avançada do Google, como `"site:exemplo.*"` ou `related:exemplo.com`, é possível revelar outros domínios de interesse, inclusive domínios de marca ou presença em diferentes países. Ferramentas como o `dnsrecon` também permitem automatizar a expansão de TLDs, possibilitando varreduras de DNS configuradas para procurar por domínios adicionais com TLDs distintos. Com comandos específicos, como o modo `-t tld`, é possível verificar variantes do domínio em diversas extensões.

---
### DNS Zone Transfer

A técnica de _DNS Zone Transfer_ (transferência de zona DNS) é uma operação usada para replicar os dados de uma zona DNS de um servidor para outro. Em ambientes de produção, a transferência de zona é essencial para manter a consistência entre servidores DNS primários e secundários, mas quando explorada por um invasor, essa técnica pode revelar informações sobre a infraestrutura de rede do alvo. A consulta de transferência de zona, caso seja mal configurada, pode permitir que um atacante obtenha uma lista completa de todos os registros DNS da zona, incluindo subdomínios, endereços IP, e outras informações críticas.

Para realizar um _DNS Zone Transfer_, é necessário consultar um servidor DNS autoritativo usando o tipo de consulta `AXFR` (Asynchronous Full Transfer Zone). Essa consulta solicita uma cópia completa dos dados da zona DNS. Se o servidor estiver mal configurado e permitir transferências de zona sem restrições, ele pode responder com todos os registros DNS da zona, expondo detalhes importantes da estrutura do domínio.

Existem várias ferramentas e comandos que facilitam a realização de consultas de transferência de zona:

**Dig**: Em sistemas Linux, o comando `dig` permite realizar uma transferência de zona com o seguinte comando:

```bash
dig @servidor_dns alvo.com axfr
```

Se o servidor DNS responder, ele fornecerá uma lista completa de registros DNS associados ao domínio.

**Ferramentas Automatizadas**: Ferramentas como `dnsrecon` e `Fierce` podem ser utilizadas para automatizar e tentar transferência de zona em uma lista de servidores DNS de forma mais ampla. Com o `dnsrecon`, por exemplo:

```bash
dnsrecon -d alvo.com -t axfr
```

A técnica de _DNS Zone Transfer_ depende de uma configuração inadequada no servidor DNS alvo, o que de fato é raro. Muitos servidores DNS são configurados para limitar a transferência de zona apenas para IPs específicos, geralmente servidores secundários confiáveis, evitando que informações sensíveis sejam expostas. Além disso, muitas organizações utilizam firewalls e outras medidas de segurança para bloquear consultas `AXFR` não autorizadas. Ainda assim, se bem-sucedida, uma transferência de zona pode facilitar o reconhecimento de subdomínios e aumentar significativamente a superfície de ataque.

---
### Certificate Transparency

O _Certificate Transparency_ (CT) é um sistema público e aberto que monitora e registra certificados emitidos por Autoridades Certificadoras (CAs). Ele foi criado para aumentar a segurança e a transparência dos certificados SSL/TLS, permitindo que qualquer pessoa consulte esses registros e verifique os certificados emitidos para um determinado domínio. Para um atacante ou um pesquisador, o CT também pode ser uma fonte valiosa para encontrar subdomínios de um alvo, já que certificados SSL/TLS frequentemente incluem informações sobre subdomínios.

Certificados SSL são registrados em logs públicos de CT, onde geralmente contêm o nome do domínio e de seus subdomínios, como `subdominio.alvo.com`. Isso permite que, por meio de consultas nesses logs, seja possível identificar subdomínios que poderiam não estar listados publicamente, mas que foram utilizados em certificados emitidos pela organização alvo.

Existem diversas ferramentas e serviços que facilitam a busca por subdomínios em logs de _Certificate Transparency_:

- **CRT.sh**: Um serviço gratuito que permite consultar certificados emitidos para um domínio e exibe uma lista de subdomínios encontrados. Basta acessar o site e realizar a consulta:

  ```
  https://crt.sh/?q=alvo.com
  ```

  O CRT.sh retorna uma lista de subdomínios incluídos em certificados registrados, muitas vezes revelando subdomínios não listados em registros DNS públicos.

- **Facebook Certificate Transparency**: Ferramenta fornecida pelo Facebook para verificar certificados emitidos para um domínio específico, permite adicionar alertas para quando um novo certificado é registrado para um domínio. 

  ```
  https://developers.facebook.com/tools/ct
  ```

---
### Bases de Subdomínios

Uma das formas mais diretas de identificar ativos associados a um alvo é consultar bases consolidadas de subdomínios. Essas bases reúnem informações obtidas a partir de fontes públicas como certificados TLS, registros DNS, dados de crawling e resoluções passivas. Plataformas como [dnsdumpster.com](https://dnsdumpster.com) oferecem visualizações gráficas das relações entre domínios, subdomínios e endereços IP. Já serviços como [subdomains.whoisxmlapi.com](https://subdomains.whoisxmlapi.com) permitem consultas por meio de APIs que retornam listas extensas de subdomínios historicamente associados ao domínio principal, incluindo informações temporais relevantes para análise de mudanças na infraestrutura ao longo do tempo.

---
### Bruteforce de Subdomínios

A técnica de brute force de subdomínios consiste em testar exaustivamente nomes comuns ou gerados a partir de dicionários sobre um domínio-alvo, com o objetivo de identificar subdomínios ativos ainda não revelados por outras fontes. Por ser uma abordagem intensiva em requisições DNS, ela é considerada ruidosa e pode ser facilmente detectada e bloqueada por mecanismos de proteção como firewalls ou provedores DNS com limites de taxa. Ferramentas como _ffuf_ e _gobuster_ são amplamente utilizadas para este fim, permitindo ajustes finos como tamanho de wordlists, threads e validação de respostas. Apesar de suas limitações, o brute force pode revelar subdomínios internos, ambientes de teste ou interfaces administrativas que escapam da indexação pública.

---
### Autonomous System

Sistemas Autônomos (ASNs) são blocos de endereços IP administrados por uma mesma entidade, tipicamente um provedor de serviços ou uma grande organização. Cada ASN é identificado por um número único atribuído por registros regionais (como LACNIC, ARIN, RIPE, etc.) e pode ser usado para delimitar a infraestrutura de uma organização-alvo na internet. A consulta ao ASN permite mapear todas as faixas de IP pertencentes a esse sistema, revelando servidores, redes de distribuição de conteúdo, VPNs corporativas e outras interfaces públicas da organização. Ferramentas como [ASNLookup](https://asnlookup.com), [bgp.he.net](https://bgp.he.net/) e APIs como IPinfo oferecem essa funcionalidade. Essa etapa é especialmente relevante para grandes empresas que possuem delegações IP próprias e infraestrutura geograficamente distribuída.

---
### Buscadores de Dispositivos

Buscadores de dispositivos como Shodan, Censys e ZoomEye realizam varreduras massivas da internet e indexam os resultados com base nos _banners_ e metadados retornados por serviços em escuta em diversas portas. Essa técnica permite a identificação de aplicações específicas, versões de serviços, sistemas operacionais, certificados SSL/TLS, títulos de páginas HTTP, entre outros. Por meio de consultas direcionadas, é possível mapear servidores da organização-alvo expostos publicamente, bem como avaliar sua postura de segurança com base em versões obsoletas ou mal configuradas. Censys, por exemplo, permite pesquisas refinadas por fingerprint de certificados, ASN, ou mesmo por fragmentos de texto nos banners. São ferramentas cruciais para reconhecimento passivo e podem ser combinadas com dados de ASN ou blocos de IP para maior precisão.'

---
### Portscan

A varredura de portas é uma técnica clássica e fundamental no reconhecimento de redes, utilizada para identificar serviços ativos escutando em portas específicas de um determinado endereço IP. A ferramenta padrão para essa tarefa é o _Nmap_, amplamente empregada devido à sua versatilidade e ao suporte a múltiplas técnicas de escaneamento. Com o _Nmap_, é possível detectar portas abertas em protocolos comuns — como HTTP (80), HTTPS (443), SSH (22), entre outros — bem como extrair informações adicionais por meio de scripts automatizados, os chamados _Nmap Scripting Engine (NSE)_, que permitem coletar metadados relevantes, como títulos de páginas web (_http-title_), detalhes de certificados digitais (_ssl-cert_) e indicadores das tecnologias empregadas nos serviços expostos.

Do ponto de vista técnico, a varredura pode ser conduzida com diferentes métodos, escolhidos de acordo com o nível de discrição requerido, a profundidade da coleta e os mecanismos de defesa empregados pelo alvo. O tipo mais direto é o _TCP Connect Scan_, que realiza o _three-way handshake_ completo, estabelecendo de fato a conexão com a porta alvo — sendo, portanto, altamente detectável por sistemas de monitoramento. Uma alternativa mais furtiva é o _SYN Scan_ (também conhecido como _half-open scan_), que envia pacotes SYN e analisa as respostas sem completar a conexão, reduzindo a visibilidade do escaneamento. Outras técnicas incluem o _UDP Scan_, voltado à análise de serviços baseados em UDP, cuja ausência de handshake torna a inferência de estados mais ambígua; e o _ACK Scan_, útil para mapear regras de filtragem e identificar firewalls com rastreamento de estado (stateful inspection).

A realização de varreduras de portas, especialmente de forma não disfarçada, pode ser interpretada como um indicador claro da fase inicial de um ataque direcionado. Em resposta, sistemas defensivos bem configurados costumam implementar medidas de detecção e contenção, como limitação de taxa (_rate limiting_), correlação de padrões de escaneamento, e armadilhas como _honeypots_ e _port knocking_, que visam atrasar ou desincentivar o avanço da sondagem. Diante disso, a escolha da técnica de _port scanning_ deve considerar não apenas a eficácia da coleta, mas também os riscos de exposição.

---
### Reverse IP

A técnica de reverse IP explora a coexistência de múltiplos domínios em um único endereço IP, prática comum em servidores compartilhados ou ambientes de virtual hosting. A partir de um IP conhecido da organização-alvo, busca-se identificar todos os domínios que também apontam para esse endereço, com o objetivo de encontrar novas superfícies de ataque relacionadas ao mesmo backend. Serviços como [viewdns.info](https://viewdns.info), [SecurityTrails](https://securitytrails.com), [HackerTarget](https://hackertarget.com) , [DNSlytics](https://dnslytics.com) e até o Bing (usando o operador `ip:`) fornecem essa funcionalidade. Essa técnica é particularmente eficaz contra pequenos alvos que hospedam múltiplos serviços em um mesmo IP e pode revelar interfaces administrativas ou versões de testes de aplicações não diretamente vinculadas ao domínio principal.

---
### Bruteforce de Vhosts

A técnica de _brute force_ de _Virtual Hosts_ (VHosts) consiste em tentar descobrir quais domínios virtuais estão configurados em um determinado servidor web, especialmente quando vários serviços compartilham o mesmo endereço IP. Em servidores HTTP modernos, é comum que múltiplos sites sejam hospedados sob o mesmo IP, sendo diferenciados pelo valor presente no cabeçalho `Host` da requisição HTTP/1.1 ou no campo `:authority` em HTTP/2. Esse modelo de hospedagem virtual permite, por exemplo, que `www.exemplo.com` e `painel.exemplo-interno.net` coexistam no mesmo servidor, ainda que apenas um dos domínios seja publicamente conhecido.

A identificação de VHosts durante a fase de reconhecimento pode revelar aplicações web que não estão diretamente vinculadas aos registros DNS públicos ou que foram intencionalmente ocultadas da superfície da internet. Esses domínios podem expor interfaces administrativas, ambientes de homologação ou outros serviços sensíveis que compartilham a mesma infraestrutura.

O brute force envolve enviar sucessivas requisições HTTP para o IP alvo, variando o valor do cabeçalho `Host` com base em uma _wordlist_ de nomes prováveis, como `admin.empresa.com`, `dev.empresa.com`, `intranet.empresa.com`, entre outros. A resposta do servidor é então analisada para detectar mudanças que indiquem a existência de um VHost válido — como variações no código de status HTTP, no conteúdo da página ou na presença de cabeçalhos específicos. Ferramentas como _ffuf_ e _Gobuster_ facilitam esse processo ao permitir requisições paralelas e análise automatizada das respostas, acelerando a descoberta de domínios virtuais ativos.

A eficácia do brute force de VHosts depende diretamente da qualidade da wordlist utilizada, da configuração do servidor web e da existência (ou não) de comportamentos diferenciais em resposta a domínios inexistentes. Em muitos casos, servidores retornam uma página genérica ou redirecionam para um domínio padrão quando o VHost requisitado não existe, dificultando a identificação por análise superficial. Por isso, é comum empregar heurísticas como análise de entropia da resposta, comparação de _hashes_ ou variação de tamanho de corpo das respostas para detectar anomalias.

---
### Reverse Analytics

_Reverse Analytics_ é uma técnica para extrair e correlacionar identificadores de serviços de monitoramento e análise embutidos em páginas web, como _Google Analytics_, _Google Tag Manager_, _Facebook Pixel_, entre outros, a fim de identificar outros domínios que compartilham os mesmos identificadores. Essa abordagem se baseia na premissa de que um mesmo responsável por um conjunto de aplicações — como uma organização ou provedor de serviços — tende a reutilizar os mesmos IDs de rastreamento em diferentes sites sob seu controle. Ao descobrir essas reutilizações, é possível mapear relações ocultas entre sistemas aparentemente distintos, revelando subdomínios, ambientes internos ou aplicações esquecidas.

Durante a coleta inicial, o atacante analisa o código-fonte HTML e os scripts JavaScript carregados pela página, em busca de identificadores específicos, como `UA-XXXXXX-X` (para Google Analytics), `GTM-XXXXXX` (para Tag Manager), `fbq('init', 'XXXXXXXXXX')` (para Facebook Pixel), entre outros. Essa extração pode ser feita manualmente ou com o auxílio de ferramentas automatizadas, como o [analyzeid.com](https://analyzeid.com), [DNSlytics](https://dnslytics.com), e [HackerTarget](https://hackertarget.com), que mantêm bases de dados públicas de correlação entre IDs e domínios conhecidos.

Após a obtenção de um ou mais identificadores, a etapa seguinte consiste em realizar buscas reversas por esses IDs — processo análogo ao _reverse WHOIS_ — para descobrir quais outros domínios os utilizam. Essa correlação pode revelar, por exemplo, que o domínio público `www.empresa.com` compartilha o mesmo ID de analytics com `dev.empresa.net`, `api.empresa-interna.org` ou até com domínios terceirizados que prestam serviços à organização. Tais descobertas são relevantes quando os domínios correlacionados não estão vinculados ao mesmo conjunto de registros DNS, ASN ou blocos de IP, dificultando sua associação por outras técnicas tradicionais.

É importante notar que a reutilização de identificadores de análise não é limitada a um único domínio ou subdomínio. Muitas empresas adotam o mesmo código de rastreamento em múltiplos sistemas por conveniência ou centralização de métricas, o que, inadvertidamente, fornece um vetor de correlação para adversários. Além disso, mesmo quando os scripts de análise são carregados a partir de domínios externos (como `www.googletagmanager.com`), os parâmetros de configuração costumam estar embutidos diretamente no HTML ou JavaScript da página, tornando-os facilmente acessíveis durante a inspeção.

---
## Considerações Finais

O reconhecimento do alvo é uma etapa crítica em uma atividade ofensiva, seja ela conduzida por pesquisadores de segurança, equipes de _red team_, ou adversários mal-intencionados. Compreender a superfície exposta de um sistema ou organização permite identificar pontos de entrada, áreas negligenciadas e caminhos potenciais para movimentação lateral ou escalonamento de privilégios. Técnicas como enumeração de subdomínios, brute force, análise de ASNs, varredura de portas, mapeamento de _virtual hosts_ e uso de artefatos de análise embutidos exemplificam a diversidade de vetores disponíveis para um agente com conhecimento técnico e criatividade.

Por fim, é essencial lembrar que, embora as técnicas descritas sejam comumente utilizadas em cenários ofensivos, seu entendimento também é indispensável para a defesa. Equipes de segurança que conhecem como um atacante realiza o mapeamento de sua infraestrutura estão em melhor posição para minimizar exposição, identificar falhas de configuração e antecipar vetores de ataque.

---
## Referências

**Mitre ATT&CK Framework – Reconnaissance Tactics**
https://attack.mitre.org/tactics/TA0043/
Estrutura mantida pelo MITRE para categorização de comportamentos ofensivos. A seção de Reconnaissance detalha técnicas utilizadas por agentes maliciosos para coleta de informações iniciais.

**OWASP Reconnaissance Guide**  
https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/01-Information_Gathering/
Guia da OWASP com técnicas e conceitos relacionados à fase de reconhecimento em testes de segurança.

**Nmap - Network Mapper**
https://nmap.org/book/
Documentação oficial do Nmap, ferramenta central para varreduras de portas, identificação de serviços e execução de scripts de reconhecimento.

**ViewDNS.info**
https://viewdns.info/  
Plataforma que oferece um conjunto de ferramentas úteis para reconhecimento, incluindo _reverse IP lookup_, _DNS record discovery_, _subnet enumeration_ e _whois history_.

**DNS Dumpster**
https://dnsdumpster.com/
Serviço online de enumeração de domínios e subdomínios a partir de fontes públicas de DNS.

**ffuf (Fuzz Faster U Fool)**
https://github.com/ffuf/ffuf
Ferramenta de força bruta para fuzzing de caminhos, subdomínios e virtual hosts via requisições HTTP.

**Shodan – Search Engine for the Internet of Everything**
https://www.shodan.io/
Motor de busca especializado em dispositivos e serviços conectados, com foco na coleta de banners e identificação de tecnologias.

**Censys**
https://censys.io/
Plataforma de varredura e indexação de serviços expostos na internet, com foco em análise de certificados e configurações de segurança.

**SecurityTrails**
https://securitytrails.com/
Ferramenta para obtenção de informações históricas e estruturais sobre domínios, IPs e infraestrutura DNS.

**WhoisXML API**
https://whois.whoisxmlapi.com/
Conjunto de APIs para extração de dados de WHOIS, DNS, subdomínios e ASN.

**HackerTarget Tools**
https://hackertarget.com/
Coleção de ferramentas online para testes de descoberta de hosts, DNS, WHOIS, reverse IP, entre outros.

**analyzeid.com**
https://analyzeid.com/
Serviço de extração de identificadores embutidos em páginas web, como IDs de Google Analytics, AdSense e Facebook Pixel.

**Recon-ng Framework**
https://github.com/lanmaster53/recon-ng
Framework modular para automação de tarefas de reconhecimento em segurança ofensiva.

