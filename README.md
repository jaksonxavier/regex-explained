# Reg(exp){2}lained

---

## Sumário

1. [Introdução](#-expressões-regulares)
2. [História](#-história)
3. [Terminologia](#-terminologia)
4. [Quando usar e para que server](#-quando-usar-e-para-que-serve)
5. [Metacaracteres](#-metacaracteres)
6. [Gulodice X Moderação](#-gulodice-x-moderação)
7. [Precedência entre metacaracteres](#-precedência-entre-metacaracteres)
8. [Modificadores](#-modificadores)
9. [Localidade do sistema e caracteres acentuados](#-localidade-do-sistema-e-caracteres-acentuados)
10. [Regex com JavaScript](#-regex-com-JavaScript)
11. [Boas práticas](#-boas-práticas)
12. [Referencias](#-referencias)

---

## Expressões regulares

É uma junção de símbolos ( metacaracteres com funções especiais ) e caracteres literais formando um padrão ( uma regra textual ) para ser interpretado ( por linguagens de programação ou editores de texto ) e usado para validar entrada de dados ou fazer buscas e extração de informações em textos.

> - Podemos fazer uma analogia com o cubo mágico onde para resolve-lo precisamos primeiro entender seu funcionamento e como encaixar cada peça no local correto, no começo parece complexo, mas quando desvendamos os padrões e movimentos a solução vem espontaneamente.

```javascript
// example of using regex in javascript
const pattern = `Em suas pesquisas pela web você pode depara-se com as mais
variadas formas de referênciar o termo Expressão Regular como: Regular Expression,
RegExp, Regex, ...etc.`;

const regex = /Reg(?:[Ee]xp?|ular Expression)/gm;

const matches = pattern.match(regex);

console.log(matches);
// expected output: [ 'Regular Expression', 'RegExp', 'Regex' ]
```

---

## História

Inspirado pelo funcionamento de seus próprios neurônios, o termo deriva do trabalho do matemático norte-americano Stephen Cole Kleene, que desenvolveu as expressões regulares como uma notação ao que ele chamava de álgebra de conjuntos regulares. Seu trabalho serviu de base para os primeiros algoritmos computacionais de busca, e depois para algumas das mais antigas ferramentas de tratamento de texto da plataforma Unix.

O uso atual de expressões regulares inclui **procura e substituição de texto**, **validação de formatos de texto**, **validação de protocolos ou formatos digitais**, **realce de sintaxe** e **filtragem de informação**.

---

## Terminologia

Ao ingressar no mundo das expressões regulares, é comum esbarrar com nomeclaturas até então desconhecidas para o leitor, por isso, é importante familiarizar-se com alguns desses termos previamente.

- **Metacharacter/Metacaractere** - Caracteres que tem um significado especial, sua função pode mudar dependendo do contexto no qual está inserido.
- **Macth/Casar** - Ato de bater, combinar, encontrar, encaixar, equiparar.
- **Pattern/Padrão** - É o objetivo quando fazemos uma regex, casar um padrão. Esse padrão pode ser uma palavra, várias, uma linha vazia, um número, `...etc`.
- **Compiler/Compilador** ( ou interpretador ) - O que vai ler, checar, entender e aplicar sua regex no texto desejado.

---

## Quando usar e para que serve

Basicamente servem para você dizer algo abrangente de forma específica, definindo seu padrão de busca, você tem uma lista ( finita ou não ) de possibilidades de casamento.

Em um exemplo rápido, `/[gpr]ato/` pode casar **gato**, **pato** e **rato**. Ou seja, sua lista "abrange especificamente" essas três palavras, nada mais.

Na prática, as expressões regulares servem para uma infinidade de tarefas. São úteis para buscar e validar um padrão de texto, que pode ser variável, como: data, horário, rg, cpf, email, `...etc`.

---

## Metacaracteres

É um caractere ( ou sequência de caracteres ) que tem um significado especial para o compilador, sua função pode mudar dependendo do contexto no qual está inserido, e podemos agregá-los uns com os outros, combinando suas funções e fazendo construções mais complexas. Eles estão divididos em quatro grupos distintos, de acordo com características comuns.

### Representates

São metacaracteres cuja função é representar um ou mais caracteres.

<table >
  <thead>
    <tr>
      <th >Metacaractere</th>
      <th >Nome</th>
      <th >Função</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td >.</td>
      <td >Ponto</td>
      <td >Um caractere qualquer</td>
    </tr>
    <tr>
      <td >[...]</td>
      <td >Lista</td>
      <td >Lista de caracteres permitidos</td>
    </tr>
    <tr>
      <td >[^...]</td>
      <td >Lista negada</td>
      <td >Lista de caracteres proibidos
</td>
    </tr>
  </tbody>
</table>

#### Ponto - `/ponto./`

O ponto casa qualquer caractere, pode ser uma letra, um número, um tab `...etc`.

```javascript
// example of using metacharacter dot

const clockSound = "tic-tac!, tic, tac, tic! tac. dim! drim!";

const regex = /t.c./g;

const matches = clockSound.match(regex);

console.log(matches);
// expected output: [ 'tic-', 'tac!', 'tic,', 'tac,', 'tic!', 'tac.' ]
```

Exemplo de funcionamento do metacaractere `.` ( ponto ), casando com os caracteres `-` ( hífen ), `!` ( exclamação ), `,` ( vírgula) , `.` ( ponto ).

> - O metacaracte ponto casa, entre outros, o caractere ponto.
> - O metacaracte `.` ( ponto ) não casa o caracter `\n` ( nova linha ).

#### Classes de caracteres ( Listas ) - `/[xyz]/`

Pesquisa correspondência para qualquer um dos caracteres entre colchetes. Caracteres especiais ( como o ponto `.` e o asterisco `*` ) são interpretados como caracteres literais, desse modo não é necessária a utilização de escape, mas se utiliza-lo também irá funcionar.

Por padrão expressões regulares são case sensitive; significa que caracteres em caixa alta e em caixa baixa são tratados de modo diferente. Por exemplo, as palavras `checar` e `CHECAR` são consideradas diferentes.

```javascript
// example of using list characters

const pattern = "Garfilde o gato, Jerry o rato e Donald o pato!";

const regex = /[gpr]ato/g;

const matches = pattern.match(regex);

console.log(matches);
// expected output: [ 'gato', 'rato', 'pato' ]
```

> - Não é necessária a utilização do escape dentro da lista, salvo os caracteres de colchetes.
> - Para representar `[]` ( colchetes ) dentro de uma lista, devem-se ser escapados `[\[\]]`.
> - Expressões regulares são case sensitive.

##### Intervalos em listas

Os colchetes podem conter intervalos ( ranges ), que são especificados usando o caractere hífen. Por exemplo, `[a-z]` é um intervalo de caracteres de 'a' até 'z', e `[0-9]` é um dígito de '0' até '9'.

Por exemplo, `/[a-f]/` abrange especificamente os caracteres 'a', 'b', 'c', 'd', 'e', 'f'.

> - Os intervalos respeitam a ordem numérica da [tabela ASCII](https://www.ascii-code.com/).
> - Se houver um `-` ( hífen ) entre dois caracteres, representa todo intervalo entre eles.
> - Para representar um hífen literal, coloca-se o no final da lista.

#### Classes de caracteres negada ( Lista negada ) - `/[^xyz]/`

Digitar um circunflexo após o colchete de abertura nega a classe de caractere. O resultado é que a expressão casará com qualquer caractere fora os listados. Ao contrário do metacaractere ponto, as classes de caracteres negados também correspondem aos caracteres de quebra de linha. Se você não deseja incluí-los, é precisa especificar em sua regex, tal como `/[^0-9\r\n]/` corresponde a qualquer caractere que não seja um dígito ou uma quebra de linha.

> - Todas as regras da lista normal aplicam-se a lista negada também.
> - Lista negada deve corresponder a um caractere.
> - Lista negada casa com o caractere `\n` ( nova linha ).

### Quantificadores

Servem para indicar o número de repetições permitidas para a entidade imediatamente anterior. Entidade pode ser um caractere ou metacaractere.

<table >
  <thead>
    <tr>
      <th >Metacaractere</th>
      <th >Nome</th>
      <th >Função</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td >?</td>
      <td >Opcional</td>
      <td >Zero ou um</td>
    </tr>
    <tr>
      <td >*</td>
      <td >Asterisco</td>
      <td >Zero, um ou mais</td>
    </tr>
    <tr>
      <td >+</td>
      <td >Mais</td>
      <td >Um ou mais</td>
    </tr>
    <tr>
      <td >{n,m}</td>
      <td >Chaves</td>
      <td >De n até m</td>
    </tr>
  </tbody>
</table>

> - Os quantificadores não são quantificáveis então dois deles seguidos em uma regex é um erro, [salvo quando quantificadores não gulosos](#-gulodice-x-moderação).
> - [Todos os quantificadores são gulosos](#-gulodice-x-moderação)

#### Opcional - `/.?/`

Corresponde a expressão que o precede repetido 0 ou 1 vez. Muito útil quando estamos procurando palavras que não sabemos se está ou não no plural. Na regex `/codes?/`, podemos casar com as palavras `code` e `codes`;

> - Cada caractere é um atomo da regex, então o opcional é aplicado somente ao caractere precedido.
> - Podemos tornar opcional caracteres, metacaracteres e classes de caracteres.

#### Asterisco - `/.*/`

A entidade anterior pode ocorrer, não ocorrer ou ocorrer várias ( infinitas ) vezes.

> - Quantificadores por padrão tentam casar o máximo de caracteres possiveis.
> - Grupos de caracteres são quantificáveis.

#### Mais - `/.+/`

Possui o funcionamento idêntico ao `*` ( asterisco ), todas suas regras também aplicam-se ao metacaractere `+` ( mais ). Sua única diferenca é que mais não é opcional, então a entidade anterior deve casar pelo menos uma vez.

> - Corresponde a entidade anterior, repetida uma ou mais vezes.
> - A entidade anterior deve casar pelo menos uma vez.

#### Chaves - `/.{n,m}/`

Pesquisa a `n` menor correspondência e a `m` maior correspondência do caractere precedido. Onde, `n` e `m` devem ser inteiros positivos, quando `n` é zero, ele poderá ser omitido, quando `m` é omitido, torna-se indeterminado, ou seja, possui apenas quantidade minima de casamento.

> - Com as chaves, conseguimos reproduzir o funcionamento dos metacaracteres mais, opcional e asterisco.
> - As chaves são a solucão para uma quantificação mais controlada.
> - Com as chaves podemos espeficar exatemente a quantidade de repetições desejada `{n}` ou um range `{n,m}`, finito ou não de possibilidades.

```javascript
// example of using quantifiers metacharacters

const beeSound = "bu buz buzz buzzz buzzzzzz!";

const quantifierAsterisk = /buz*/g;
const quantifierPlus = /buz+/g;
const quantifierKeys = /buz{4,6}/g;

const matchesAsterisk = beeSound.match(quantifierAsterisk);
const matchesPlus = beeSound.match(quantifierPlus);
const matchesKeys = beeSound.match(quantifierKeys);

console.log(matchesAsterisk);
// expected output: [ 'bu', 'buz', 'buzz', 'buzzz', 'buzzzzzz' ]

console.log(matchesPlus);
// expected output: [ 'buz', 'buzz', 'buzzz', 'buzzzzzz' ]

console.log(matchesKeys);
// expected output: [ 'buzzzzzz' ]
```

### Âncoras

Estabelecem posições de referência para o casamento do restante da regex.

> - Quantificadores não tem influência sobre as âncoras.
> - Metacaracteres do tipo âncora não casam com caracteres no texto, mas sim com posições antes, depois ou entre os caracteres.

<table >
  <thead>
    <tr>
      <th >Metacaractere</th>
      <th >Nome</th>
      <th >Função</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td >^</td>
      <td >Circunflexo</td>
      <td >Início da linha</td>
    </tr>
    <tr>
      <td >$</td>
      <td >Cifrão</td>
      <td >Fim da linha</td>
    </tr>
    <tr>
      <td >\b</td>
      <td >Borda</td>
      <td >Início ou fim de palavra</td>
    </tr>
  </tbody>
</table>

#### Circunflexo - `/^circunflexo/`

Corresponde ao início de uma linha. Por exemplo `/^Sanji/`, casa com 'Sanji' em "Sanji, quero comida", mas não caso com 'Sanji' em "Quero comida Sanji".

> - Serve para procurar palavras no início da linha.
> - Só é interpretado como metacaractere no início da regex ( e de uma lista ).

#### Cifrão - `/cifrão$/`

Corresponde ao final de uma linha. Então a regex `/Sanji$/` casa com 'Sanji' em "Quero comida Sanji", mas não casa com 'Sanji' em "Sanji, quero comida".

> - Serve para procurar palavras no final da linha.
> - Só é interpretado como metacaractere no final da regex.

#### Borda - `/\bborda\b/`

Pesquisa correspondência em uma fronteira de caractere. Uma fronteira de caractere corresponde a posição onde um caractere/palavra não é seguido ou antecedido por outro caractere/palavra.

```javascript
// example of using
const pattern = "feliz, felizmente, infelizmente";
// start the word
const findStartWord = /\bfeliz/g;

pattern.match(findStartWord);
// end the word
const findEndWord = /\bmente/g;

// exactly the word
const findWord = /\bfeliz\b/g;
```

> - Entende-se por palavra, a sequência dos caracteres `[A-Za-z0-9_]`.

### Outros

São metacaracteres que têm funções específicas e não relacionadas entre si.

<table >
  <thead>
    <tr>
      <th >Metacaractere</th>
      <th >Nome</th>
      <th >Função</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td >\*</td>
      <td >Escape</td>
      <td >Torna literal o caractere '*'</td>
    </tr>
    <tr>
      <td >x|y</td>
      <td >Ou</td>
      <td >Pesquisa correspondência em 'x' ou 'y'</td>
    </tr>
    <tr>
      <td >(x)</td>
      <td >Grupo</td>
      <td >Delimita um grupo</td>
    </tr>
    <tr>
      <td >(?:x)</td>
      <td >Non-capturing group</td>
      <td >Não memoriza o texto casado no grupo</td>
    </tr>
    <tr>
      <td >x (?=y)</td>
      <td >Positive Lookahead</td>
      <td >Pesquisa a correspondência em 'x' apenas se 'x' é seguido por 'y'</td>
    </tr>
    <tr>
      <td >x (?!y)</td>
      <td >Negative Lookahead</td>
      <td >Pesquisa a correspondência em 'x' apenas se 'x' não é seguido por 'y'</td>
    </tr>
    <tr>
      <td >(?&lt;=x) y</td>
      <td >Positive Lookbehind</td>
      <td >Pesquisa a correspondência em 'x' apenas se 'x' é precido por 'y'</td>
    </tr>
    <tr>
      <td >(?&lt;!x) y</td>
      <td >Negative Lookbehind</td>
      <td >Pesquisa a correspondência em 'x' apenas se 'x' não é precido por 'y'</td>
    </tr>
    <tr>
      <td >(?&lt;name&gt;x)</td>
      <td >Grupo nomeado</td>
      <td >Corresponde a 'x' e armazena-o na prorpriedade groups da correspondência retornada com o nome especificado &lt;name&gt;</td>
    </tr>
    <tr>
      <td >\1, \2 ... \9</td>
      <td >Retrovisor</td>
      <td >Texto casado nos grupos 1, 2 ... 9</td>
    </tr>
    <tr>
      <td >\k&lt;name&gt;</td>
      <td >Retrovisor de grupo nomeado</td>
      <td >Texto casado no grupo 'name'</td>
    </tr>
  </tbody>
</table>

#### Escape - `/\./`

Uma barra invertida que preceda um caractere não especial significa que o caractere seguinte é especial e não deve ser interpretado de forma literal. Por exemplo, o caractere 'b' quando não precedido de uma barra invertida significará uma ocorrência do próprio caractere 'b' minúsculo, porém se precedido da barra invertida `\b` ele passará a significar a ocorrência do metacaractere borda ( fronteira do caractere ).

Quando a barra invertida preceder um metacaractere isso significará que o próximo caractere deve ser interpretado de forma literal. Por exemplo o padrão `/a*/`, que selecionará a ocorrência de zero ou mais caracteres 'a' quando utilizado sem a `\` para escape. Por outro lado no padrão `/a\*/` o asterisco deixa de ter seu significado especial, pois a `\` de escape fará com que o `\*` seja interpretado de forma literal, passando o padrão a selecionar o caractere 'a' seguido do caractere '\*'.

#### Ou - `/x|y/`

Pesquisa correspondência em 'x' ou 'y'. Por exemplo, `/Luffy|Garp/` encontra 'Luffy' em "Monkey D. Luffy" e 'Garp' em "Monkey D. Garp".

#### Grupo - `/(x)/`

Agrupa metacaracteres, caracteres e até outros grupos, para efeito de aplicação de quantificador, alternativa ou de posterior extração ou reúso.

> - O grupo não altera o sentido da regex.
> - Grupos podem conter outros grupos.
> - Grupos são quantificáveis.
> - Os parênteses são chamados parênteses de captura.

#### No-capturing group - `/(?:x)/`

Corresponde a 'x', mas não se lembra da correspondência. A substring correspondida não pode ser recuperada dos elementos do array resultante `([1], ..., [n])` ou das propriedades do objeto RegExp predefinido `($1, ..., $9)`.

#### Positive Lookahead - `/x (?=y)/`

Corresponde a 'x' apenas se 'x' for seguido por 'y'. Por exemplo, `/Edward (?=Newgate)/` corresponde a 'Edward' apenas se for seguido por 'Newgate'.
`/Edward (?=Newgate|Weevil)/` corresponde a 'Edward' apenas se for seguido por 'Newgate' ou 'Weevil'. No entanto, nem 'Newgate' nem 'Weevil' fazem parte do match.

#### Negative Lookahead - `/x (?!y)/`

Corresponde a 'x' apenas se 'x' não for seguido por 'y'. Por exemplo, `Edward (?!Weevil)/` corresponde a 'Edward' em "Edward Newgate", mas não corresponde a 'Edward' em "Edward Weevil".

#### Positive Lookbehind - `/(?<=x) y/`

Corresponde a 'x' apenas se 'x' for precedido por 'y'. Por exemplo, `/(?<=John) Wick/` corresponde a 'Wick' apenas se for precedido por 'John'. `/(?<=Sr.|John) Wick/` corresponde a 'Wick' apenas se for precedido por 'Sr.' ou 'John'. No entanto, nem 'Sr.' nem 'John' fazem parte do match.

#### Negative Lookbehind - `/(?<!x) y/`

Corresponde a 'x' apenas se 'x' não for precedido por 'y'. Por exemplo, `/(?<!Insp.) Frank Abagnale/` corresponde a "Frank Abagnale" apenas se não for precedido por um 'Inps.', `/(?<!Insp.|Dr.) Frank Abagnale/` corresponde a "Frank Abagnale" apenas se não for precedido por 'Inps.' ou 'Dr.'.

#### Grupo nomeado - `/(?<name>x)/`

Corresponde a 'x' e o armazena na propriedade `groups` das correspondências retornadas sob o nome especificado por `<name>`. Os `<>` ( colchetes ) são necessários para o nome do grupo.

Por exemplo, para extrair o código de área de um número de telefone, poderíamos usar `/\((?<area>[0-9]{2,3})\)/`. O resultado apareceria em `match.groups.area`.

> - Todas as regras do grupo, também aplicam-se ao grupo nomeado.

#### Retrovisor - `/(grupo) \1/`

Faz referência a substring pertencente à um grupo, recuperando o texto casado no `n`-ésimo grupo, onde `n` é um inteiro positivo de '1' até '9'.

> - O retrovisor só funciona se usado com o grupo.
> - O retrovisor referencia o texto casado e não a regex do grupo.
> - Temos no máximo 9 retrovisores por regex.
> - Numeram-se retrovisores contando os grupos da esquerda para a direita.

#### Retrovisor de grupo nomeado - `/(?<name>grupo) \k<name>/`

Uma referência anterior à última substring que corresponde ao grupo de captura nomeado, especificado por `<name>`.

> - Referencia o texto casado e não a regex do grupo.
> - Só funciona se usado com o grupo nomeado.
> - `\k` é usado para indicar o início de uma referência anterior a um grupo de captura nomeado.

### Metacaracteres tipo barra-letra

São átomos representados por uma barra invertida `\` seguida de uma letra qualquer: `\d`, `\w`, `\s`. Dependendo da letra, muda-se o significado desse metacaractere.

<table >
  <thead>
    <tr>
      <th >Metacaractere</th>
      <th >Função</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td >[\b]</td>
      <td >Pesquisa correspondência com espaço em branco. É preciso utilizar os colchetes se você quer encontrar um espaço em branco. (Não confunda-o com \b.)</td>
    </tr>
    <tr>
      <td >\c<i>X</i></td>
      <td >Onde X é um caractere pertencente ao conjunto A-Z. Encontra correspondência de um caractere de controle em uma string.<br />Por exemplo, /\cM/ encontra correspondente control-M em uma string.</td>
    </tr>
    <tr>
        <td >\d</td>
        <td >Encontra correspondência com um número. Equivalente a [0-9].</td>
    </tr>
    <tr>
        <td >\f</td>
        <td >Encontra correspondência com um caractere de escape avanço de página.</td>
    </tr>
    <tr>
        <td >\n</td>
        <td >Encontra correspondência com um caractere de escape quebra de linha.</td>
    </tr>
    <tr>
        <td >\r</td>
        <td >Encontra correspondência com um caractere de escape retorno de carro.</td>
    </tr>
    <tr>
        <td >\s</td>
        <td >Encontra correspondência com um único caractere de espaço em branco, espaço, tabulação, avanço de página, quebra de linha.</td>
    </tr>
    <tr>
        <td >\t</td>
        <td >Encontra correspondência em uma tabulação.</td>
    </tr>
    <tr>
        <td >\v</td>
        <td >Encontra correspondência em uma tabulação vertical.</td>
    </tr>
    <tr>
        <td >\w</td>
        <td >Encontra correspondência de qualquer caractere alfanumérico incluindo underline. Equivalente a [A-Za-z0-9_].</td>
    </tr>
    <tr>
        <td >\0</td>
        <td >Encontra correspondência em um caractere NULL. Não adicione outro número após o zero, pois \0<digitos> é um escape para número octal.</td>
    </tr>
    <tr>
        <td >\x<i>hh</i></td>
        <td >Encontra correspondência com o código hh (dois valores hexadecimal).</td>
    </tr>
    <tr>
        <td >\u{hhhh}</td>
        <td >Encontra correspondência com o valor Unicode hhhh (dígitos hexadecimais).</td>
    </tr>
    <tr>
        <td >\p{UnicodeProperty}</td>
        <td ></td>
    </tr>
  </tbody>
</table>

### Negação metacaracteres tipo barra-letra

Geralmente um caractere `barra-LETRA` é a negação de um `barra-letra`. Por exemplo `/\D/` é a negação de `/\d/`, casando assim qualquer caractere exeto números.

<table >
  <thead>
    <tr>
      <th >Metacaractere</th>
      <th >Função</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td >\B</td>
      <td >Pesquisa correspondência  que não seja em uma fronteira de caractere. Para a correspondência é associada uma posição onde o caractere anterior e o próximo tem as mesmas características: ambos são caractere/palavra, ou ambos não sejam caractere/palavra. O início e o fim de uma string não considerados como não caractere/palavra.</td>
    </tr>
    <tr>
      <td >\D</td>
      <td >Encontra correspondência com um caractere que não seja número. Equivalente a [^0-9].</td>
    </tr>
    <tr>
      <td >\S</td>
      <td >Encontra correspondência em um único caractere que não seja espaço em branco.</td>
    </tr>
    <tr>
      <td >\W</td>
      <td >Encontra correspondência em um não caractere. Equivalente a [^A-Za-z0-9_].</td>
    </tr>
    <tr>
      <td >\P{UnicodeProperty}</td>
      <td ></td>
    </tr>

  </tbody>
</table>

---

## Gulodice X Moderação

Por padrão todos os quantificadores são gulosos, ou seja, sempre tentarão casar o máximo de caracteres que conseguirem. Por exemplo, ao tentar remover as tags HTML de um texto, podemos tentar escrever a seguinte expressão:

```javascript
// greedy
const pattern = "<h1>Regexp Explained</h1>";

const regex = /<.*>/g;

const cleanStatement = pattern.replace(regex, "㰠");

console.log(cleanStatement);
// expected output: 㰠
```

Nosso objetivo é pegar o marcadores `<h1>` e `</h1>` e apagalos. Mas ao aplicarmos nossa regex temos um resultado inesperado, que acontece, devido a natureza voraz dos metacaracteres quantificadores.

```javascript

```

Para fazer o efeito contrario no quantificador guloso se acrescenta o metacaractere `?` ( opcional ), assim, ao invés de capturar o máximo, tenta capturar o mínimo.

Em geral se referencia o `*?`, que casa com o máximo, mas tenta capturar o mínimo.

```javascript
// lazy
const pattern = "<h1>Regexp Explained</h1>";

const regex = /<.*?>/g;

const cleanStatement = pattern.replace(regex, "∾");

console.log(cleanStatement);
// expected output: ∾Regexp Explained∾
```

> - Gulodice às vezes é boa, às vezes é ruim.
> - O quantificador guloso é mais rápido, pois ele vai casando até não dar mais, enquanto o não-guloso é mais lento pois verifica sempre se é o mínimo possível.

---

## Precedência entre metacaracteres

---

## Modificadores

Servem para mudar o comportamento padrão de uma expressão, o modificador pode ser uma ou mais letras, sendo que cada letra possui uma funcionalidade diferente.

<table >
  <thead>
    <tr>
      <th >Metacaractere</th>
      <th >Nome</th>
      <th >Função</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td >g</td>
      <td >global</td>
      <td >Procura todas as ocorrências da expressão no texto, ao invés de parar na primeira ocorrência</td>
    </tr>
    <tr>
      <td >m</td>
      <td >multi line</td>
      <td >Procura por ocorrências em múltiplas linhas</td>
    </tr>
    <tr>
      <td >i</td>
      <td >insensitive</td>
      <td >Não leva em consideração maiúsculas e minúsculas (case-insensitive)</td>
    </tr>
    <tr>
      <td >y</td>
      <td >sticky</td>
      <td >Executa a correspondência fixa no texto de destino, ancorando no início do padrão ou no final da correspondência mais recente</td>
    </tr>
    <tr>
      <td >u</td>
      <td >unicode</td>
      <td >Trata o padrão como uma sequência de código unicode</td>
    </tr>
    <tr>
      <td >s</td>
      <td >single line</td>
      <td >Faz o metacaractere . ( ponto ) casar com o caracter \n ( nova linha )</td>
    </tr>
  </tbody>
</table>

---

## Localidade do sistema e caracteres acentuados

### POSIX

### Unicode

```javascript
const text = "";

const normalizedText = text.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
```

---

## Regex com JavaScript

Em JavaScript, expressões regulares também são objetos. Elas podem ser utilizadas com os métodos **exec** e **test** do objeto **RegExp**, e com os métodos **match**, **replace**, **search**, e **split** do objeto **String**.

### Há duas maneiras de construir uma expressão regular com javascript

- Usando uma expressão literal, que consiste em um padrão fechado entre barras ( **parsed regex** ):

```javascript
// validate format

const formattedCPF = "682.809.370-48";
const unformattedCPF = "85548102010";

const regex = /\d{3}.\d{3}.\d{3}-\d{2}/;

console.log(regex.test(formattedCPF));
// true
console.log(regex.test(unformattedCPF));
// false
```

As expressões regulares na forma literal são compiladas quando o script é carregado. Esta forma de construção possui melhor performace quando a expressão regular utilizada é uma constante, ou seja, não muda durante a execução.

- Chamando o construtor do objeto RegExp ( **raw strings** ):

```javascript
// filter text

const formattedCNPJ = "91.262.875/0001-17";
const unformattedCNPJ = "20418581000193";

const regex = new RegExp("\\d{2}.\\d{3}.\\d{3}/\\d{4}-\\d{2}", "g");

console.log(regex.test(formattedCNPJ));
// true
console.log(regex.test(unformattedCNPJ));
// false
```

Usando o construtor, a compilação da expressão regular é realizada em tempo de execução. Use o construtor quando souber que o padrão da expressão regular irá mudar ou quando o padrão for desconhecido ou derivado duma entrada de usuário por exemplo.

> - Ao usar o construtor RegExp sua senteça é primeiro interpretada pelo objeto String e só depois passada para o compilador.
> - A utilizamos essa sintax, temos que tomar alguns cuidados, pois alguns trechos podem ser confundidos com estruturas especiais do javascript, como a interpretação de escapes, metacaracteres barra-letras e caracteres de controle. Nesse caso temos que prever o pre-processamento modificando nossa expressão para casar o texto desejado.

> - Números de documentos gerados por [4devs](https://www.4devs.com.br/)

### Métodos

Expressões Regulares são usadas com os metodos test e exec do objeto RegExp e com os metodos match, replace, search, e split do objeto String.

#### exec

Um método RegExp que execute uma pesquisa por uma correspondência em uma string. Retorna um array de informações.

#### test

Um método RegExp que testa uma correspondência em uma string. Retorna true ou false.

#### match

Um método String que executa uma pesquisa por uma correspondência em uma string. Retorna uma array de informações ou null caso não haja uma correspondência.

#### replace

Um método String que executa uma pesquisa por uma correspondência em uma string, e substitui a substring correspondênte por uma substring de substituição.

#### search

Um método String que testa uma correspondência em uma string. Retorna o indice da correspondência ou -1 se o teste falhar.

#### split

Um método String que usa uma expressão regular ou uma string fixa para quebrar uma string dentro de um array de substrings.

---

## Boas práticas

---

## Referencias

JARGAS, Aurelio Marinho. Expressões Regulares: Uma abordagem divertida. 5. ed. Novatec, 2016.

Mozilla. Regular expressions. MDN Web Docs. 2021. Disponível em: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions). Acesso em: 17 fev. 2021.

Wikipedia. Regular expression. Wikipedia is a free online encyclopedia. 2021. Disponível em: [https://en.wikipedia.org/wiki/Regular_expression](https://en.wikipedia.org/wiki/Regular_expression). Acesso em: 17 fev. 2021.
