# Relatório 4 - ESOF
## Facebook/React - Verificação e Validação de Software

### <a name="introducao"></a>Introdução

O objetivo deste relatório é a análise dos processos de verificação e validação (V&V) seguidos no desenvolvimento da biblioteca React, com a descrição de algumas das características deste projeto que digam respeito à aplicação desses processos.

Numa primeira fase, explorar-se-á o grau de testabilidade do *software*, analisando a [controlabilidade](#controllability) do estado dos componentes, a [observabilidade](#observability) dos resultados e a [isolabilidade](#isolateability) dos componentes, assim como o grau de [separação](#separation) de funcionalidades, de [inteligibilidade](#understandability) dos componentes e de [heterogeneidade](#heterogeneity) das tecnologias utilizadas.

Numa segunda fase, serão apresentadas algumas estatísticas pertinentes relacionadas com a verificação e validação do *software*.

Finalmente, será realizado um exercício que consistirá na seleção de um *bug report* e na conceção de casos de teste que possam, eventualmente, conduzir à resolução do mesmo.

### <a name="testabilidade"></a>Testabilidade do *Software*

A discussão que se desenrolará nesta secção incidirá sobre o quão testável o projeto React é, isto é, até que ponto é possível verificar e validar a implementação da biblioteca. A discussão será acompanhada de exemplos e de referências que suportem a interpretação dos autores sempre que for considerado pertinente.

#### <a name="controllability"></a>Controlabilidade

A biblioteca React define um conjunto de classes que implementam a sua funcionalidade, tirando partido do suporte à programação orientada por objetos, baseada na definição de protótipos, oferecido pelo JavaScript. Uma vez que é possível aceder, num dado instante, às [propriedades](http://www.w3schools.com/js/js_properties.asp) de um objeto, que podem ser atributos ou métodos, parece razoável admitir que é possível controlar o estado do mesmo, já que é possível conhecê-lo em cada momento. Visto que os testes unitários são realizados ao nível da classe e do método, é possível afirmar que, para esse tipo de testes, o estado do componente que está a ser testado é controlável. Os testes unitários sobre a biblioteca React são realizados com [Jest](https://facebook.github.io/jest/), como será explorado nas subsecções seguintes.

Outro tipo de testes que são realizados sobre a biblioteca React são os testes de integração que fazem uso da ferramenta [Travis CI](https://travis-ci.org/), conforme referido adiante. Estes testes são realizados ao nível de um conjunto de módulos, por exemplo, ao nível do pacote. Neste caso, podemos definir o estado de um pacote como sendo uma instanciação possível do conjunto das classes definidas no mesmo. Uma vez que é possível conhecer o estado de cada objeto, conforme discutido no parágrafo anterior, também é possível conhecer o estado dessa instanciação, o que faz com que, neste caso, o componente a ser testado também seja controlável.

Estes são os dois tipos de teste mais relevantes no contexto da biblioteca React.

#### <a name="observability"></a>Observabilidade

Como já foi referido anteriormente, as duas ferramentas usadas pelos colaboradores do projeto React para efeitos de teste são o [Jest](https://facebook.github.io/jest/), para [testes unitários](https://en.wikipedia.org/wiki/Unit_testing), e o [Travis CI](https://travis-ci.org/), para [testes de integração](https://en.wikipedia.org/wiki/Integration_testing).

O Jest é uma *framework* para a definição de testes unitários desenvolvida pelo Facebook e é a principal ferramenta do género usada no projeto React, surgindo como uma extensão à *framework* [Jasmine](http://jasmine.github.io/edge/introduction.html). Entre as suas principais características, destaca-se o facto de permitir o [isolamento](http://facebook.github.io/jest/docs/automatic-mocking.html) de uma unidade de código, como será discutido na subsecção seguinte, e a execução de testes em processos paralelos, o que resulta num melhor desempenho. Esta ferramenta permite recolher informação sobre o resultado de um teste, como mostra o seguinte exemplo:

> npm test
> [PASS] jest/examples/__tests__/fetchCurrentUser-test.js (0.075s)

A ferramenta Travis CI foi já apresentada no [Relatório 2](./Relatorio_2.md#validacao). É utilizada para realizar testes de integração sobre o código submetido pelos colaboradores do projeto em *pull requests*, conforme explicado nesse relatório. Na página da ferramenta, é possível ver o resultado de alguns dos testes realizados, como mostra o [seguinte exemplo](https://travis-ci.org/facebook/react/builds/90839775). Cada teste tem um conjunto de *jobs* associados, isto é, um conjunto de tarefas que constituem o teste. [Nesta página](https://travis-ci.org/facebook/react/jobs/90839776), podem ser encontradas informações sobre um dos *jobs* do teste de exemplo referido acima, nomeadamente o *log* resultante da execução do teste, contendo os respetivos resultados. Note-se que um teste de integração com Travis CI pode, eventualmente, envolver a execução de testes unitários com Jest, como mostra o *log* [deste *job*](https://travis-ci.org/facebook/react/jobs/90839777).

Conclui-se, desta forma, que é possível observar e recolher informação útil sobre os resultados dos testes que são realizados sobre a biblioteca React.

#### <a name="isolateability"></a>Isolabilidade

Um dos principais desafios na definição de testes unitários é o isolamento de cada componente, isto é, a definição de testes cujo resultado não seja condicionado por eventuais dependências exteriores à unidade de código a ser testada. É neste capítulo que a ferramenta Jest se revela particularmente útil, pois permite o [isolamento](https://facebook.github.io/jest/docs/automatic-mocking.html) da unidade a ser testada através da definição de *mocks*, isto é, de funções que substituirão as funções reais definidas nos módulos dos quais a unidade depende.

Estas [funções](https://facebook.github.io/jest/docs/mock-functions.html) são definidas automaticamente, embora também seja possível defini-las [manualmente](https://facebook.github.io/jest/docs/manual-mocks.html) sempre que for necessário. A [API](https://facebook.github.io/jest/docs/api.html#content) do Jest define funções que permitem, por exemplo, simular [valores de retorno](https://facebook.github.io/jest/docs/mock-functions.html#mock-return-values) das funções *mock*, alterar a sua [implementação](https://facebook.github.io/jest/docs/mock-functions.html#mock-implementations), ou testar a forma como as mesmas são [chamadas](https://facebook.github.io/jest/docs/mock-functions.html#custom-matchers), nomeadamente o número de vezes em que tal ocorre e o número e valor dos argumentos.

Assim, é possível concluir que, no caso da biblioteca React, ao ser usada a *framework* Jest para a definição e execução de testes unitários, é possível isolar o componente que está a ser testado, fazendo uso da funcionalidade referida.

#### <a name="separation"></a>Separação de Funcionalidades

Ao desenvolver uma dada classe ou componente, é importante garantir que a funcionalidade atríbuida fique confinada, o mais possível, ao elemento a que diz respeito, sob pena de tornar o código mais confuso e, por conseguinte, menos testável. Com efeito, em projectos de dimensão considerável, este aspecto deve ser tido em consideração por todos os programadores, por forma a evitar a existência de [*Spaghetti code*](https://en.wikipedia.org/wiki/Spaghetti_code), que dificulta, a médio e a longo prazo, a manutenção do projecto.

De uma forma geral, a biblioteca React apresenta uma separação de funcionalidades bem definida, característica essa que está patente na organização de todo o projecto. Repare-se no exemplo de [renderização das páginas](https://github.com/facebook/react/tree/master/src/renderers/dom): a renderização de uma página web pode ser realizada quer no lado do cliente, quer no lado do servidor (esta última possibilitada através de [isomorfismo](./Relatorio_2.md#levantamento)); contudo, é efectuada de maneira diferente, consoante a entidade em questão. Tem-se, assim, uma separação importante da funcionalidade que é importante frisar, e que a *core-team* decidiu implementar através da criação de vários *packages*, expostos sob a forma de *folders*. 
Por outro lado, é também importante isolar o conjunto de funcionalidades que são comuns quer no lado do cliente quer no servidor, com o principal intuito de evitar a repetição de código ([DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)) e promover a [reutilização de código](https://en.wikipedia.org/wiki/Code_reuse), por forma a evitar, ao máximo, a introdução de *bugs* no projecto. O *package* *shared* contém, assim, os elementos que são utilizados por quaisquer das entidades já referidas (cliente ou servidor). 

É importante notar que a separação de funcionalidades facilita o [isolamento](#isolateability) do elemento na fase de testes, o que permite testar o código de forma mais incisiva e cobrir um maior número de situações de teste.

#### <a name="understandability"></a>Inteligibilidade

Em projectos de dimensão considerável, a componente de documentação possui um peso bastante considerável, por forma a garantir a consistência de uso dos vários elementos disponibilizados.

A biblioteca React [apresenta uma API](https://facebook.github.io/react/docs/top-level-api.html) dos vários recursos, que, no ponto de vista dos autores do relatório, é bastante extensiva e concisa. Para além desta documentação, é possível encontrar, ao longo do código, vários elementos (comentários, nomes de variáveis, ...), que permitem um maior esclarecimento das entidades aí presentes, tal como se verifica [neste exemplo](https://github.com/facebook/react/blob/master/src/renderers/dom/client/ReactMount.js).

Durante o desenvolvimento de um *test suite*, é importante conhecer a funcionalidade dos vários elementos. Contudo, esse conhecimento só é possível de ser adquirido caso exista uma documentação bem estruturada, daí a importância dessa documentação relativa aos elementos a serem testados. Desta forma, é possível cobrir um maior número de casos a testar, e, por conseguinte, averiguar a robustez do código.


#### <a name="heterogeneity"></a>Heterogeneidade

-- Referir que a utilização de um repositório Git que pode receber contribuições de muitos utilizadores conduz à necessidade de testes de integração com Travis CI, que são aplicados em paralelo com os testes unitários definidos com Jest.

Uma vez que o React é uma biblioteca *open-source*, e, por conseguinte, sujeita a contribuições de vários programadores, é necessário garantir que, após a aceitação e a incorporação das modificações dos *pull-requests*, o sistema permanece funcional. Desta forma, é testada a integrabilidade do sistema, em paralelo com a realização de testes unitários incidentes nos vários elementos da biblioteca.

Por um lado, a realização de testes unitários ao nível da classe permite garantir a consistência dos elementos da biblioteca, com o recurso à *framework* [Jest](https://facebook.github.io/jest/). Por outro lado, é preciso averiguar se a integração dos vários elementos é feita de forma correcta. Nesta vertente, é utilizada a ferramenta [Travis CI](https://travis-ci.org/facebook/react), que premite automatizar este processo.


### <a name="estatisticas"></a>Estatísticas de Teste

-- Exemplos de logs do Travis CI. Tentar recolher estatísticas de cobertura dos testes Travis CI. Referir possibilidade de recolher estatísticas de cobertura com Jest (https://facebook.github.io/jest/docs/api.html#config-collectcoverage-boolean). Já publiquei no fórum: https://discuss.reactjs.org/t/line-coverage-for-tests-on-react/2479

-- Link a estudar https://github.com/travis-ci/travis-logs

#### <a name="travis"></a>Travis CI

Travis CI consiste numa ferramenta muito versátil e útil para este projeto. Na sua vertente [Travis Logs] (https://github.com/travis-ci/travis-logs), processa atualizações do log que são transmitidos de outra vertente, [Travis Worker] (https://github.com/travis-ci/worker), via [RabbitMQ] (http://www.rabbitmq.com/), um programa *open-source* [intermediário para envio de mensagens] (https://en.wikipedia.org/wiki/Message_broker). Uma vez que todas as peças de log forem recebidas, e passado um tempo limite padronizado de 10 segundos, estas peças são agregadas num log final.

#### <a name="jest"></a>Jest

A *framework* [Jest](https://facebook.github.io/jest/), ferramenta para testes unitários do [Facebook] (https://github.com/facebook/), utilizada para testar as aplicações React de forma simples. Estes [testes] (http://facebook.github.io/react/docs/test-utils.html) manipulam componentes do React averiguando o seu comportamento. Para um teste unitário operar numa unidade isolada da aplicação é necessário *mock* todos os módulos exceto aquele que está a ser testado. Jest constrói o *mocking* das partes de uma aplicação [Flux] (https://facebook.github.io/flux/) trivial. Flux é a arquitetura da aplicação que o Facebook utiliza para construir aplicações *web* com o React.

-- Utilização do Jest no React: https://github.com/facebook/jest/blob/fd33aad1da1ed430b656719c7a0e00bb7952c6df/docs/TutorialReact.md

#### <a name="cobertura"></a>Cobertura dos Testes

De forma a conseguir estudar a cobertura do projeto React, utilizou-se a ferramenta [istanbul](http://gotwarlost.github.io/istanbul/), que permite testar a cobertura do código JavaScript.

![Teste de Cobertura](./Resources/Testes de Cobertura.png)

### <a name="opcional"></a>Fix Bug Report 

*Bug* escolhido: https://github.com/facebook/react/issues/5468

Regiões do código modificadas:

https://github.com/rppc/react/blob/master/src/isomorphic/classic/class/ReactClass.js#L344
https://github.com/rppc/react/blob/master/src/isomorphic/classic/class/ReactClass.js#L359
https://github.com/rppc/react/blob/master/src/isomorphic/classic/class/ReactClass.js#L388
https://github.com/rppc/react/blob/master/src/isomorphic/classic/class/ReactClass.js#L403
https://github.com/rppc/react/blob/master/src/isomorphic/classic/class/ReactClass.js#L418
https://github.com/rppc/react/blob/master/src/isomorphic/classic/types/ReactPropTypes.js#L99
https://github.com/rppc/react/blob/master/src/isomorphic/classic/types/ReactPropTypes.js#L116
https://github.com/rppc/react/blob/master/src/isomorphic/classic/types/ReactPropTypes.js#L213
https://github.com/rppc/react/blob/master/src/isomorphic/classic/types/ReactPropTypes.js#L277

Caso de teste:

https://github.com/rppc/react/blob/master/src/isomorphic/classic/class/__tests__/ReactClass-test.js#L384

*Pull request*:

https://github.com/facebook/react/pull/5475

### <a name="analise"></a>Análise Crítica


### <a name="info"></a>Informações

##### Autores:

* António Casimiro (antonio.casimiro@fe.up.pt)
	* Número de horas despendidas:
	* Contribuição:
* Diogo Amaral (diogo.amaral@fe.up.pt)
	* Número de horas despendidas:
	* Contribuição:
* Pedro Silva (pedro.silva@fe.up.pt)
	* Número de horas despendidas:
	* Contribuição:
* Rui Cardoso (rui.peixoto@fe.up.pt)
	* Número de horas despendidas:
	* Contribuição:

Faculdade de Engenharia da Universidade do Porto - MIEIC

2015-11-22
