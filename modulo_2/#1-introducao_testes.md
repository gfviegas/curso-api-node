# Introdução a Testes
Este capítulo aborda rapidamente conceitos básicos sobre testes. Se você já tem alguma experiência em testes, sinta-se a vontade em pular este tópico.

## Tipos de Testes
Além de saber a importância dos testes no desenvolvimento, também precisamos saber e aplicar o tipo de teste correto para o nosso software. Existem alguns tipos de testes, como os *unitários*, os *integrados*, os *E2E*, entre outros.

Como vamos desenvolver uma API RESTful, vamos fazer testes de dois tipos: Unitários e Integrados, então vamos lá!

### Testes Unitários
Este tipo de teste tem o objetivo de testar **unidades** do seu programa, ou seja, cada teste consiste em testar apenas uma unidade, testar a menor parte possível do seu código. Essas unidades, na maioria das vezes são *funções*.

#### Responsabilidade dos Testes Unitários
Muitas vezes sua função vai depender de outros módulos, argumentos, etc. Ao se testar essa função, você vai "simular" estas dependências, criando **mocks**, **stubs**, e etc.

Você precisa entender que a responsabilidade do seu teste é testar apenas o comportamento da função a ser testada, e nada além disso.


#### Exemplo de Simulação de Dependências
Então se estamos testando a seguinte função exemplo:
```javascript
const verificarClimaParaEsquiar = (termometro) => {
  if (termometro.getTemperatura() > 20) {
    return 'calor demais pra esquiar!'
  } else {
    return 'tá de boa'
  }
}
```
Essa função *depende* da execução da função `getTemperatura` do termômetro. O termômetro é uma dependência da função `verificarClimaParaEsquiar`.

Nesse caso, não nos importa o comportamento do "termômetro". Nós precisamos nos preocupar apenas em como que a função `verificarClimaParaEsquiar` vai se comportar. E como não podemos mudar a temperatura do ambiente, precisamos "adulterar" esse termômetro pra ver como a função vai se comportar com diferentes temperaturas.

Então, ao testar como comporta essa função quando a temperatura é de 20 graus, por exemplo, ao invés de passar um termômetro mesmo como argumento, passaríamos uma simulação dele, como:
```javascript
const termometroSimulado = {
  getTemperatura: () => {
    return 20
  }
}

const mensagemClima = verificarClimaParaEsquiar(termometroSimulado)

// Esperamos que a mensagemClima seja 'tá de boa'
```

### Testes Integrados
Diferente do teste unitário, que testa apenas um átomo de um sistema, o teste de integração, ou integrados, testa a chamada de diversas funções em sequencia, simulando o comportamento do sistema "na prática".

Por exemplo, sabemos que as função A e B funciona, bem (testamos tudo nelas, glória a Goku!). Mas a função A chama a função B, causando um erro ao ser chamadas ao simular uma situação real e não com os mocks.

No caso de uma API REST, testaremos na pratica, os requests HTTP, mandaremos POST, HEAD, GET, PUT, DELETE, todos os requests registrados, verificar a resposta, validação de dados, e etc. Ou seja, o teste integrado seria uma forma de testar a API como se fossemos um cliente consumidor da API.

#### Exemplos de Testes Integrados
Exemplos:

Ao mandar um POST, o meu recurso é criado? Ele me retorna o status correto? Ele valida os campos obrigatórios?

Ao mandar um PUT, ele atualiza o meu recurso? Ele atualiza apenas o meu recurso? Ele valida o nome dos meus campos? Etc..

### Fontes e Sugestão de Leituras:
