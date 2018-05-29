# Identificando Elementos
## Podemos identificar os elementos de uma página por ID, Name, Text, etc.
### Isso depende muito da forma que a aplicação foi desenvolvida.

Seguem alguns exemplos em como identificar os elementos:

1. Quadro de Exemplo Geral
 ![](/imagens/01_dentificando-elementos.png)  

2. Busca do elemento por ID - Lembrando que usar ‘id’ é a maneira mais fácil e segura de localizar um elemento em HTML. 
 ![](/imagens/02_id.png) 

3. Busca do elemento por "name": Os atributos ‘name’ são usados nos controles de formulário (forms) como campos de texto e botões de ‘radio button’. Os valores dos atributos de nome são passados para o servidor quando um formulário é enviado. Em termos de menor probabilidade de uma alteração, o atributo nome é um segundo ID.
 ![](/imagens/03_name.png) 

4. Busca do elemento por "Link Text":
Somente para hiperlinks. Usar o texto de um link provavelmente é a maneira mais direta de clicar em um link, pois ele é o que vemos na página mesmo sem inspeciona-la.
![](/imagens/04_link-text.png)

5. Busca do elemento por "Partial Link Text":
O Selenium permite que você identifique um hiperlink com parte de um texto. Isso pode ser bastante útil quando o texto é gerado dinamicamente. Em outras palavras, o texto em uma página da Web pode ser diferente na sua próxima visita. Podemos usar o texto comum compartilhado por esses textos de link dinamicamente gerados para identificá-los.
![](/imagens/05_partial-link-text.png)

6. Busca do elemento por "XPath":
XPath, o XML Path Language, é uma linguagem de consulta para selecionar nós de um XML documento. Quando um navegador renderiza uma página da Web, ela é analisada em uma árvore DOM (Document Objetc Model) ou similar. XPath pode ser usado para encaminhar um determinado nó na árvore DOM. Se isso soa um pouco técnico demais para você, não se preocupe, lembre-se de XPath é a maneira mais poderosa de encontrar um controle web específico.
![](/imagens/06_xpath.png)

### Evite usar o XPath gerado de ferramentas ou plugins de navegadores
Algumas ferramentas disponibilizadas pro extensões de navegadores facilitam muito na hora de selecionar os elementos diretamente na camada de apresentação da página, porém nem sempre o formato correto é selecionado.

No Chrome ou nos demais navegadores, quando aciona a tecla [F12], abre a Ferramente do Desenvolvedor do Navegador. Ela sim é bastante útil para selecionar corretamente o XPath através de um simples passo.

Primeiro: Localize o seu alvo através da inspeção de elemento:
![](/imagens/ex-localizar-elemento.png)

### Veja no exemplo que existe uma série de locators para o campo de busca do Google como:
- name=”q”
- id=” lst-ib”
- class=”gsfi”
porém precisamos do XPath e para isso basta clicar com o botão direito do mouse sobre o element id=”lst-ib” por exemplo e selecionar a opção “Copy XPath”

![](/imagens/copy-xpath.png)

### Para ver como funciona, clique [Ctrl + F] na ferramenta de desenvolvedor e cole o resultado e veja o que acontece:
![](/imagens/modo-dev-chrome.png)

### Ele te dá a opção de realizar a busca por uma String, selector ou XPath. Vamos colar o resultado na busca de elementos
![](/imagens/busca-por-string.png)

***  Veja que todo o bloco relacionado ao campo de busca do Google foi selecionado.
Mesmo sendo a maneira mais direta de acessar um elemento da página Web, se por ventura um desenvolvedor acrescentar ou remover uma ‘div’ o seu selector pode ser alterado.


7. Busca do elemento por "Tag name": Existe um conjunto limitado de nomes de tags em HTML. Em outras palavras, muitos elementos compartilham a mesma tag em uma página. Normalmente não usamos o localizador ‘tag_name’ para localizar um elemento. Mas existe uma exceção:

![](/imagens/07_tag-name.png)

A declaração de teste acima retorna a exibição do texto de uma pagina. Isso é muito útil como o Selenium WebDriver não possui nenhum método interno para retornar o texto de uma página Web.

8. Busca do elemento por "class": O atributo de ‘class’ é um elemento HTML usado para a folha de estilo. Também pode ser usado para identificar elementos. É comum o atributo ‘class’ de um elemento ter vários valores:
![](/imagens/09_class.png)

Você pode utilizar qualquer um desses exemplos:
![](/imagens/09_class_2.png)

Já o exemplo abaixo retorna um erro, pois utiliza um nome composto de classe e isso não é permitido pelo WebDriver:
![](/imagens/09_class_3.png)

O localizador de ‘class’ é conveniente para testar bibliotecas JavaScript / CSS (como o TinyMCE) que normalmente usa um conjunto de nomes de classes definidos.
![](/imagens/09_class_4.png)

Localizando pelo CSS Path (CSS Selector)
Você pode usar o caminho do CSS para localizar um elemento Web
![](/imagens/09_class_5.png)

8. Busca por uma cadeia de elementos para obter um locator: 
Para uma página web que contém mais de um elemento com os mesmos atributos como o exemplo abaixo, poderíamos o locator XPath
![](/imagens/13_cadeia-de-elementos.png)

Porém existe uma outra maneira e encontrar um elemento que seria usando uma cadeia de elementos:
![](/imagens/14_cadeia-de-elementos_2.png)

Veja que no exemplo acima localizamos a ‘div2' pelo id e depois procurei dentro desse ‘id’ um elemento pelo name “nome”.

9. Busca em uma combobox:

Quando a página exibe uma combobox:
![](/imagens/15_combo-box.png)

Podemos selecionar um dos elementos da seguinte maneira:
![](/imagens/16_combo-box_2.png)

Observação: O comando “Selenium::WeDriver::Support::Select.new() dá permissão para o Webdrive realizar uma seleção do elemento da página web passada como parâmetro. E o que estamos passamos por parâmetro é o comando [@driver.find_element(:id, “month”)).select_by(:text, “Maio”]

Depois executamos o .click no elemento find_element(:id, “month”).



### Caso você não conheça muito sobre HTML e seus elementos, segue abaixo dois links que poderão lhe ajudar bastante:

## Elementos mais comuns no HTML

https://www.w3schools.com/html/html_elements.asp

## Atributos constantemente usados em elementos

https://www.w3schools.com/html/html_attributes.asp




[Voltar](https://github.com/andresilveiraleite/java_webdriver_novos_conceitos/blob/master/docs/a-introducao/001_introducao.md)  




