# Engenharia Reversa - Visão Geral

**Compilado por Denner Lopes** – *Revisado com uso do ChatGPT-4o*

## Definição

### Contexto Geral

Engenharia reversa é a prática de analisar um produto existente (como software, componentes mecânicos ou placas eletrônicas) para entender seu funcionamento, suas funcionalidades e seu comportamento em diferentes situações. É comum recorrer à engenharia reversa quando precisamos substituir ou modificar uma peça (ou software) sem acesso à sua documentação original.

Por exemplo, imagine uma fábrica em que uma bomba falha e precisa ser substituída. Essa bomba foi instalada há 25 anos, os profissionais que cuidaram da instalação já se aposentaram, e o fornecedor original não existe mais. A fábrica precisa encontrar uma bomba com exatamente as mesmas características, ou seja, compatível com a tubulação existente (dimensões, método de fixação, volume ocupado etc.). Algumas dessas características são facilmente observáveis, mas outras, como vazão exigida ou restrições operacionais específicas, podem ser mais difíceis de identificar — e todas podem ser importantes para a substituição correta.

### Engenharia Reversa na Computação

Na computação, a engenharia reversa consiste em analisar software ou hardware para compreender seu funcionamento interno, mesmo quando a documentação não está disponível.

Essa técnica é utilizada para:

* Identificar vulnerabilidades de segurança.
* Garantir compatibilidade entre sistemas e componentes.
* Recuperar informações de sistemas legados.
* Desenvolver soluções alternativas ou melhorias para software ou hardware existente.

## Resumo histórico

### Origens dos descompiladores

Na década de 1960, a maioria dos programas era escrita em linguagem de montagem (assembly). Com o surgimento de novas arquiteturas, era comum precisar reescrever programas inteiros para que funcionassem em outras máquinas. Foi nesse cenário que o professor Maurice Halstead, conhecido como o "Pai da Descompilação", desenvolveu o projeto D-Neliac, capaz de converter imagens binárias em código assembly, reduzindo em até 90% o esforço de migração.

Nas décadas seguintes, os descompiladores passaram a ser usados para portabilidade de programas, documentação, depuração, recriação de código-fonte perdido e modificação de binários. A partir da década de 1990, essas ferramentas consolidaram-se na engenharia reversa para tarefas como:

* Identificar código malicioso em programas.
* Validar o funcionamento de compiladores.
* Traduzir binários entre arquiteturas.
* Entender implementações específicas em bibliotecas.

## Grafo de Fluxo de Controle

A Dra. Cristina Cifuentes é uma das pioneiras na área, com sua dissertação de 1994, *Reverse Compilation Techniques*. Ela destaca que, mesmo após a desmontagem de um programa e sua representação em um Grafo de Fluxo de Controle (GFC), ainda resta muito trabalho:

> “O grafo de fluxo de controle... não contém informações sobre estruturas de controle de linguagem de alto nível, como if..then..else..while loops. Tal grafo pode ser convertido em um grafo estruturado de linguagem de alto nível por meio de um algoritmo de estruturação.”

Um algoritmo de estruturação transforma o CFG (Control Flow Graph) em estruturas de alto nível, identificando padrões conhecidos no grafo, chamados de esquemas. A estruturação é feita de forma progressiva, de baixo para cima, até que todos os padrões sejam reconhecidos. Quando isso não é possível, o uso do `goto` permite preservar o fluxo do programa, mesmo que com legibilidade reduzida.

Por exemplo, considere o seguinte CFG com cinco nós:

![CFG](/Imagens/cfg1.png)

A estrutura `B–E` representa um padrão clássico `if-then-else`, com `y` como condição. No entanto, uma aresta de `A–C` interfere nesse padrão. Como não se pode entrar diretamente no escopo de um `if`, a solução é inserir um `goto`. O resultado possível em C seria:

```C++
A();
if (x)
    goto C;
B();

if (y) {
    C:
    C();
} else {
    D();
}
E();
```

Há diversas maneiras de converter esse grafo em código C. A escolha de qual aresta se torna `goto`, qual condição é invertida e como os blocos são reduzidos afeta diretamente a legibilidade. Em tempo de execução, não é possível garantir qual versão (se alguma) corresponde ao código original.

A dissertação de Cifuentes estabelece três pilares da descompilação:

1. Recuperação do CFG por desmontagem.
2. Recuperação de variáveis (com inferência de tipos).
3. Estruturação do fluxo de controle.

Embora ainda pouco exploradas em 1994, essas ideias foram fundamentais para descompiladores modernos.

## O trabalho acadêmico

Em 2011, a Carnegie Mellon retomou o estudo acadêmico sobre descompiladores com o trabalho *TIE*, focado na recuperação de tipos e variáveis. Em 2013, a mesma equipe lançou o artigo conhecido como *Phoenix*, o primeiro sobre estruturação de fluxo de controle publicado em conferências de ponta como USENIX Security e CCS.

O Phoenix trouxe refinamentos importantes:

* Loops, switches e condições melhor estruturados.
* Redução inteligente do uso de `goto`.

Além disso, estabeleceu três princípios:

1. Quando não há correspondência com um esquema, insere-se um `goto` (chamado de **virtualização**).
2. Gotos são indesejáveis e devem ser minimizados.
3. O conjunto Coreutils é ideal para avaliar descompiladores.

## Técnicas modernas em CFG

### DREAM

Em 2015, o artigo *DREAM* apresentou um algoritmo capaz de descompilar com **zero gotos**, um avanço considerável em relação ao Phoenix, que produzia mais de 4.000 gotos nos testes com Coreutils.

DREAM abandona esquemas e opera diretamente com expressões condicionais para preservar a semântica. No exemplo do CFG anterior, sua primeira versão do código seria:

```C++
A();
if (~x)
  B();
if ((~x && y) || x)
  C();
if (~x && ~y)
  D();
E();
```

Após simplificação:

```C++
A();
if (~x)
  B();
if (x || y) {
  C();
} else {
  D();
}
E();
```

### rev.ng

Em 2020, o descompilador comercial rev.ng foi apresentado com uma abordagem híbrida: utilizava algoritmos baseados em esquemas (como Phoenix), mas os corrigia previamente com o algoritmo **Combing**, que duplica trechos de código para permitir estruturações mais limpas, sem `goto`.

Por exemplo:

![CFG rev.ng](/Imagens/cfg_rev.png)

Se antes havia uma única instância do bloco `C`, após o Combing temos duas — uma para cada caminho no grafo. Isso permite uma estrutura em forma de losangos aninhados, mais próxima de uma árvore `if-else`.

## Limitações

O DREAM apresenta dois problemas principais:

1. A simplificação de expressões booleanas é **NP-difícil**.
2. A eliminação de todos os `goto` pode remover até os que estavam no código original.

Exemplo:

```C
if (!v2 && !a0->field_34 && a0->field_38 >= 0 && (a0->field_30 & 0xf000) == 0x1000){
    a0->field_38 = -1;
    a0->field_34 = 1;
}
if (a0->field_38 < 0 || v2 || a0->field_34 || (a0->field_30 & 0xf000) != 0x1000)
    v1 += 1;
```

A quantidade de operadores booleanos gerados por DREAM (9.600) é muito maior que a do Phoenix (342) e do código-fonte original (1.256).

Já o rev.ng, ao duplicar código, pode sofrer com crescimento exponencial, além de também se distanciar do código-fonte original.

## SAILR

Depois de entender os avanços fundamentais apresentados por Phoenix, DREAM e rev.ng, talvez ainda reste uma dúvida importante:

**Por que os gotos existem na descompilação?**
E mais: **a redução de gotos é realmente a melhor forma de medir a qualidade de um descompilador?**

Essas perguntas motivaram o trabalho de Zion Basque, publicado em 2024, intitulado *"Ahoy SAILR! There is No Need to DREAM of C: A Compiler-Aware Structuring Algorithm for Binary Decompilation"*, conhecido como **SAILR**.

A principal descoberta do SAILR é que a grande maioria dos gotos gerados na descompilação se deve a apenas **nove otimizações específicas realizadas pelo compilador**, em sua maioria ativadas no nível de otimização `-O2`. Para qualquer descompilador experiente, essa constatação muda o jogo: ela sugere que a estruturação baseada apenas na redutibilidade dos grafos de controle **não é suficiente** — o caminho mais fiel ao código-fonte é **reverter essas otimizações de forma precisa**.

Essa descoberta é crucial, porque até então, descompiladores vinham tentando eliminar gotos **sem compreender sua origem exata**. Isso levava a efeitos colaterais, como:

* **DREAM**: reduzia gotos, mas à custa da duplicação de condições.
* **rev.ng**: reduzia gotos, mas à custa da duplicação de blocos inteiros de código.
* **Phoenix**: evitava alterações artificiais, mas deixava gotos que não existiam no código-fonte original.

Com isso em mente, o SAILR atuou em duas frentes. Implementou a reversão precisa das **nove otimizações** responsáveis pelos gotos, restaurando a estrutura original do código. E propôs um novo método de avaliação: o **CFGED** (*Control-Flow Graph Edit Distance*), uma métrica baseada em distância de edição entre grafos de controle, que permite comparar a saída de um descompilador com o código-fonte. Diferente da contagem bruta de gotos, o CFGED aceita a presença de gotos, desde que a estrutura geral corresponda ao código original.

Em resumo, o SAILR defende três ideias centrais:

1. **Os gotos na descompilação são causados por otimizações do compilador — e muitas delas podem ser revertidas.**
2. **O CFGED é uma métrica mais justa e precisa para avaliar a qualidade da descompilação.**
3. **Nem todo goto é ruim — alguns são inevitáveis ou até necessários para preservar a semântica.**

## Fontes

* [https://www.program-transformation.org/Transform/HistoryOfDecompilation1.html](https://www.program-transformation.org/Transform/HistoryOfDecompilation1.html)
* [http://www2.ic.uff.br/\~otton/graduacao/informaticaI/apresentacoes/eng\_reversa.pdf](http://www2.ic.uff.br/~otton/graduacao/informaticaI/apresentacoes/eng_reversa.pdf)
* [https://mahaloz.re/](https://mahaloz.re/)
