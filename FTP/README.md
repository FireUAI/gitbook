# FTP - Semana 1: Mentalidade Hacker, Fundamentos e Linux

Seja muito bem-vindo(a) ao FireUAI Training Process (FTP) de 2026/2!
Esperamos que esse treinamento sirva não somente para selecionar os
novos membros da nossa equipe, mas também contribua para seu aprendizado
e interesse em Cibersegurança.

Antes de começar com o assunto da semana, é importante esclarecer: esse
documento não tem o objetivo de ser um material exaustivo sobre seu
tema, e nas próximas semanas seguiremos com o mesmo método. Ao
contrário, nosso objetivo aqui é te guiar pelos tópicos e estabelecer
uma base sólida para que **você** possa desenvolver autonomia e
conhecimento para **pesquisar e aprender por conta própria**,
característica essencial da nossa aula. Prepare seu navegador, você vai
precisar!

Nesta primeira semana, vamos definir alguns fundamentos essenciais que
você deve ter em mente durante o treinamento e para a sua eventual
trajetória dentro do FireUAI. Para aprender esses princípios, antes de
sequer pensar em atacar um sistema, você precisa aprender a pensar de um
modo diferente, você precisa aprender a pensar como um atacante.

## Mentalidade Hacker

Você possivelmente já viu diversos filmes, séries e jogos com diferentes
e exageradas representações de hackers apertando um botão e apagando as
luzes de uma cidade (Watch Dogs), roubando documentos confidenciais (Mr.
Robot), etc. Para aprender como atacar um sistema, esqueça
**absolutamente** tudo o que você já viu na cultura pop: aqui você vai
receber um choque de realidade sobre como ataques realmente funcionam.

Você não vai se formar na faculdade e estar apto a derrubar uma mega
corporação com um ataque ultrassofisticado, nem vai conseguir roubar
documentos confidenciais de agências governamentais na sua primeira
semana. Na vida real, esse tipo de coisa demanda décadas de estudo e
investimentos enormes em pessoas especializadas, tempo e dinheiro. Em
geral, os grandes ataques noticiados por jornais são obras de equipes
gigantescas de hackers, frequentemente ligadas a governos inteiros, e
demandam anos de trabalho. Se for do seu interesse, você possivelmente
vai estudar cibersegurança a sua vida inteira e ainda não será capaz de
compreender a área em sua totalidade, quem dirá executar um desses.

Para atender essa demanda de especialização, o FireUAI é dividido em
squads para tópicos específicos. Antes de tudo, hackear é um **trabalho
em equipe**, depende de colaboração entre pessoas de diferentes pontos
fortes e fracos, diferentemente do que é retratado na mídia. Além disso,
outro dos principais erros de quem começa na área de Segurança Ofensiva
é acreditar que o conhecimento se resume a saber usar várias
ferramentas. Não é o caso. Dentro do meio, esse tipo de pessoa recebe o
rótulo pejorativo de *Script Kiddie*, um papagaio que repete comandos
que não entende. O objetivo deste treinamento não é te ensinar a apertar
botões no *Burp Suite* ou usar o *Nmap*, mas sim fazer você entender o
porquê está executando cada etapa. No contexto do treinamento, nosso
foco é te apresentar conhecimento que não é divulgado por aí, porque não
dá dinheiro para vendedor de curso. Por natureza, esse conhecimento não
é linear e depende de muitos pré-requisitos, se expandindo e se
dividindo em várias direções diferentes.

**\"OK, entendi. Então o que eu vou aprender de fato?\"** Você vai
aprender a ser curioso, do jeito certo e entender a fundo várias áreas
da computação.

### Abstração e Resiliência

Com o intuito de quebrar uma aplicação, você precisa primeiro
**entender** como ela foi construída. Para isso, o trabalho de um hacker
envolve construir um modelo mental do sistema: Como os dados trafegam?
Como o estado da sessão é mantido? Onde a validação está ocorrendo?

Com isso, uma vez que você consiga modelar a arquitetura na sua cabeça,
as vulnerabilidades de um sistema se tornam consequências lógicas do seu
próprio funcionamento. Nesse sentido, existe uma sabedoria popular da
comunidade de offsec que diz: \"Se você não sabe qual falha tentar
explorar, volte para a enumeração\", vamos falar mais disso abaixo.

Como consequência, o pensamento lógico, e principalmente o pensamento
intuitivo é uma das principais características de um hacker. Porém, esse
tipo de raciocínio só é adquirido através de experiência, resolução de
CTFs e muita, muita prática. Durante esse processo, você vai dar de cara
com várias paredes inúmeras vezes. Isso **é normal, e é um bom sinal**:
significa que você está aprendendo de verdade. Porém, se a cada
obstáculo que você enfrentar você disser para si mesmo: \"Ok, eu não sei
esse conceito, eu não vou conseguir\" a verdade é que você provavelmente
não vai mesmo, mais que isso, com esse tipo de mentalidade você vai
desistir dessa área rapidamente. Ao invés disso, foque em não cometer o
mesmo erro duas vezes: hacking é sobre entender os obstáculos e aprender
a contorná-los de forma inteligente!

### A Arte da Pesquisa e Fundamentos

Tocando novamente no assunto ferramentas, nós poderíamos gastar páginas
e páginas enumerando conceitos básicos como HTTP, SQL, XSS, TCP/IP, etc.
Mas, sendo muito sincero, no mundo de hoje isso não faz muito sentido.
Se você quer aprender sobre um assunto específico é só abrir o Google ou
ChatGPT e pedir uma explicação, informação para aprendizado autodidata
nunca foi tão acessível. Nós vamos te aconselhar com alguns dos caminhos
possíveis, indicar os melhores materiais, mas no fim **quem é o
responsável pela sua trajetória e aprendizado é você mesmo.**

Esse tipo de abordagem teria ainda mais um problema: A área de
tecnologia evolui diariamente, e o que você aprende hoje pode estar
obsoleto amanhã. Portanto, a habilidade mais fundamental que você vai
desenvolver aqui é a de buscar conhecimento de forma autônoma. Assim,
isso nos leva a uma regra essencial da computação e do hacking: **RTFM**
(Read The Fucking Manual - Leia o maldito manual). Por um motivo de
praticidade, é praticamente impossível saber o funcionamento exato de um
servidor rodando Windows 7 *a priori*, ao invés disso, é mais importante
ter a capacidade de aprender **sob demanda** quando você precisar de um,
isso se estende para todos os outros conceitos que você vai precisar
nesse treinamento. Sempre que se deparar com uma tecnologia desconhecida
(um novo framework, um banco de dados exótico), seu primeiro instinto
deve ser ler a documentação e tentar entender como funciona. Além disso,
existe mais uma coisa para se manter em mente: Você **NUNCA** vai
entender como explorar um SQL Injection sem saber o básico de SQL. É
impossível. Não pule etapas. Domine os fundamentos, e você não terá
problemas em construir conhecimento complexo sobre esse solo firme. Por
isso, sempre que identificar uma lacuna no seu próprio conhecimento,
busque primeiro voltar ao básico, aos fundamentos, e reconstrua seu
entendimento passo a passo.

### Ética e Ambiente de Testes

Desnecessário dizer, mas nunca teste em um sistema real sem autorização!
Isso é cibercrime, não hacking. Antes de qualquer outra, essa regra é
absolutamente inegociável para a sua permanência no treinamento e no
grupo. Para seu próprio resguardo, além disso, nunca teste nada sem uma
autorização **explícita, detalhada e assinada**.

Porém, é impossível que você aprenda hacking sem hackear, portanto, para
que você possa praticar, ao longo das semanas nós forneceremos ambientes
de laboratório propositalmente vulneráveis, para que você possa testar e
desenvolver seu conhecimento. Apesar de ser uma ótima forma de começar,
o seu aprendizado não deve ficar restrito apenas aos alvos que nós
entregamos. Para ir além, se você quer realmente entender como quebrar
algo, aprenda construindo. Com isso em mente, codifique uma aplicação
web simples, cometa erros de segurança de propósito e, em seguida,
ataque-a, demonstre na prática a ameaça que aquela falha representa.
Mais do que ter uma visão externa, quando você mesmo constrói o sistema
vulnerável e depois o explora, seu nível de retenção de conhecimento e
sua intuição sobre as falhas aumentam drasticamente.

### Sua Metodologia e Anotações

Conforme você avançar no treinamento e na área, seguir tutoriais
cegamente vai deixar de funcionar. Para ser efetivo, você precisará
criar a sua própria metodologia de ataque, o seu próprio sistema de
operação. A construção desse sistema não é algo escrito em pedra; ele
evolui e amadurece de forma proporcional ao seu conhecimento técnico.

Ao final do treinamento, se você decidir seguir na área de hacking, é
fundamental ter uma fórmula própria que funcione para você. Porém, não
encare esse plano como \"O Método Mágico Infalível Para Quebrar Qualquer
Sistema\", mas sim como o seu guia: ele define qual é o seu ponto de
partida, por quais caminhos seguir após a exploração, e como não se
perder no caos. Ao mesmo tempo, ele deve ser dinâmico o suficiente para
se adaptar quando surgir a necessidade.

Para se manter organizado, é fundamental registrar todo o seu processo,
sua metodologia e os progressos feitos. Sob essa ótica, a documentação
não é um trabalho burocrático que você faz depois do ataque, ela é uma
parte ativa da sua metodologia durante o ataque. Isso ocorre porque, se
você passar 4 horas testando payloads em um alvo e não anotar o que já
falhou, vai repetir os mesmos erros. Não importa qual seja, encontre a
ferramenta que funciona para você (Notion, Obsidian, arquivos de texto
simples), mas crie o hábito.

## Como ser um invasor

Apesar de cada sistema exigir uma abordagem única, praticamente todo
ciberataque (inclusive a resolução de CTFs) segue um fluxo lógico. Por
isso, Hacking não é chutar senhas ou rodar scripts da sua IA favorita
até algo dar certo; é um processo analítico e estruturado.

**Ao invés de um sistema digital, imagine que seu objetivo é invadir um
prédio muito bem protegido.**

Um amador (ou *Script Kiddie*) simplesmente correria até a porta da
frente e tentaria arrombá-la com um chute. Claramente, essa estratégia
faria barulho, chamaria a atenção dos vizinhos, dispararia o alarme e a
polícia chegaria em minutos. Ele falhou porque pulou as etapas. Um
profissional age de forma diferente.

### Reconhecimento (Recon)

Um profissional não começa encostando no prédio. Ao invés disso, ele
passa de carro pela rua, verifica quantas entradas o terreno possui, se
há câmeras de segurança, se os muros são altos e qual é a rotina dos
moradores.

Primeiro, você **mapeia o alvo**. Sendo o alvo digital, você descobriria
quais são os endereços IP, quais os subdomínios estão disponíveis na
internet, checa o LinkedIN da organização, enfim, coleta informações
**públicas**, enquanto o alvo nem sabe da sua existência. Assim, você
tem uma visão completa da superfície de ataque para saber exatamente o
que irá enfrentar.

### Enumeração

Com o prédio mapeado por fora, o invasor se arrisca um pouco mais e se
aproxima para olhar os detalhes. Por isso, ele não vai arrombar a porta
principal, mas ele vai até lá para ver qual é a marca da fechadura.
Então, ele dá uma volta nos fundos, testa as maçanetas das saídas de
emergência para ver se alguma está frouxa e percebe que uma janela de um
banheiro no segundo andar ficou levemente aberta. Nessa fase, já existe
a chance de um funcionário ver ele por lá e achar o comportamento
suspeito mas, se ele for um profissional, ele é capaz de apagar seus
rastros e se disfarçar na multidão.

No digital, essa é a fase de **interrogar o alvo**. Você descobriu na
fase de Recon que existe um servidor Web rodando na porta 80. A
informação é útil, mas apenas isso não é o suficiente. Na Enumeração,
você descobre que esse servidor é um Apache versão 2.4.49, que existe um
diretório oculto chamado /admin~backup~, ou que o formulário de login
aceita caracteres especiais. Com isso, é aqui que você confirma
informações e separa o ruído daquilo que é útil. Você encontra a janela
aberta.

A primeira pergunta que vem à mente sempre é \"Mas e se não houver
nenhuma janela aberta?\". É quase impossível não ter nenhuma janela
aberta em um prédio com 100 delas. Além disso, se não houver janelas
abertas, outra falha diferente do prédio vai existir, e assim por
diante.

Como dissemos antes, sempre que ficar sem ter o que explorar, considere
que sua busca por informação falhou e recomece, você certamente esqueceu
alguma janela.

### Exploração (Exploitation)

Somente agora, com a inteligência coletada, o ataque realmente acontece.
O invasor sabe que a janela do segundo andar está aberta, então ele traz
uma escada do tamanho exato, sobe em silêncio e entra no prédio sem
disparar os alarmes do térreo que ele havia identificado. Seguindo esses
passos, ele encontra pouca ou nenhuma surpresa no processo de invasão;
ele **diminui o risco do momento mais crucial**.

No mundo digital, com as informações em mãos, você age. Finalmente, é o
momento de enviar o payload exato para explorar aquela versão específica
do Apache que você enumerou, contornar a restrição de login ou injetar o
comando que vai te dar acesso à máquina. A exploração é apenas uma
consequência natural de uma enumeração bem feita.

Se você tentar explorar um sistema antes de enumerá-lo completamente,
estará apenas dando chutes na porta da frente. Portanto, force-se a
seguir essa ordem: olhe ao redor, entenda o que está na sua frente e só
depois pense em como quebrar.

### Pós-exploração

O invasor conseguiu entrar pela janela e agora está dentro do prédio. O
trabalho acabou? Longe disso. Ele não vai ficar parado no banheiro, por
que alguém ficaria parado no banheiro?

Agora, com o objetivo de causar algum impacto real, ele precisa acessar
as salas adjacentes (Movimentação Lateral), encontrar o cofre escondido
no quarto principal para roubar objetos de valor (Escalonamento de
Privilégios) e, antes de ir embora, destrancar a porta dos fundos para
poder voltar na noite seguinte sem precisar usar a escada de novo
(Persistência). Por fim, ele precisa também limpar suas impressões
digitais da maçaneta (Limpeza de Rastros).

No mundo digital, conseguir o acesso inicial raramente te dá controle
total. Geralmente, você entra como um usuário comum ou um serviço com
permissões limitadas. Na Pós-exploração, o seu objetivo é explorar
falhas internas para se tornar o administrador do sistema (root),
mover-se para outros servidores na mesma rede, extrair dados sensíveis
ou impedir seu funcionamento normal e instalar um backdoor para garantir
que você não perca o acesso se o administrador reiniciar a máquina ou
corrigir a falha inicial. Entrar é apenas o começo; é na pós-exploração
que o estrago real acontece.

### Conclusão

Seguindo o processo descrito acima, você será capaz de entender o
funcionamento de um atacante e, a partir dele, conseguir fazer um teste
de intrusão mais real. Por isso, tenha esses passos em mente durante
todo o resto do seu treinamento, eles te serão muito úteis para entender
e vencer os desafios propostos. Agora que você já entende um pouco de
como um hacker pensa, vamos conversar sobre a caixa de ferramentas que
torna o seu trabalho possível: **O seu computador**

## Vamos falar do Pinguim

Se você pretende seguir na área, ou até em computação no geral, você
inevitavelmente vai acabar lidando muito com Linux, querendo ou não.

O objetivo desse documento não é entrar em detalhes sobre seu
funcionamento, novamente, já existem diversos tutoriais e informação
disponível para instalar e se familiarizar com o sistema, mas falaremos
uma das coisas fundamentais que você precisa saber: **Tudo no Linux é um
arquivo.**

Falamos literalmente, um documento de texto é um arquivo. As
configurações do sistema são arquivos de texto. Seu disco rígido é lido
pelo sistema como um arquivo. Os processos rodando na memória são
arquivos. Seu teclado e mouse escrevem em arquivos e são processados
para funcionarem. Por isso, conseguir navegar, ler e manipular arquivos
através da Linha de Comando (CLI) é um princípio fundamental para que
você saiba lidar com Linux.

### Terminal

A maioria das pessoas usa computadores todos os dias mas nunca chegou a
encostar em um terminal. Lembre-se do RTFM, não vamos entrar em detalhes
sobre como dominar seu uso, mas podemos te dar um guia dos tópicos que
você precisa saber. Portanto, para resolver os desafios dessa semana,
pesquise e entenda o funcionamento básico dos seguintes conceitos:

- **Navegação de Diretórios:** Como saber onde você está, como listar o
  que tem dentro da pasta, como se mover entre elas e arquivos ocultos.
- **Manipulação e Leitura de Texto:** Como ver o conteúdo de um arquivo
  pelo terminal. Como extrair textos de arquivos binários. Como criar
  novos arquivos e editá-los.
- **Pipes:** Aprenda como funciona a concatenação de comandos. Isto é,
  como pegar a saída de um comando e jogar como entrada do próximo. Os
  usos dessa habilidade são diversos, um exemplo de cadeia com esse
  funcionamento seria listar os arquivos, jogar para um filtro, e depois
  jogar para um decodificador. Ferramentas do Linux são construídas
  seguindo a **Filosofia Unix** (Em poucas palavras, cada ferramenta
  deve fazer apenas uma coisa e fazer muito bem, pesquise sobre isso!),
  somente ao combiná-las é possível extrair o máximo de sua utilidade.
- **Permissões:** O Linux usa um sistema de segurança baseado em dono,
  grupo e permissões (Leitura, Escrita, Execução). Se você tentar apagar
  ou ler um arquivo e receber um erro de *Permission denied* é o sistema
  operacional fazendo o trabalho dele porque você não tem os privilégios
  necessários. Entenda o seu lugar na máquina, entenda o lugar dos
  outros na máquina, entenda o que você tem acesso e o que você pode
  fazer.

## Sobre os Relatórios Semanais

Como falado anteriormente, documentar é parte do processo. Junto com
este material, você recebeu o nosso Template de Relatório Semanal. Ele é
simples e direto. A formatação é apenas uma sugestão, você pode
alterá-la, mas mantenha as informações solicitadas.

Para cada semana vamos disponibilizar um ou mais desafios com um total
de três flags, que representam a conclusão do desafio. Você não precisa
necessariamente encontrar as três, mas precisa ao menos documentar suas
tentativas.

O que desejamos é avaliar a sua capacidade de raciocínio e a sua
habilidade de documentação dos passos. Mais do que entregar simplesmente
a flag, você irá descrever como chegou até ela. Você pode adicionar
imagens, prints e os comandos utilizados nesse processo, isso ajuda a
entender o seu processo de raciocínio e comprova que você aprendeu o que
abordamos durante a semana. No entanto, não foque somente no caminho do
sucesso, descreva também os obstáculos enfrentados e os erros com os
quais você se deparou. A resiliência e a capacidade de explicar o
próprio erro valem tanto quanto acertar a resposta de primeira. Além
disso, muitas vezes uma ideia de solução pode aparecer lendo o que você
já tentou. No fim, salve o relatório em formato PDF e envie pelo
formulário que vamos disponibilizar para cada uma das semanas. Fique
atento, pois existe um prazo limite para a entrega de cada relatório.

Durante todo o processo, iremos prestar orientações pontuais no Discord,
mas atenção, a política é: somente peça ajuda depois de esgotar todas as
suas tentativas e pesquisas (RTFM). Não vamos de forma alguma dar a
solução de qualquer desafio. Aplique a metodologia de reconhecimento e
enumeração, lembre-se de que tudo no Linux é um arquivo e documente cada
passo. Ademais, se você está tentando algo muito mirabolante,
possivelmente não está certo, pensar demais às vezes atrapalha tanto
quanto pensar de menos.

Por fim, sobre o uso de Inteligência Artificial: na dúvida, peque pela
falta. É nadar contra a maré proibir o uso de IA durante toda a execução
do relatório semanal, mas recomendaríamos que você não utilizasse esse
tipo de modelo para escrita do texto ou enviasse a descrição ou arquivos
pertencentes aos desafios para a análise desse tipo de modelo.
Justamente por funcionarem muito bem, é extremamente tentador usar esse
tipo de ferramenta como muleta e acabar por terceirizar todo o seu
trabalho, isso te tira o desconforto de errar, mas te impede da
satisfação de aprender. Nesse sentido, recomendamos que a IA seja
utilizada apenas para eventuais correções ortográficas, de clareza ou de
concordância **após** a escrita completa do relatório. Além desses,
outros usos responsáveis incluem sumarização de conceitos, dúvidas
conceituais ou solicitações de explicações adicionais sobre qualquer
tópico teórico que envolva os desafios.

Para quem optar por utilizar tal ferramenta, recomendamos que inclua uma
seção no relatório chamada \"Uso de Inteligência Artificial\". Nesse
contexto, essa seção não reduz pontos do aluno, ao contrário, demonstra
transparência com o processo e boa-fé no contexto de aprendizado. Tenha
em mente que, para trabalhos que apresentarem grandes indícios de uso
irresponsável dessa tecnologia sem a presença dessa seção, a correção
poderá acontecer com maior rigor, pois nosso objetivo é medir o que você
aprendeu, não fazer uma benchmark de um modelo comercial.

## Materiais Recomendados

FIREUAI. Conceitos básicos. Disponível em: <https://fireuai.gitbook.io/fireuai/introducao/conceitos-basicos>.

FIREUAI. O essencial de Linux. Disponível em: <https://fireuai.gitbook.io/fireuai/fundamentos/essencial-de-linux>.

FIREUAI. Tipos de hacker. Disponível em: <https://fireuai.gitbook.io/fireuai/introducao/tipos-de-hacker>.

MITNICK, Kevin D.; SIMON, William L. The Art of Intrusion: The Real
Stories Behind the Exploits of Hackers, Intruders & Deceivers.
Indianapolis: Wiley Publishing, 2005.
