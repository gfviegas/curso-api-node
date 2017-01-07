# Configurando nosso ambiente de Testes
Para os testes, vamos usar os seguintes packages:
- [Mocha](https://mochajs.org/) - Framework de Testes
- [Chai](http://chaijs.com/) - Framework de Asserção
- [Sinon](http://sinonjs.org/) - Spies, Stubs e Mocks!
- [Sinon-Mongoose](https://www.npmjs.com/package/sinon-mongoose) - Ferramenta para usar Sinon com mongoose models
- [SuperTest](https://github.com/visionmedia/supertest) - Nos auxilia a fazer requests HTTP e algumas asserções
- [Istanbul](https://istanbul.js.org/) - Gera code coverage
- [NYC](https://github.com/istanbuljs/nyc) - Interface de linha de comando para Istanbul

Você pode instalá-los no seu projeto com `npm i --save-dev chai istanbul mocha nyc sinon sinon-mongoose supertest`.

# Organização de Arquivos
No root de nosso projeto teremos uma pasta `tests`, que abrigará tanto nossos testes unitários como testes integrados.

## ./tests/config
Aqui vamos colocar os arquivos que vão ajustar a execução da API no ambiente de testes. Ou seja, setar as nossas variáveis de ambientes e configurações dos frameworks e plugins de testes utilizados.

## ./tests/helpers
Podemos reutilizar diversas operações em diferentes módulos com helpers.

## E por fim, nossos specs.
Em `./tests/integration`, nossos testes de integração, e em `./tests/unit` nossos testes unitários. As duas pastas vão "espelhar" a raiz do projeto, pra testarmos tudo que for possível.

Muita gente contesta esse padrão de organização de testes, mas vamos o utilizar por ser fácil visualizar os arquivos sendo testados, e para dividirmos a execução de testes integrados e unitários de forma independentes.

Os testes unitários devem ter o mesmo path do arquivo testado, mas com a extensão `.spec.js` para facilitar a identificar o arquivo como teste. Já os testes integrados vão seguir o padrão de `modules/versao/nomeDoModulo.spec.js`

### Fontes e Sugestão de Leituras:
