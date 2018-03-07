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

Agora, nós definimos Erlang como uma linguagem de programação funcional, mas ele também tem uma alta ênfase em concorrência e alta confiabilidade. Para ser possível ter dezenas de tarefas sendo executadas no mesmo tempo, Erlang usa o modelo de atores, onde cada ator é um processo separado na máquina virtual. Em resumo, se você é um ator no mundo de Erlang, você será uma pessoa sozinha sentada numa sala escura sem janelas, esperando uma mensagem na sua caixa de entrada. Quando você obtem a mensagem, você reage à ela de uma maneira específica: você paga as taxas quando você as recebe, você responde à um Cartão de Aniversário com um "Muito obrigado!" e ignore as cartas que você não entende.

Os modelo de atores de Erlang podem ser imaginados como um mundo onde todo mundo está sentado sozinho na sua própria sala e pode executar poucas tarefas distintas. Todo mundo se comunica estritamente escrevendo cartas, e só isso. Enquanto isso soa como uma vida entediante (e uma nova era para o serviço postal), isso significa que você pode pedir para várias pessoas executarem tarefas muito específicas por você, e nenhuma delas jamais fará algo errado ou cometer algum engano que terá repercusão no trabalho dos outros; eles também sequer saber da existência de outras pessoas além de você (e isso é legal).

Para sair dessa analogia, Erlang te força a escrever atores (processos) que não compartilharão informações entre eles, a menos que passem mensagem uns aos outros. Toda a comunicação é explicita, rastreável e segura.

Quando definimos Erlang, fizemos no nível de linguagem, mas num sentido mais amplo, isso não é tudo que existe: Erlang é também um ambiente de desenvolvimento como um todo. O código é compilado para _bytecode_ e é executado dentro de uma máquina virtual. Então, Erlang é como Java e crianças com deficit de atenção, podem rodar/correr em qualquer lugar. A distribuição padrão inclui (entre outros) ferramentas para desenvolvimento (compilador, debugger, profiler e frameworks para testes), o framework Open Telecom Platform (OTP), um servidor web, um gerador de parsers, e a base de dados Mnesia, um sistema de armazenamento baseado em _chave/valor_ que pode se replicar em vários servidores, suportando transações aninhadas e deixando você armazenar qualquer tipo de dados Erlang.

A VM e as bibliotecas também permitem que você atualize o código em um sistema que esteja em execução sem interromper nenhum programa, distribuir seu código com facilidade para vários computadores e gerenciar erros e falhas de uma maneira simples, porém poderosa.

Nós veremos como usar a maioria dessas ferramentas e alcançar a segurança mais tarde, mas agora, Eu vou te dizer sobre uma regra geral em Erlang: deixe quebrar. Não como um avião com dezenas de passageiros morrendo, mas como alguém que ande na corda bamba com uma rede de segurança abaixo dele. Embora você evite cometer erros, você não precisará verificar por todos os tipos ou condições de erro na maioria dos casos.

A capacidade de Erlang de se recuperar de falha, organizar código com atores torná-lo dimensionado com distribuição e concorrência parece incrível, o que nos leva até a próxima seção...