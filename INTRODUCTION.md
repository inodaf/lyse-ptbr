# Introdução
### Sobre este tutorial

Este é o começo de Learn You Some Erlang for Great Good! Ler este tutorial deve ser um dos seus seus primeiros passos aprendendo Erlang, então, vamos falar um pouco mais sobre ele.

Primeiramente, a ideia de escrever este livro veio depois de ler o tutorial "Learn You a Haskell for great Good! (LYAH)" de Miran Lipovača. Ele fez um grande trabalho tornando a linguagem atraente e deixando uma experiência de aprendizado amigável. Como eu já o conhecia, perguntei como ele se sentiu sobre eu escrever a versão do tutorial para Erlang. Ele gostou da ideia, ficando um pouco interessado em Erlang.

Então aqui estou eu escrevendo esse tutorial. Claro que possuem outras fontes para minha motivação: Eu particularmente acho o ponto de partida para a linguagem difícil (a internet possui pouca documentação, e de outra forma, você precisa comprar livros), e pensei que a comunidade se beneficiaria de uma guia como o LYAH. E menos importante que isso, eu vi pessoas atribuindo muito ou pouco mérito à Erlang, se baseando algumas vezes em generalizações arrebatadoras. Então são pessoas que claramente acreditam que Erlang não é nada além de um _hype_. Se eu quiser convencê-los do contrário, Eu sei que é muito provável que não leiam esse tutorial em primeiro lugar.

O objetivo desse livro é fornecer uma maneira de aprender Erlang para pessoas que possuem conhecimentos básicos de programação em linguagens imperativas (como C/C++, Java, Python, Ruby, etc.) e podem ou não saber programação funcional (Haskell, Scala, Erlang, Clojure, OCaml...). Eu também quero escrever esse livro da maneira mais honesta, vendendo Erlang pelo o que ele é, trazendo o conhecimento de suas forças e fraquezas.

### Então, o que é Erlang?

Primeiramente, Erlang é uma linguagem de programação funcional. Se você já trabalhou com alguma linguagem imperativa, declarações como `i++` podem ser comuns para você; na programação funcional elas não são permitidas. De fato, alterar o valor de qualquer variável é estritamente proibido! Isso pode soar estranho no princípio, mas se você se lembrar das suas aulas de matemática, é na verdade, como você aprendeu:

```
y = 2
x = y + 3
x = 2 + 3
x = 5
```

Se eu adicionasse o seguinte:
```
x = 5 + 1
x = x
∴ 5 = 6
```

Você teria ficado muito confuso. A programação funcional reconhece isto: Se Eu digo que `x` é igual a `5`, então eu não posso logicamente afirmar que `x` também é `6`! Isso é ser desonesto. Isso é também o porque uma função com o mesmo parâmetro deve sempre retornar o mesmo resultado.

```
x = add_two_to(3) = 5
∴ x = 5
```

Funções que sempre retornam o mesmo resultado para o mesmo parâmetro são chamadas de transparência referencial. É o que nos deixa substituir `add_two_to(3)` com `5`, como o resultado de `3 + 2` sempre será `5`. Isso significa que podemos então fixar dezenas de funções em conjunto para resolver problemas mais complexos tendo certeza de que nada vai quebrar. Lógico e limpo, não é mesmo? Apesar disso, temos um problema:

```
x = today() = 2009/10/22
-- wait a day --
x = today() = 2009/10/23
x = x
∴ 2009/10/22 = 2009/10/23
```

Oh não! Minhas lindas equações! Elas derrepente se tornaram todas erradas! Como minha função pode retornar um resultado diferente a cada dia?

Obviamente, possuem alguns casos que é útil quebrar a transparência referencial. Erlang possui uma abordagem muito pragmática com programação funcional: obedecer seus princípios mais puros (transparência referencial, evitando mutação de dados, etc.), mas se afastar deles quando os problemas do mundo real aparecerem.

Agora, nós definimos Erlang como uma linguagem de programação funcional, mas ele também tem uma alta ênfase em concorrência e alta confiabilidade. Para ser possível ter dezenas de tarefas sendo executadas no mesmo tempo, Erlang usa o modelo de atores, onde cada ator é um processo separado na máquina virtual. Em resumo, se você é um ator no mundo de Erlang, você será uma pessoa sozinha sentada numa sala escura sem janelas, esperando uma mensagem na sua caixa de entrada. Quando você recebe uma mensagem, você reage à ela de uma maneira específica: você paga as contas quando você as recebe, você responde à um cartão de aniversário com um "Muito obrigado!" e ignora as cartas que você não entende.

Os modelo de atores de Erlang podem ser imaginados como um mundo onde todo mundo está sentado sozinho na sua própria sala e pode executar poucas tarefas distintas. Todo mundo se comunica estritamente escrevendo cartas, e só isso. Enquanto isso soa como uma vida entediante (e uma nova era para o serviço postal), isso significa que você pode pedir para várias pessoas executarem tarefas muito específicas por você, e nenhuma delas jamais fará algo errado ou cometer algum engano que terá repercusão no trabalho dos outros; eles também sequer saber da existência de outras pessoas além de você (e isso é legal).

Para sair dessa analogia, Erlang te força a escrever atores (processos) que não compartilharão informações entre eles, a menos que passem mensagem uns aos outros. Toda a comunicação é explicita, rastreável e segura.

Quando definimos Erlang, fizemos no nível de linguagem, mas num sentido mais amplo, isso não é tudo que existe: Erlang é também um ambiente de desenvolvimento como um todo. O código é compilado para _bytecode_ e é executado dentro de uma máquina virtual. Então, Erlang é como Java e crianças com deficit de atenção, podem rodar/correr em qualquer lugar. A distribuição padrão inclui (entre outros) ferramentas para desenvolvimento (compilador, debugger, profiler e frameworks para testes), o framework Open Telecom Platform (OTP), um servidor web, um gerador de parsers, e a base de dados Mnesia, um sistema de armazenamento baseado em _chave/valor_ que pode se replicar em vários servidores, suportando transações aninhadas e deixando você armazenar qualquer tipo de dados Erlang.

A VM e as bibliotecas também permitem que você atualize o código em um sistema que esteja em execução sem interromper nenhum programa, distribuir seu código com facilidade para vários computadores e gerenciar erros e falhas de uma maneira simples, porém poderosa.

Nós veremos como usar a maioria dessas ferramentas e alcançar a segurança mais tarde, mas agora, Eu vou te dizer sobre uma regra geral em Erlang: deixe quebrar. Não como um avião com dezenas de passageiros morrendo, mas como alguém que ande na corda bamba com uma rede de segurança abaixo dele. Embora você evite cometer erros, você não precisará verificar por todos os tipos ou condições de erro na maioria dos casos.

A capacidade de Erlang de se recuperar de falha, organizar código com atores torná-lo dimensionado com distribuição e concorrência parece incrível, o que nos leva até a próxima seção...

### Não beba muito Kool-Aid

Há diversas seções de cor laranja-amarelada nomeadas como essa em torno deste livro (você reconhecerá uma quando ver). Atualmentte Erlang está ganhando muita popularidade devido à palestras zelosas que podem fazer com que as pessoas acreditem que ele é muito mais do que realmente é. Esses avisos estarão lá para te ajudar a manter seus pés no chão se você for um desses aprendizes super entusiasmados.

O primeiro caso disso está relacionado à como Erlang possui habilidades massivas para escalonamento devido à seus leves processos. E isso é verdade, os processos Erlang são muito leves: você pode ter centenas de milhares deles existindo ao mesmo tempo, mas isso não significa que você precisa usá-lo assim apenas porque você pode. Por exemplo, criar um jogo de FPS onde tudo, incluindo as balas, são seus próprios atores é loucura. A única coisa que você vai atirar em um jogo como esse é seu próprio pé. Possui um pequeno custo ao enviar mensagens de um ator para outro, e se você dividir muitas tarefas, as coisas tendem a ficar lentas.

Eu vou abordar isso com mais detalhes quando estivermos longe o bastante com nosso aprendizado para se preocupar com isso depois, mas mantenha em mente que lançar aleatóriamente o paralelismo em um problema não é o suficiente para torná-lo rápido. Não fique triste; há momentos que usar milhares de processos será mutuamente possível e útil! Só não ocorrerá com frequência.

É dito também que Erlang tem o poder de escalar de forma proporcional de acordo com a quantidade de _cores_ disponíveis no seu computador, mas isso geralmente não é verdade: é possível, mas a maioria dos problemas não se comportam de uma maneira que permite que você execute tudo ao mesmo tempo.

Outra coisa para deixar em mente: enquanto Erlang executa muito bem algumas coisas, é técnicamente possível obter o mesmo resultado a partir de outras linguagens. O oposto também é verdade; avalie cada problema dado as necessidades e escolha a ferramenta mais adequada de acordo com o problema abordado. Erlang não é uma bala de prata e particularmente não será bom lidando com processamento de imagens e sinais, drivers para sistemas operacionais e etc. e brilhará quando o assunto é grandes softwares que são usados no servidor (por exemplo: filas, _map-reduce_), fazendo levantamento juntamente com outras linguagens, implementação de protocolo de alto nível, etc. Os middlewares dependeram de você. Você não deve necessáriamente limitar-se às aplicações de Erlang para softwares de servidores; há casos em que as pessoas estão criando coisas inesperadas e surpreendentes com a linguagem. Um exemplo é o IANO, um robo criado pela equipe UNICT, que usou Erlang para lidar com inteligência artificial e ganhar a medalha de prata em 2009 na competição Eurobot. Outro exemplo é o software Wings 3D, um modelador (não um renderizador) 3D de código aberto escrito em Erlang, portanto, multi-plataforma.

### O que você precisa para explorar

Tudo que você precisa para começar é de um editor de texto e do ambiente Erlang. Você pode obter o código fonte e a distribuição para Windows diretamente do [site oficial do Erlang](http://erlang.org/download.html). Não vou entrar muito nos detalhes da instalação, mas para Windows, apenas baixe e execute o arquivo binário. Não se esqueça de adicionar o diretório do Erlang na variável de sistema `PATH` para conseguir o acesso através da linha de comando.

Nas distribuições do Linux baseadas no Debian, você pode instalar o pacote executando `$ apt-get install erlang`. Nas distribuições Fedora (se possuir o 'yum'), você pode fazer o mesmo executando `# yum install erlang`. Entretanto, na maioria das vezes, esses repositórios armazenam versões desatualizadas do pacote Erlang; Usando uma versão desatualizada poderá lhe dar algumas diferenças com o que você verá nesse tutorial e poderá impactar na performance em algumas aplicações. Eu recomendo que você compile a partir do código fonte. Consulte o arquivo `README` dentro do pacote e dê um Google para obter todos os detalhes da instalação de acordo com suas necessidades, ele, de longe fará um trabalho melhor que o meu.

No FreeBSD, possuem muitas opções disponíveis. Se você usa o _portmaster_, você pode executar `portmaster lang/erlang`. Para _ports_ padrão, deverá ser `cd /usr/ports/lang/erlang; make install clean`. Pronto, se você quiser usar o pacote, execute `pkg_add -rv erlang`.

Se você estiver usando macOS, você pode instalar o Erlang com o [Homebrew](https://github.com/Homebrew/homebrew) executando `$ brew install erlang` ou `$ port install erlang` se você prefere usar o [MacPorts](http://macports.org/).

Alternativamente, a _Erlang Solutions Ltd._ oferece pacotes para a maior parte dos Sistemas Operacionais, que geralmente funcionam muito bem (baixe uma distribuição 'Standard').

> Nota: no momento da escrita desse tutorial, estou usando a versão _R13B+_ do Erlang, mas para melhores resultados, você deve usar versões mais recentes.


### Onde conseguir ajuda

Possuem alguns lugares onde você pode consultar. Se você usa Linux, você pode acessar as _páginas man_ para obter uma boa documentação técnica. Erlang tem um módulo _lists_ (que veremos em breve): para obter a documentação atráves do _lists_ digite `$ erl -man lists`.

No Windows, a instalação deve incluir a documentação em HTML. Você pode baixá-la a qualquer momento através do [site oficial do Erlang](http://erlang.org/doc/) ou consultar um dos [sites alternativos](http://erldocs.com/).

Possuem momentos em que obter detalhes técnicos não é o suficiente. Quando isso acontece, Eu recorro à duas fontes principais: a [lista de discussões oficial](http://www.erlang.org/static/doc/mailinglist.html) (você deveria seguir para aprender ainda mais) e no canal [#erlang](irc://irc.freenode.net/erlang) no [irc.freenode.net](freenode.net).

E mais, se você for o tipo de pessoa que busca por 'cookbooks' e 'receitas pré-feitas', [trapexit](http://trapexit.org/) é o lugar que você está procurando. Eles também possuem as listas de discussões como um fórum e uma Wiki geral, que pode ser sempre útil.