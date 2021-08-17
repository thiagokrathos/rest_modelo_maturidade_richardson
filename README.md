#  Modelo de maturidade Richardson

Apesar de Roy Fielding deixar bastante claro que para uma API ser considerada RESTful ela precisa obrigatoriamente seguir todas as constraints definidas em seu trabalho, na prática, muitas vezes precisamos de uma abordagem um pouco mais simples.

Sendo assim, Leonard Richardson propôs um modelo de 4 níveis para que alcancemos uma API REST.

Os níveis 0, 1 e 2 talvez sejam mais familiares, e de fato são mais fáceis de implementar, porém, deve ficar claro que os mesmos não são considerados RESTful.

![screen shot 2018-08-12 at 19 56 38](https://user-images.githubusercontent.com/6649193/44007397-0330098e-9e6b-11e8-91e0-24a2cf5d3b55.png)


http://martinfowler.com/articles/richardsonMaturityModel.html
 
##  Nível 0 - POX

Você certamente já deve ter desenvolvido ou visto em algum lugar uma API que segue esse modelo de design. 

Apesar de ser o nível mais distante do que de fato REST propõe, muitas APIs ditas como RESTful se encontram neste nível de maturidade.

Neste nível, as mensagens podem ser serializadas em formatos como XML, JSON ou outros. 

É importante lembrar, como dito anteriormente, que não é o formato da mensagem que define ou não um sistema REST.

        POST /salvarCliente <Cliente>
             <Nome>Manoel da Silva</Nome>
             ...
             </Cliente>

### 

![screen shot 2018-08-12 at 20 01 27](https://user-images.githubusercontent.com/6649193/44007398-034c1052-9e6b-11e8-94b4-6b631486ff5d.png)

### 

![screen shot 2018-08-12 at 20 01 40](https://user-images.githubusercontent.com/6649193/44007399-036c939a-9e6b-11e8-8494-81c98a9c962e.png)
 
 
 
Um outro problema constantemente encontrado, é a manipulação incorreta dos códigos de resposta do HTTP.

Códigos e mensagens de erros são frequentemente manipuladas nas mensagens geradas pela aplicação, o que impede que elementos de gateway e proxy trabalhem de forma adequada.

    GET /buscarCliente/1
    HTTP/1.1 200 OK
    <buscarCliente>
    <status>CLIENTE NÃO ENCONTRADO</status> <codigo>404</codigo>
    <buscarCliente>

Apesar da mensagem sugerir que o cliente solicitado não foi encontrado, a resposta HTTP apresenta uma informação totalmente diferente (200 OK), ou seja, existe uma diferença semântica entre o procolo HTTP e a representação gerada pela aplicação.

Em resumo, no nível 0 podemos dizer que:

      “Você nem mesmo está usando o HTTP de forma correta!”

## Nível 1 - Recursos

No nível 1, passamos a usar recursos como forma de modelar e organizar a API. 
Neste nível não precisaremos conhecer a funcionalidade de cada método e sim apenas o recurso ao qual temos acesso.

    POST /cliente
    <Cliente>
    <Nome>Monoel da Silva</Nome>
    ...
    </Cliente>

###

![screen shot 2018-08-12 at 20 10 17](https://user-images.githubusercontent.com/6649193/44007482-a64993a0-9e6c-11e8-97d7-f918e35b7ad4.png)

###

![screen shot 2018-08-12 at 20 10 28](https://user-images.githubusercontent.com/6649193/44007483-a699b9d4-9e6c-11e8-8b5f-d38c756e93a0.png)


Modelando corretamente os recursos, precisamos usar os métodos HTTP da forma certa, para que a gente possa criar todas as interações necessárias sob um recurso.

    No nível 1 dizemos que agora temos recursos!

##  Nível 2 - Verbos HTTP

Nesse nível, o HTTP deixa de exercer um papel apenas de transporte e passa a exercer um papel semântico na API, ou seja, seus verbos passam a ser utilizados com o propósito no qual foram criados.

A utilização do métodos mais conhecidos (GET, POST, PUT e DELETE), bem como a tratativa correta dos códigos de resposta, permitem a modelagem e interação com os recursos presentes em uma API.


Enviando

    POST /cliente
    <Cliente>
    <Nome>Manoel da Silva</Nome>
    ...
    </Cliente>

Recebendo

     201 Created
     Location: /cliente/1

É importante notar 2 aspectos nessa resposta: O primeiro é a utilização correta da resposta “201 Created”. 

Como foi solicitado a criação de um recurso, nada mais adequado que uma resposta que informe que o recurso foi criado com sucesso.
 
Além disso, um importante aspecto é a presença do header “Location”. 

Esse header informa em qual endereço o recurso criado se encontra disponível.

Recebendo

    201 Created
    Location: /cliente/1
    
    
     No nível 2 dizemos que agora usamos os verbos HTTP de forma correta!

##  Código de Status HTTP

https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

##  Nível 3 - HATEOAS

HATEOAS (Hypermedia as the Engine of Application State)

Em seu blog pessoal, Fielding deixa muito claro que APIs que não utilizam HATEOAS não podem ser consideradas RESTful, mesmo assim, você vai encontrar muitos conteúdos sobre REST que nem ao menos cita essa característica.

Apesar de aparentemente ser algo não muito familiar, HATEOAS é um conceito presente no dia a dia de todos os usuários da Web. 

Ele tem como elemento principal uma representação hypermedia, que permite um documento descrever seu estado atual e quais os seus relacionamentos com outros futuros estados

              GET /cliente/1
              HTTP/1.1 200 OK
              <Cliente>
                  <Id>1</Id>
                  <Nome>Manoel da Silva</Nome>
                  <link rel="deletar" href="/cliente/1" />
                  <link rel="notificar" href="/cliente/1/notificacao" />
              </Cliente>


No exemplo, o cliente da API deverá entender o significado dos relacionamentos “deletar” e “notificar” para que ele consiga de fato consumir de forma adequada esses links.

        No nível 3 dizemos que temos controles de hipermídia!
        
## Resumo

* Nível 0 - Você não sabe nem mesmo usar o HTTP corretamente.

* Nível 1 - Agora você tem recursos.

* Nível 2 - Agora você saber usar os verbos HTTP corretamente.

* Nível 3 - Agora você usa controles de hipermídia.
