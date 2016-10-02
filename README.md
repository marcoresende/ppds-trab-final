# Trabalho Final - Java MVC 1.0

Disciplina: Plataformas de Produtividade no Desenvolvimento de Software  
Aluno: Marco T�lio T. Resende  


## T�picos


1. Descri��o da Tecnologia
2. Tecnologias Relacionadas
3. Caso de Uso
4. Livros e Sites para Aprendizado  


### 1. Descri��o da Tecnologia

O Java MVC 1.0 � uma das novas especifica��es do Java EE 8. Diferentemente do JSF, ele foi desenhado para ser um framework action-oriented, ou seja, � um framework orientado � a��es, enquanto o JSF � orientado � componentes.  

#### 1.1. Caracter�sticas

O modelo MVC refere-se a separa��o em camadas por Modelo(_Model_), Vis�o(_View_) e Controle(_Controller_). A camada de modelo est� relacionada aos dados da aplica��o, a camada de vis�o refere-se a apreseta��o dos dados da aplica��o e a camada de controle � respons�vel por gerenciar entrada de dados, chamar regras de neg�cio, atualizar o modelo e delegar qual vis�o ser� exibida.  

* **Model**  

A camada de modelo do Java MVC 1.0 � basicamente um HashMap definido no pacote _javax.mvc.Models_. Normalmente, este HashMap ser� manipulado pelos _controllers_ para prover uma mapeamento entre nomes e objetos.  

	// MVC 1.0 - source code of javax.mvc.Models
	public interface Models extends Map<String,Object>, Iterable<String> {
	}

Al�m do padr�o do pacote _javax.mvc.Models_, o MVC 1.0 suporta outro padr�o baseado em CDI (@Named). O padr�o de CDI � **recomendado** sobre o padr�o _Models_, entretando, � **opcional**.

* **View**  

Os dados mantidos no _Model_ precisam ser refletidos na _View_. Para isso, a _View_ pode utilizar express�es espec�ficas para o tipo de _View_. Por exemplo, se for retornado um objeto **Livro** que possui uma propriedade chamada **autor**, os dados ser�o exibidos da mesma maneira (geralmente utilizando Expression Language) de acordo com o tipo de _View_:  

> JSP view: ${livro.autor}

> Facelets view: #{livro.autor}

> Handlebars view: {{livro.autor}}

> Thymeleaf view: #{livro.autor}  

A implementa��o oficial do MVC 1.0 vem com v�rios mecanismos de _View_, sendo os principais deles o JSP e Facelets.  

* **Controller**

O _Controller_ � o c�rebro da aplica��o. � o respons�vel por combinar os _Models_ e _Views_ para atender �s requisi��es do usu�rio. No MVC 1.0, os _Controllers_ s�o implementados no estilo JAX-RS. Entretanto, h� algumas diferen�as e semelhan�as entre eles:  
* Um _Controller_ MVC � um recurso JAX-RS anotado com _@Controller_ (_javax.mvc.annotation.Controller_).
* A anota��o _@Controller_ pose ser anotada no escopo da classe ou do m�todo.
* Classes MVC devem utilizar somente CDI, e n�o classes JAX-RS nativas, EJB's, entre outras.
* Uma _String_ retornada por um _Controller_ � interpretada como um _path_ da _View_, n�o como conte�do de texto. Portanto, um recurso JAX-RS pode retornar um conte�do de texto, enquanto um _Controller_ MVC n�o.
* O padr�o para o _media type_ � _text/html_, por�m, pode ser utilizada a anota��o _@Produces_, como no JAX-RS, para indicar o tipo.
* Todos os par�metros "injet�veis" em um recurso JAX-RS, tamb�m est�o dispon�veis para um _Controller_ MVC.
* O ciclo de vida padr�o da inst�ncia de um classe, � por requisi��o no JAX-RS como no MVC. Entretanto, algumas implementa��es podem suportar outros ciclos de vida via CDI.

##### 1.1.1. Principais Anota��es

O Java MVC 1.0 traz algumas anota��es, sendo elas:

* @Controller (javax.mvc.annotation.Controller): Quando aplicado a n�vel de classe, define um Controller MVC. Quando aplicado no m�todo, define uma classe h�brida (controller e recurso JAX-RS).
* @View (javax.mvc.annotation.View): Quando aplicado � classe, aponta a view para todos os m�todos void do controller. Quando aplicado ao m�todo, aponta a view para este m�todo void do controller ou para um m�todo n�o void quando este retorna _null_.
* @CsrfValid (javax.mvc.annotation.CsrfValid): Somente pode ser aplicado ao n�vel de m�todo e requer que um token CSRF seja validado antes da invoca��o do controller. Falha na valida��o causa _ForbiddenException_.
* RedirectScoped (javax.mvc.annotation.RedirectScoped): Pode ser aplicado a tipo, m�todo, ou atributo; indica que um determinado bean est� no escopo de redirecionamento.

#### 1.2. Java MVC 1.0 vs JSF

Ambas as tecnologias s�o baseadas no modelo MVC, mas com algumas diferen�as:  

* Java MVC 1.0  

>	1. Action-based MVC.  
>	2. O design da p�gina Web fica a crit�rio do desenvolvedor.  
>	3. Processamento manual do par�metro de request.  
>	4. A tarefa de manter as valida��es/convers�es ficam por conta do desenvolvedor.  
>	5. Nenhum estado mantido entre as requisi��es.
>	6. Foco na requisi��o.
>	7. Suporte limitado para re�so.
	
* JSF

>	1. Component-based MVC
>	2. Componentes s�o renderizados pelo framework em c�digos HTML/JS nas p�ginas Web.
>	3. Processamento autom�tico do par�metro de request.
>	4. Tarefa de valida��es/convers�es tratadas pelo framework de acordo com as configura��es do desenvolvedor.
>	5. Por padr�o, estado � mantido sobre m�ltiplas requisi��es, mas o JSF tamb�m suporta requisi��es _stateless_.
>	6. Foco na p�gina.
>	7. Componentes favorecem o re�so.

![Figura 1: MVC 1.0 vs JSF](res/mvc_jsf.png "MVC 1.0 vs JSF")
