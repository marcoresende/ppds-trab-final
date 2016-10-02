# Trabalho Final - Java MVC 1.0

Disciplina: Plataformas de Produtividade no Desenvolvimento de Software  
Aluno: Marco Túlio T. Resende  


## Tópicos


1. Descrição da Tecnologia
2. Tecnologias Relacionadas
3. Caso de Uso
4. Livros e Sites para Aprendizado  


### 1. Descrição da Tecnologia

O Java MVC 1.0 é uma das novas especificações do Java EE 8. Diferentemente do JSF, ele foi desenhado para ser um framework action-oriented, ou seja, é um framework orientado à ações, enquanto o JSF é orientado à componentes.  

#### 1.1. Características

O modelo MVC refere-se a separação em camadas por Modelo(_Model_), Visão(_View_) e Controle(_Controller_). A camada de modelo está relacionada aos dados da aplicação, a camada de visão refere-se a apresetação dos dados da aplicação e a camada de controle é responsável por gerenciar entrada de dados, chamar regras de negócio, atualizar o modelo e delegar qual visão será exibida.  

* **Model**  

A camada de modelo do Java MVC 1.0 é basicamente um HashMap definido no pacote _javax.mvc.Models_. Normalmente, este HashMap será manipulado pelos _controllers_ para prover uma mapeamento entre nomes e objetos.  

	// MVC 1.0 - source code of javax.mvc.Models
	public interface Models extends Map<String,Object>, Iterable<String> {
	}

Além do padrão do pacote _javax.mvc.Models_, o MVC 1.0 suporta outro padrão baseado em CDI (@Named). O padrão de CDI é **recomendado** sobre o padrão _Models_, entretando, é **opcional**.

* **View**  

Os dados mantidos no _Model_ precisam ser refletidos na _View_. Para isso, a _View_ pode utilizar expressões específicas para o tipo de _View_. Por exemplo, se for retornado um objeto **Livro** que possui uma propriedade chamada **autor**, os dados serão exibidos da mesma maneira (geralmente utilizando Expression Language) de acordo com o tipo de _View_:  

> JSP view: ${livro.autor}

> Facelets view: #{livro.autor}

> Handlebars view: {{livro.autor}}

> Thymeleaf view: #{livro.autor}  

A implementação oficial do MVC 1.0 vem com vários mecanismos de _View_, sendo os principais deles o JSP e Facelets.  

* **Controller**

O _Controller_ é o cérebro da aplicação. É o responsável por combinar os _Models_ e _Views_ para atender às requisições do usuário. No MVC 1.0, os _Controllers_ são implementados no estilo JAX-RS. Entretanto, há algumas diferenças e semelhanças entre eles:  
* Um _Controller_ MVC é um recurso JAX-RS anotado com _@Controller_ (_javax.mvc.annotation.Controller_).
* A anotação _@Controller_ pose ser anotada no escopo da classe ou do método.
* Classes MVC devem utilizar somente CDI, e não classes JAX-RS nativas, EJB's, entre outras.
* Uma _String_ retornada por um _Controller_ é interpretada como um _path_ da _View_, não como conteúdo de texto. Portanto, um recurso JAX-RS pode retornar um conteúdo de texto, enquanto um _Controller_ MVC não.
* O padrão para o _media type_ é _text/html_, porém, pode ser utilizada a anotação _@Produces_, como no JAX-RS, para indicar o tipo.
* Todos os parâmetros "injetáveis" em um recurso JAX-RS, também estão disponíveis para um _Controller_ MVC.
* O ciclo de vida padrão da instância de um classe, é por requisição no JAX-RS como no MVC. Entretanto, algumas implementações podem suportar outros ciclos de vida via CDI.


