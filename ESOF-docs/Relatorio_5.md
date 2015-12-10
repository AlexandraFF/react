# Relatório 5 - ESOF
## Facebook/React - Evolução do *Software*

### <a name="introducao"></a>Introdução

Nesta fase final do projeto desenvolvido no âmbito da unidade curricular de Engenharia de Software, os autores deste relatório procederam à identificação e posterior implementação de uma nova funcionalidade sobre a biblioteca React. O presente relatório tem como objetivo a exposição da nova funcionalidade desenvolvida. 

### <a name="feature"></a>Identificação de *feature*

O [*git*](https://github.com/facebook/react) do projecto React disponibiliza uma vasta [lista de issues](https://github.com/facebook/react/issues) encontrados por todos os contribuidores. Após uma análise cuidada da mesma, o grupo decidiu evoluir a [seguinte feature](https://github.com/facebook/react/issues/5549). Para a perceber melhor, o criador do seu tópico disponibilizou um [executável](http://jsbin.com/mifedepada/edit?js,console,output) e que depois de analisado percebemos que não havia a possibilidade de apanhar exeções quando o método *render* de um *ReactCompositeComponent* falhava.


### <a name="componentes"></a>Identificação de componentes que implementam a *feature*



### <a name="evolução"></a>Evolução da *feature*

A nossa abordagem então foi criar um novo método que é chamado quando o método *render* lança uma exeção. Mas surgiu-nos a dúvida em relação ao que iria de facto fazer esta implementação. Como o projeto *React* apresenta um nível de complexidade elevado, não quisemos fazer algo de muito complexo pois poderia interferir com outras funcionalidades já existentes e posteriormente não passar nos testes. Sendo assim, optamos por algo mais simples como imprimir para a consola a mensagem da exceção para aumentar as probabilidades de o *pull request* ser aceite. 

### <a name="submissao"></a>Submissão do *patch*

A *feature*, após a sua implementação, foi sujeita a um pedido de *pull-request*, que pode ser encontrado no [GitHub](https://github.com/facebook/react/pull/5615).

### <a name="analise"></a>Análise Crítica




### <a name="info"></a>Informações




##### Autores:

* António Casimiro (antonio.casimiro@fe.up.pt)
	* Número de horas despendidas: 
	* Contribuição: %
* Diogo Amaral (diogo.amaral@fe.up.pt)
	* Número de horas despendidas: 
	* Contribuição: %
* Pedro Silva (pedro.silva@fe.up.pt)
	* Número de horas despendidas: 
	* Contribuição: %
* Rui Cardoso (rui.peixoto@fe.up.pt)
	* Número de horas despendidas: 
	* Contribuição: %

Faculdade de Engenharia da Universidade do Porto - MIEIC

2015-12-13
