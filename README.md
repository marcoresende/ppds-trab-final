# Trabalho Final - Java MVC 1.0

Disciplina: Plataformas de Produtividade no Desenvolvimento de Software  
Aluno: Marco T�lio T. Resende  


## T�picos


1. Descri��o da Tecnologia
2. Tecnologias Relacionadas
3. Caso Real de Uso da Tecnologia
4. Livros e Sites para Aprendizado  
5. C�digo M�nimo que Mostre a Tecnologia em Contexto 


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
Fonte: [Introduction to the New MVC 1.0](http://www.developer.com/java/ent/introduction-to-the-new-mvc-1.0-ozark-ri.html "Anghel, 2016")  

### 2. Tecnologias Relacionadas  

#### 2.1. [Spring MVC](https://spring.io/ "Spring Framework") 

O Spring � um framework de aplica��o de c�digo-fonte aberto popular que pode facilitar o desenvolvimento do Java EE. Ele consiste em um cont�iner, um framework para gerenciar componentes, e um conjunto de servi�os de snap-in para interfaces de usu�rio, transa��es e persist�ncia da Web. Uma parte do Spring Framework � o Spring Web MVC, um framework MVC extens�vel para cria��o de aplica��es Web [NETBEANS, 2016].  
O Spring MVC � desenhado em torno de um _DispatcherServlet_ que despacha requisi��es para _handlers_. O _handler_ padr�o � baseado nas anota��es _@Controller_ e _@RequestMapping_, que oferecem uma larga flexibilidade para tratar m�todos. Com a introdu��o da vers�o 3.0, o mecanismo _@Controller_ tamb�m passou a permitir a cria��o de sites e aplica��es com RESTful, atrav�s da anota��o _@PathVariable_ e outras funcionalidades.  

#### 2.2. [VRaptor](http://www.vraptor.org/pt/ "VRaptor") 

VRaptor � um framework MVC web para desenvolvimento �gil com java. Criado em 2003 no IME-USP, teve sua vers�o 2.0 lan�ada em 2005 e a vers�o 3.0 em 2009. Atualmente mantido pela Caelum e diversos desenvolvedores de outras empresas. Utiliza muitas id�ias e boas pr�ticas que surgiram nos �ltimos anos, como Conven��o sobre Configura��o, Inje��o de Depend�ncias e um modelo REST. � tamb�m uma iniciativa brasileira, nascida dentro da Universidade de S�o Paulo.  
Tudo que se precisa fazer para criar um controller do VRaptor � adicionar a anota��o @Controller. A partir da� o framework j� utiliza suas conven��es de URLs e JSPs, exigindo o m�nimo de configura��es.  

#### 2.3. [Apache Struts 2](https://struts.apache.org/ "Struts")  

Apache Struts 2 � um framework de c�digo aberto para desenvolvimento de aplica��es Java EE. Ele utiliza e extende a Java Servlet API para incentivar os desenvolvedores a adotarem o modelo MVC. Algumas caracter�sticas do Struts 2 s�o:
* Plug-ins: aceita plug-ins disponibilizados por terceiros, assim, n�o exige que o framework venha com tudo e sim apenas as funcionalidades b�sicas.  
* Conven��es ao inv�s de configura��es: nomes de classes podem oferecer mapeamento de a��es, e valores de resultdos retornados podem oferecer nomes para as p�ginas JSP serem renderizadas.  
* Anota��o ao inv�s de configura��o XML: disponibiliza anota��es para serem utilizadas nas classes, reduzindo a configura��o XML.  
* Convers�o de dados: a convers�o de valores de campos de formul�rios baseados em String em objetos ou tipos primitivos, � tratada pela estrutura de suporte.  
* Inje��o de depend�ncia: reduz o acoplamento entre as camadas do aplicativo, tornando-o muito mais simples.  

#### 2.4. [Apache Tapestry](http://tapestry.apache.org/ "Apache Tapestry")   

Apache Tapestry � um framework de c�digo aberto para cria��o de aplica��es Java din�micas, robustas e altamente escal�veis. O Tapestry se baseia no padr�o Java Servlet API, e funciona em qualquer servlet container ou servidor de aplica��o. O framework divide a aplica��o Web em um conjunto de p�ginas, cada uma constru�da com componentes. Isto fornece uma estrutura consistente, onde o Tapestry assume responsabilidade pelas preocupa��es chaves, como constru��o da URL e despache, estado persistente no cliente ou servidor, valida��o de input do usu�rio, localiza��o/internacionaliza��o, e relat�rios de exce��o. O desenvolvimento de aplica��es com Tapestry envolve a cria��o de templates HTML, e a adi��o de uma classe java para cada.  

#### 2.5. [Play Framework](https://www.playframework.com/ "Play Framework")  

Play � um framework de c�digo aberto, desenvolvido na linguagem Scala e tamb�m utiliz�vel na linguagem Java. Utiliza o padr�o MVC e aumenta a produtividade utilizando o conceito de _conven��o sobre configura��o_. Play � baseado em uma arquitetura leve, stateless, amig�vel e possui como caracter�stica o consumo m�nimo de recursos (CPU, mem�ria, threads) para criar aplica��es altamente escal�veis.  

### 3. Caso Real de Uso da Tecnologia  

Como a previs�o para a release final da especifica��o do Java MVC 1.0 est� prevista para 2017, n�o foi encontrado nenhum caso real de uso com documenta��o dispon�vel na Web. Portanto, este item do trabalho n�o ser� abordado.  

### 4. Livros e Sites para Aprendizado

Ainda n�o existem muitos materiais dispon�veis para a nova especifica��o. Um dos motivos � que a especifica��o n�o foi completamente finalizada. N�o foram encontrados livros sobre o tema, entretanto existem alguns sites com introdu��o ao tema e at� alguns exemplos de implementa��o.

* [JSR 371: Model-View-Controller (MVC 1.0) Specification](https://www.jcp.org/en/jsr/detail?id=371 "JSR 371")  

* [Ozark - Implementa��o de Refer�ncia para o MVC 1.0](https://ozark.java.net/ "Ozark")  

* [Caelum - Primeiros passos com a especifica��o do MVC 1.0](http://blog.caelum.com.br/primeiros-passos-do-mvc-1-0/ "Caelum")  

* [Introduction to the New MVC 1.0 (Ozark RI)](http://www.developer.com/java/ent/introduction-to-the-new-mvc-1.0-ozark-ri.html "Ozark RI")  

* [MVC 1.0 in Java EE 8 - How to work with Controllers](http://www.bennet-schulz.com/2015/10/javaee-mvc-controllers.html)  

* [MVC 1.0 in Java EE 8 - Getting Started with NetBeans 8.1 and Payara 4.1](https://dzone.com/articles/mvc-10-in-java-ee-8-getting-started-with-netbeans)  

* [MVC 1.0 in Java EE 8: Getting Started Using Facelets](https://itblog.inginea.eu/index.php/mvc-1-0-in-java-ee-8-getting-started-using-facelets/)  

* [Shipping MVC 1.0 into Glassfish 5](https://istanbul-jug.org/2016/03/shipping-mvc-1-0-into-glassfish-5/)

### 5. C�digo M�nimo que Mostre a Tecnologia em Contexto  

O c�digo contido neste reposit�rio foi obtido atrav�s do tutorial disponibilizado no seguinte site:  

[Shipping MVC 1.0 into Glassfish 5](https://istanbul-jug.org/2016/03/shipping-mvc-1-0-into-glassfish-5/)  

#### 5.1. Configura��o  

* Baixar o Glassfish 5 nightly build no seguinte link:  

	[http://download.oracle.com/glassfish/5.0/nightly/index.html](http://download.oracle.com/glassfish/5.0/nightly/index.html)  

* Extrair o zip baixado e copiar o [javax.mvc-1.0.jar](https://github.com/rahmanusta/ozark/releases/tag/v1.0)  para o diret�rio $GF_HOME/glassfish/modules. Ap�s, v� para o diret�rio bin pelo terminal e inicialize o Glassfish com o seguinte comando:

	asadmin start-domain  
	
* Construa o projeto utilizando o Maven no reposit�rio do c�digo de demonstra��o:

	mvn clean install  

* Ser� gerado um arquivo "mvc.jar" no diret�rio _target_. Execute o seguinte comando para realizar o deploy do demo no Glassfish:

	asadmin deploy target/mvc.war  
	
* Acesse o navegador no endere�o [http://localhost:8080/mvc/app/person](http://localhost:8080/mvc/app/person) e teste a aplica��o.

![Demonstra��o](res/appdemo.png) 

