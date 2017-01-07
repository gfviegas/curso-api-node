# O que é TDD?
TDD significa desenvolvimento guiado por testes. Isso significa que você começa o desenvolvimento criando os testes, depois o codigo, fazendo mais testes, e melhorando o seu código.

## Peraí...
> Não entendi NADA. PRA QUÊ DIABOS EU VOU FAZER TESTE SE EU NÃO FIZ O CÓDIGO ANTES?? WTFFFF

Bom quando você vai programar qualquer coisa você sabe o que vai fazer, não sabe? Na sua cabeça você já vai pensando em como quer que seu programa final se comporta. Por exemplo (clássico exemplo), uma calculadora. Antes de você começar o código você já sabe que quando apertar 2 númeors e o botão de *+*, quer que o resultado que apareça é a soma dos dois números. Você ainda não fez código, mas já tem um cenário de teste.

> Tá.. mas, não tenho código, vou testar o que?

Você começa escrevendo o cenário do seu teste, por exemplo `Ao mandar dividir, deve-se retornar a divisão do primeiro número pelo segundo`. Você literalmente escreve o cenário do seu teste, e manda chamar sua função (que não existe ainda) e verifica se o resultado realmente foi a soma dos dois números. Obviamente o seu teste vai falhar, porque sua função ainda não existe.

É agora que você realmente vai fazer o código. Você explicitou todos os cenários referentes a sua função (Exemplo: Divisão por zero, divisão de floats, divisão de strings, divisão de arrays) e agora sabe exatamente como a sua função deve se comportar. A sua meta agora é escrever código até seus testes passarem.

A medida que seu sistema cresce, cresce também a necessidade de aprimorar as suas funções (Exemplo: permitir divisão de um array de numeros por outro). Você vai escrever o teste da sua nova funcionalidade, ver ela falhar, e aí sim desenvolver ela. Pode parecer que você vai perder tempo escrevendo teste antes, e burocratizando o processo, mas isso na verdade de salva MUITO tempo debugando erros e garantindo a qualidade do seu código.

![Test, Code, Refactor](https://rafaelfranchi.files.wordpress.com/2012/01/tdd_cycle.jpg)

Então lembre-se desse ciclo:
- Faça o teste
- Veja ele falhar miseravelmente
- Escreva o seu código
- Veja seu teste passar
- Faça mais testes!!
- ....


### Fontes e Sugestão de Leituras:
- [Test-Driven-Development for building APIs in Node.js and Express](https://developers.redhat.com/blog/2016/03/15/test-driven-development-for-building-apis-in-node-js-and-express/)
- [O que é TDD](https://rafaelfranchi.wordpress.com/2012/01/25/o-que-e-tdd/)
