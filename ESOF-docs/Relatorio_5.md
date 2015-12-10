# Relatório 5 - ESOF
## Facebook/React - Evolução do *Software*

### <a name="introducao"></a>Introdução

Nesta fase final do projeto desenvolvido no âmbito da unidade curricular de Engenharia de Software, os autores deste relatório procederam à identificação e posterior implementação de uma nova funcionalidade sobre a biblioteca React. O presente relatório tem como objetivo a exposição da nova funcionalidade desenvolvida, bem como de algumas reflexões gerais sobre o projeto React.


### <a name="feature"></a>Identificação da Funcionalidade

O [repositório](https://github.com/facebook/react) do projeto React disponibiliza uma vasta [lista de *issues*](https://github.com/facebook/react/issues) que é alimentada pelos seus colaboradores, como já foi referido em [relatórios anteriores](./Relatorio_2.md#levantamento). Após uma análise cuidada da mesma, o grupo decidiu evoluir uma funcionalidade que tem como base os *issues* [#2461](https://github.com/facebook/react/issues/2461) e [#5549](https://github.com/facebook/react/issues/5549).

Como já foi explicado em relatórios anteriores, a funcionalidade do React baseia-se na definição de [componentes](http://facebook.github.io/react/docs/component-specs.html) que, quando instanciados, constituem um [DOM virtual](http://facebook.github.io/react/docs/glossary.html). Ao criar um componente, é necessário definir um [método `render()`](http://facebook.github.io/react/docs/component-specs.html#render), que especifica como o componente será apresentado no ecrã. Este método é chamado de forma assíncrona, apenas quando um componente sofre modificações que tornem necessária a realização de alterações sobre o DOM da página. Esta funcionalidade é vantajosa a nível de desempenho da aplicação, mas faz com que não seja possível apanhar exceções que sejam lançadas pelo método `render()` de forma tradicional. Por exemplo, o seguinte excerto de código não permite apanhar uma exceção lançada pelo método `render()` da classe `MyComponent`.

```javascript
try {
React.render(
  <MyComponent name="Joe Schmoe"/>,
  document.getElementById('example')
); 
} catch (e) {
  console.error('err', e)
}
```

Assim, os autores deste relatório propuseram-se a estender a funcionalidade da biblioteca React de modo a permitir o tratamento de exceções lançadas pelo método `render()` de um componente. A solução proposta consiste em possibilitar a definição de um método, que deverá ter o nome `exceptionCallBack`, que deverá ser chamado de cada vez que uma exceção é lançada no corpo do método `render()`. Desta forma, o seguinte bloco de código é capaz de produzir o resultado esperado, sendo a mensagem **err** escrita na consola de cada vez que há um clique do rato sobre o componente.

```javascript
var a = false;

var MyComponent = React.createClass({
	render: function() {
		if (this.props.a) {
			throw 'err';
		}

		return (      
			<h1 onClick={ () => {a = true; run()} }>Hello {this.props.name}</h1>      
		);
	},
	exceptionCallBack: function(e) {
		console.log(e);
	}	
});

function run() {
	ReactDOM.render(
		<MyComponent a={a} name={"Rui Cardoso"} />,
		document.getElementById('example')
	);
}

ReactDOM.render(<MyComponent a={false} name={"Rui Cardoso"} />, document.getElementById('example'));
```

#### <a name="componentes"></a>Identificação dos Componentes que Implementam a Funcionalidade

O módulo da biblioteca React que implementa o *rendering* de um componente no DOM virtual designa-se [**ReactCompositeComponent**](https://github.com/facebook/react/blob/master/src/renderers/shared/reconciler/ReactCompositeComponent.js). [O método](https://github.com/rppc/react/blob/feature/src/renderers/shared/reconciler/ReactCompositeComponent.js#L787) que realiza a atualização do DOM da página foi alterado pelos autores deste relatório de forma a contemplar a possibilidade de exceções lançadas no método `render()` de cada componente, chamando a função `exceptionCallBack` caso esta tenha sido definida para esse componente.

### <a name="submissao"></a>Submissão de um *Pull Request*

Após a implementação da nova funcionalidade, os autores [submeteram um *pull request*](https://github.com/facebook/react/pull/5615) com vista à sua integração na biblioteca React.

### <a name="analise"></a>Análise Crítica

Apesar de a funcionalidade escolhida ser de um nível de complexidade que pode ser considerado médio-elevado, os autores deste relatório consideram que essa dificuldade foi acentuada por alguns problemas na organização do código do projeto. De facto, há algumas falhas na organização dos módulos e das classes do projeto que podem estar relacionadas com falta de planeamento adequado e alguma incapacidade por parte da equipa proprietária do React em preparar, no início da sua génese, o projeto para a dimensão que este iria, eventualmente, atingir.

Os autores deste relatório não deixam, no entanto, de salientar o grande potencial que este projeto *open-source* tem em termos de evolução do *software*, numa altura em que a biblioteca React é cada vez mais utilizada para o desenvolvimento de aplicações Web e de aplicações nativas para Android e iOS.

### <a name="info"></a>Informações

##### Autores:

* António Casimiro (antonio.casimiro@fe.up.pt)
	* Número de horas despendidas: 6
	* Contribuição: 25%
* Diogo Amaral (diogo.amaral@fe.up.pt)
	* Número de horas despendidas: 6
	* Contribuição: 25%
* Pedro Silva (pedro.silva@fe.up.pt)
	* Número de horas despendidas: 6
	* Contribuição: 25%
* Rui Cardoso (rui.peixoto@fe.up.pt)
	* Número de horas despendidas: 6
	* Contribuição: 25%

Faculdade de Engenharia da Universidade do Porto - MIEIC

2015-12-13
