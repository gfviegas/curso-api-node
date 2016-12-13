# O que é uma API RESTful?
REST é uma sigla para Representational State Transfer (REST). RESTful é o "ato de utilizar o REST", se é que posso colocar assim. O conceito de quando sua API pode ser considerada RESTful ou não é bem controverso e não tem um martelo com a decisão final. A questão é que tem diversas regras e boas práticas a serem seguidas, e uma API RESTful, pra mim, é a que segue a maioria delas, respeitando um mínimo de coerência e padrão.

O assunto é bem vasto, então vou dar uma resumida nos pontos mais importantes de uma API RESTful. A grande maioria dessas coisas vão se esclarecendo a medida que você consome e constrói uma API, mas é importante entender esses aspectos antes de partir pra ação, para *pensarmos de maneira REST*
- Utilizar o devido método HTTP a operação desejada
  - Ou seja, não utilize o método GET para criar um novo resource, ou POST para pegar as informações de um dado. Vamos explicar cada um dos métodos a seguir.
- Não utilize verbos nas rotas de recursos
  - Justamente por utilizarmos os métodos HTTP para definir o que queremos fazer (listar, criar, deletar, etc...), não precisamos definir o que a rota faz com o seu recurso. Ou seja, não crie uma rota `/api/create/user`. Isso é feio e desnecessário.
  - Qualquer manipulação de algo que não seja um recurso o uso de verbos é permitido, mas considerado uma má pratica. Imagine uma API de um player de video que tem uma rota `POST /api/video/12/pause`. Teoricamente, ele pausa o video 12. Ok. Mas pensando de maneira RESTful, você está alterando o "estado" do player para "pausado", então deveria ser um `PATCH /api/video/12`, onde você enviaria um body, com por exemplo: `{status: 'paused'}`. Mais sobre o PATCH a seguir.
- Ser stateless
  -  Ou seja, não possuir "estados", e não guardar o estado da comunicação de qualquer um dos clientes além de uma única requisição.
  - Isso significa que não devemos guardar sessões de usuários na API. Mais sobre isso no módulo de autenticação nesse curso.
- Versione sua API. Preferencialmente na URL, e omite as subverões.
 - Sua API precisa ter versões pois muitos clientes podem implementar suas solucoes em uma versão da sua API, que por exemplo, tem uma rota `api/photos`. Mas vamos supor que você, por qualquer motivo, teve que renomear essa rota para `api/pictures`. Você vai alterar sua API e quebrar as aplicações dos seus clientes? O ideal é versionar a api, fazendo essa alteração em uma versão futura como: `api/v2/pictures`, e documentar essa alteração e seu motivo.
 - Muitas soluções fazem a comunicação da versão de sua API via header. O ideal é utilizar via URL para explicitar ao browser o seu resource através das versões.
 - Sua API com certeza terá sub-versões como `1.1`, `1.2.34`, etc. Essas versões podem ser omitidas na URL, podendo utilizar por exemplo, `/api/v1/user`, para comunicar com o resource User em todas as subversões de `1.X`. Imagine fazer os clientes atualizarem suas URLs a cada subversão? E qual seria a necessidade? Normalmente quando algo é impactante e "quebrável", o recomendado é criar uma nova versão inteira, ou um novo resource e manter suporte a rota anterior. Vai depender da sua aplicação.


 Bom, pontuei apenas os pontos que considero extremamente necessários, mas não listei nem metade. Como disse, o mundo REST é vasto, e te sugiro estudar a fundo as outras regras e sugestões, mas vamos continuar com o conteúdo.

## Métodos HTTP
Existem outros além do que será explicado a seguir, mas eles são importantes conhecer. Os outros métodos são muito específicos e pouco utilizados.

- [OPTIONS](#options)
- [HEAD](#head)
- [GET](#get)
- [POST](#post)
- [PUT](#put)
- [PATCH](#patch)
- [DELETE](#delete)

### OPTIONS
Quem não conhece RESTful se assusta ao ver esse carinha aqui! Ele é enviado para saber quais cabeçalhos, origens, métodos o servidor aceita no request futuro.

> Como assim "Request Futuro?"

É que o OPTIONS é chamado de *preflight*. Ele é enviado *"automaticamente"* pelo browser quando você tenta enviar uma header incomum ou um método POST para uma origem desconhecida. Então quando tentamos, por exemplo, mandar um POST para `http://www.google.com/test` com um `Content-Type: application/json` provavelmente o seu browser enviará esse OPTIONS antes, pra saber se o destino do seu request *POST* aceita o seu request e o que você está tentando enviar.

Como uma API RESTful não conhece a fundo o seus consumidores, e o que ele envia, ela tem que tratar esses requests OPTIONS, aceitando as origens e headers. Vale lembrar que o OPTIONS também pode enviar informações preciosas para *auto-documentar* sua API. Você teoricamente só precisa responder o seu request OPTIONS com uma header `Accept` e um status 2XX. Mas nada te impede de te enviar um corpo de resposta, especificando por exemplos os campos obrigatórios do endpoint, o que ele faz, como utilizar, etc. Isso pode gerar uma banda maior por ficar trafegando mais dados pelo HTTP, mas em alguns cenários, isso pode ser bom.

### HEAD
> O método HEAD é retorna todos os cabeçalhos HTTP do método GET, porém sem corpo de resposta.

Esse método pode parecer meio inútil a primeira vista. Ele é bastante útil sim, em qualquer aplicação, mas nem sempre necessário. Então muitas vezes ele é esquecido. :pensive:

O HEAD também é confudido muitas vezes com o OPTIONS, mas eles tem funções distintas. Como já explicado acima, o OPTIONS verifica se o endpoint *aceita* o origin e headers enviadas, e quais são aceitos. Já o HEAD te envia **quais** headers seriam retornadas do mesmo endpoint em GET.

> Tá. Mas quando diabos só pegar as headers vai ser útil?

Alguns exemplos são: pegar informações de Cache, qual Content-Type é retornado (dependendo da aplicação pode ter content-types misturados), qual o status code que seria retornado, verificar se o server está ativo ou não, pegar informações de qual servidor está sendo utilizado, verificar se tem permissão (autenticação) para realizar o request, entre outras coisas...

Bom, bacana, entendemos o que é o HEAD. Vamos aprender também algumas regras em torno desse método.

> O método HEAD **NÃO DEVE** retornar um body na sua resposta.

> A informação contida nos cabeçalhos de repsosta HTTP **PODE** ser idêntica ao do método GET.

### GET
O método GET recupera um recurso. Ele é o método mais comum e é utilizado o tempo todo para buscar dados. Ele não tem muito segredo, envie um GET ao endpoint de um recurso e você receberá os dados solicitados.

Se tratando de RESTful é importante que os parametros sejam enviados de forma correta. Sabemos que o padrão RESTful é por entidade, e não métodos. Sendo assim, uma lista de uma entidade, por exemplo `Foods`, seria recuperado com um `GET /api/v1/food`. Se você quer recuperar um food específico, você o passa como parametro na url path: `GET /api/v1/food/23`, que te traria o food identificado como "23".

Note que essa maneira de passar os parâmetros vale para **IDENTIFICADORES** de um recurso, e não pra buscas, paginação, filtros, etc. No exemplo anterior, `23` seria o ID, ou codigo unico do food. Esse ID identifica o recurso que estamos analisando e ele sempre será o food #23. Quando queremos fazer um GET com paginação, ordenação, busca e outros parametros, o recomendado é passar via query string, como em `GET api/v1/food?page=1`. Este *padrão* de parâmetros não existe, mas é dessa forma que a maioria das APIS REST são feitas, e então sugiro fazer o mesmo.

> NUNCA, eu disse **NUNCA** use GET para fazer qualquer outra coisa a não ser recuperar dados.
>> É sério, NUNCA.


### POST
O método POST serve para criar recursos. Algumas APIs utilizam esse método para atualizar os dados também, mas não faça isso. Já temos outros dois métodos HTTP para atualização de dados, não tem porquê usar o POST.

Nem sempre fica explícito que uma certa operação é de criação de recursos e deve usar o POST. Por exemplo, se tivermos uma loja virtual e precisamos de um endpoint para comprar um produto. Não é explicito que "comprar um produto" significa "criar um novo recurso", mas teoricamente você está cadastrando um novo pedido não é? Então o correto seria um `POST /api/v1/order`, enviando no body os dados desse pedido. Nada de `POST /api/v1/purchase` ou qualquer verbo, como foi dito anteriormente.

Sempre envie os dados do POST no body do seu request. Se tratando de APIs REST a maioria dos casos você deverá enviar um JSON, mas em alguns casos como em upload de arquivos é totalmente aceitável enviar um Form Data.

Retorne um status `201 Created` se for retornar o recurso criado. Caso contrário retorne um status `204 No Content`.

### PUT
Esse é um dos métodos para atualizar recursos. Ele se comporta da mesma maneira que o método POST, enviando dados por body, porém você precisa identificar o recurso sendo atualizado. Seguindo o exemplo anterior, se você quisesse atualizar o seu pedido, teria que fazer um `PUT /api/v1/order/930293` pra atualizar o pedido #930293. Sacou?

O PUT é utilizado para atualização *COMPLETA* do seu recurso, substituindo todos os valores pelos enviados. Isso significa que esse método não serve para atualizar por exemplo, apenas o nome do seu usuário. Você tem que mandar TODOS os dados do seu usuário e o PUT substitui tudo recebido. Se você precisa atualizar somente alguns dados, veja o método abaixo.

Você pode retornar um status `200 OK` se quiser retornar algum dado descrevendo a entidade atualizada, um `202 Accepted` se ainda não executou a atualização ou um `204 No Content` se atualizou mas não retorna nenhum dado.

### PATCH
É o mesmo esquema do PUT, porém ele não substitui por completo o recurso atualizado. Isto é, utilize esse método se quer atualizar apenas parte do recurso, como um campo do banco de dados.

Assim como o PUT, você pode retornar um status `200 OK` se quiser retornar algum dado descrevendo a entidade atualizada, um `202 Accepted` se ainda não executou a atualização ou um `204 No Content` se atualizou mas não retorna nenhum dado.

### DELETE
Mais autodescritivo que esse, não vamos encontrar. `DELETE /api/v1/user/92`: excluí o user #92. Simples assim.

Não se deve retornar uma resposta de sucesso a exclusão ao menos que a ação destrutiva já tenha sido finalizada (como excluindo o arquivo do storage, ou o recurso do banco de dados).

Você pode retornar um status `200 OK` se quiser retornar algum dado descrevendo a entidade excluída, um `202 Accepted` se ainda não executou a ação destrutiva ou um `204 No Content` se executou a açào mas não retorna nenhum dado.

### Fontes e Sugestão de Leituras:
- [Best Practices for Designing a Pragmatic RESTful API](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)
- [RESTful patterns for the HEAD verb](https://www.pragmaticapi.com/blog/2013/02/14/restful-patterns-for-the-head-verb)
- [The HTTP OPTIONS method and potential for self-describing RESTful APIs](http://zacstewart.com/2012/04/14/http-options-method.html)
- [REST introduction](https://www.infoq.com/br/articles/rest-introduction)
