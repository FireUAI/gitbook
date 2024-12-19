Por Thiago Felipe Alves do Carmo
#### Contexto Geral
O desafio envolve a exploração de um Smart Contract Solidity chamado `CryoPod.sol`, o qual permite aos usuários armazenar informações quaisquer em seus respectivos "pods". O objetivo é encontrar uma flag oculta nessa Blockchain.

#### Enunciado do Desafio:
In the shadowed reaches of the Frontier Cluster, where lawlessness reigns and the oppressive Frontier Board tightens its grip, people lost hope for a prosperous future and decided to undergo cryogenic preservation, to be awakened in 50 years to a new world, or never to be awakened again. The CryoPod smart contract serves as a digital vault, housing the secrets of those who sought to preserve some of their most precious memories.

#### Entendendo a Estrutura da Blockchain
A Blockchain é composta por dois Smart Contracts `CryoPod.sol` e `Setup.sol`, o primeiro implementa toda a lógica da Blockchain e o segundo dá deploy no primeiro, adicionando a lógica da função isSolved(), que confere se a flag foi encontrada, então vamos focar no primeiro que contém a lógica principal.

#### Entendendo o Smart Contract `CryoPod.sol`

O desafio fornece os arquivos de ambos os script Solidity, então vamos começar analisando eles, o
smart contract `CryoPod` é relativamente simples. Ele utiliza um mapeamento (`mapping`) chamado `pods` para associar endereços de usuários a strings de dados. A função principal é a `storePod`, que permite que qualquer usuário armazene ou atualize seus dados um dado qualquer na rede.

```sol
function storePod(string memory _data) external {
    pods[msg.sender] = _data;
    emit PodStored(msg.sender, _data);
}
```

É importante notar que, cada vez que um pod é criado ou atualizado, um evento `PodStored` é emitido, incluindo o endereço do usuário e os dados armazenados.

#### Eventos como Chave para a Flag
O enunciado diz que as pessoas estão guardando coisas valiosas nesses pods, tendo isso em mente, porque alguém não guardaria uma flag? Parece valioso o suficiente não? 
Se conseguirmos acessar os logs de eventos passados, podemos talvez encontrar uma flag escondida dentro de um desses pods.

#### Acessando a Rede
Para extrair os eventos da blockchain, vamos construir um Script que acesse os logs do contrato, para isso precisamos das informações obtidas em um servidor http fornecido, além do endereço rcp onde a rede está hospedada, também fornecida:
```sh
>> nc <ip> <porta>
1 - Get connection informations
2 - Restart Instance
3 - Get flag
Select action (enter number): 1
[*] No running node found. Launching new node...

Player Private Key : 284477731b53bf1ca7621e448fed03bf904fee0a8be351e6757dad80598b517d
Player Address     : 0xBd9EDbdCA8cec061AA487F8E70aC89519d30ad5d
Target contract    : 0x112AF6EaC7a55f1E5e1c208E56526B654DB6f300
Setup contract     : 0xD3D1800fdcE22309f25121c84e88001fca41E962
```
Esses valores são gerados toda vez, então confira-os antes de começar a exploração

#### Criando um script para a conexão com o servidor rcp:
Este script utiliza a biblioteca `web3.js` para se conectar à blockchain e buscar logs. Aqui estão os trechos mais importantes:
- **Conexão com a Blockchain:**
```js
const Web3 = require('web3');
const web3 = new Web3('Servidor rcp aqui');
const contractAddress = 'Endereço do contrato Cryopod';
```
Este código inicializa a conexão com a blockchain usando o endereço fornecido e define o endereço do contrato `CryoPod`.
- **Busca dos Logs:**
```js
const options = {
    fromBlock: 0,
    toBlock: 'latest',
    address: contractAddress,
};
const logs = await web3.eth.getPastLogs(options);
```
Aqui, o script especifica as opções para buscar todos os logs de eventos, desde o bloco 0 até o bloco mais recente, para o contrato em questão.
- **Processamento dos Eventos:**
```js
logs.forEach((log) => {
    if (log.topics.length >= 2) {
        const user = web3.utils.toChecksumAddress('0x' + log.topics.slice(26));
        let data;
        try {
            data = web3.utils.hexToUtf8(log.data);
            if (!data) {
                throw new Error("Data não é uma string válida");
            }
        } catch (e) {
            data = log.data;
        }
        console.log('Evento encontrado:');
        console.log('User:', user);
        console.log('Data:', data);
        console.log('Transaction Hash:', log.transactionHash);
        console.log('Block Number:', log.blockNumber);
    } else {
        console.log('Log sem os tópicos esperados:', log);
    }
});
```
Este bloco de código itera sobre os logs de eventos, extrai o endereço do usuário e tenta converter os dados de hexadecimal para UTF-8, mostrando os resultados para cada evento. Se a conversão para UTF-8 falhar, os dados são mantidos em formato hexadecimal.

**Script completo:**
```js
const Web3 = require('web3');
const web3 = new Web3('Servidor RCP');
const contractAddress = 'Endereço do Cryopod.sol';

async function fetchAllEvents() {
    try {
        const options = {
            fromBlock: 0,
            toBlock: 'latest', 
            address: contractAddress, 
        };
        const logs = await web3.eth.getPastLogs(options);
        if (logs.length === 0) {
            console.log('Nenhum evento encontrado.');
        } else {
            logs.forEach((log) => {
                if (log.topics.length >= 2) {
                    const user = web3.utils.toChecksumAddress('0x' + log.topics[1].slice(26));
                    let data;
                    try {
                        data = web3.utils.hexToUtf8(log.data); 
                        if (!data) {
                            throw new Error("Data não é uma string válida");
                        }
                    } catch (e) {
                        data = log.data; // Mantém o dado como hexadecimal
                    }
                    console.log('Evento encontrado:');
                    console.log('User:', user);
                    console.log('Data:', data); 
                    console.log('Transaction Hash:', log.transactionHash);
                    console.log('Block Number:', log.blockNumber);
                } else {
                    console.log('Log sem os tópicos esperados:', log);
                }
            });
        }
    } catch (err) {
        console.error('Erro ao buscar eventos:', err);
    }
}
fetchAllEvents();
```

#### O Caminho para a Flag
Ao executar o script, todos os eventos `PodStored` emitidos pelo contrato são exibidos no console. 
analisando esses dados, é possível identificar alguns com receitas de comida, entre outros, assim como um evento específico que contém a flag.

**Adendo:**
Pode ser necessário executar o script algumas vezes, pois se você tiver sido muito rápido, talvez o evento ainda não tenha ocorrido.

**Flag:** `HTB{h3ll0_*************}`.
