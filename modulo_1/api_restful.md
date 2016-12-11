### O que é uma API RESTful?
<!-- TODO: Escrever isso -->

#### Request OPTIONS
Quem não conhece RESTful se assusta ao ver esse carinha aqui! Ele é enviado para saber quais cabeçalhos, origens, métodos o servidor aceita no request futuro.

> Como assim "Request Futuro?"

É que o OPTIONS é chamado de *preflight*. Ele é enviado *"automaticamente"* pelo browser quando você tenta enviar uma header incomum ou um método POST para uma origem desconhecida. Então quando tentamos por exemplo mandar um POST para `http://www.google.com/test` com um `Content-Type: application/json`, por exemplo, provavelmente o seu browser enviará esse OPTIONS antes, pra saber se o destino do seu request *POST* aceita o seu request e o que você está tentando enviar.

Como uma API RESTful não conhece a fundo o seu consumidor, e o que ele envia, ela tem que tratar esses requests OPTIONS, aceitando as origens e headers. Vale lembrar que o OPTIONS também pode enviar informações preciosas para *auto-documentar* sua API. Você teoricamente só precisa responder o seu request OPTIONS com uma header `Accept` e um status 2XX. Mas nada te impede de te enviar um corpo de resposta, especificando por exemplos os campos obrigatórios do endpoint, o que ele faz, como utilizar, etc. Isso pode gerar uma banda maior por ficar trafegando mais dados pelo HTTP, mas em alguns cenários, isso pode ser bom.

#### Request HEAD
> O método HEAD é retorna todos os cabeçalhos HTTP do método GET, porém sem corpo de resposta.

Esse método pode parecer meio inútil a primeira vista. Ele é bastante útil sim, em qualquer aplicação, mas nem sempre necessário. Então muitas vezes ele é esquecido. :pensive:

O HEAD também é confudido muitas vezes com o OPTIONS, mas eles tem funções distintas. Como já explicado acima, o OPTIONS verifica se o endpoint *aceita* o origin e headers enviadas, e quais são aceitos. Já o HEAD te envia **quais** headers seriam retornadas do mesmo endpoint em GET.

> Tá. Mas quando diabos só pegar as headers vai ser útil?

Alguns exemplos são: pegar informações de Cache, qual Content-Type é retornado (dependendo da aplicação pode ter content-types misturados), qual o status code que seria retornado, verificar se o server está ativo ou não, pegar informações de qual servidor está sendo utilizado, verificar se tem permissão (autenticação) para realizar o request, entre outras coisas...

Bom, bacana, entendemos o que é o HEAD. Vamos aprender também algumas regras em torno desse método.

> O método HEAD **NÃO DEVE** retornar um body na sua resposta.
> A informação contida nos cabeçalhos de repsosta HTTP **PODE** ser idêntica ao do método GET.

#### Request GET
<!-- TODO: Escrever isso -->
#### Request POST
<!-- TODO: Escrever isso -->
#### Request PUT
<!-- TODO: Escrever isso -->
#### Request PATCH
<!-- TODO: Escrever isso -->
#### Request DELETE
<!-- TODO: Escrever isso -->

### Fontes e Sugestão de Leituras:
<!-- TODO: Colocar links legais aqui -->
[RESTful patterns for the HEAD verb](https://www.pragmaticapi.com/blog/2013/02/14/restful-patterns-for-the-head-verb)
[The HTTP OPTIONS method and potential for self-describing RESTful APIs](http://zacstewart.com/2012/04/14/http-options-method.html)
