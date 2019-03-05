# Começando

### O Shell

Em Erlang, você pode testar a maioria dos seus experimentos em um emulador; ele executará seus scripts quando compilados e implantados, mas ele também deixa você editar seus códigos ao vivo. Para iniciar o Shell no Linux abra um terminal e digite `$ erl`. Se você instalou corretamente, verá um texto como esse:

```bash
Erlang R13B01 (erts-5.7.2) [source] [smp:2:2] [rq:2] [async-threads:0]
[hipe] [kernel-poll:false]

Eshell V5.7.2  (abort with ^G)
```

Parabéns, você está executando o Shell do Erlang!

Para usuários Windows, você pode executar o `erl.exe`, mas ao invés desse, é recomendado usar o `werl.exe`, que você pode encontrar no Menu Iniciar (`Programas > Erlang`). Werl é uma implementação do Erlang Shell exclusiva para Windows, possuindo sua própria janela com barras de rolagem e com suporte a edições via linha de comando (como copiar/colar, que é uma dor quando usamos o Shell `cmd.exe` padrão do Windows). O `erl shell` ainda é necessário se você quiser redirecionar inputs ou outputs padrão, ou usar pipelines.

Seremos capazes de digitar e executar
