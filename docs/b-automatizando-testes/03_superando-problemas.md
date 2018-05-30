# Superando problemas em Combo Boxes e Toast Messages
## Aqui iremos tratar dois assuntos muito interessantes
### A prática herdada do Padrão AAA, que refere-se a Arrange, Act e Assert e do uso da classe "Select" do Webdriver para interagirmos com comboboxes

Exemplo real:
```
package tests;
//Quando possuímos uma classe que existem diversos métodos estáticos, podemos mudar
// a forma de importar todos os métodos dessa classe (ex. abaixo) modo antigo: import org.junit.Assert.
import static org.junit.Assert.*;

import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.Select;

import java.util.concurrent.TimeUnit;

public class InformacoesUsuarioTest {
    private WebDriver navegador;

    @Before
    public void setUp(){
        //Abrindo o NAVEGADOR:
        //Aqui definimos que iremos utilizar o chromedriver
        System.setProperty("webdriver.chrome.driver", "C:\\drivers\\chromedriver.exe");

        // Utilizaremos a classe Webdriver, a mesma representa o "Browser", criaremos uma variável do tipo
        // webdriver (Navegador), que receberá uma especificação webdriver que é o chromedriver
        navegador = new ChromeDriver();

        //Definimos um timeout de 5 segundos para aguardar que todos os elementos da página sejam carregados
        navegador.manage().timeouts().implicitlyWait(5, TimeUnit.SECONDS);

        //Maximizar o browser
        navegador.manage().window().maximize();
    }

    @Test  //É importante salientar que todo scrip de teste "@test" vai sempre chamar o @before e @after

    //todo método de teste precisa ser público / void = todo método de teste não pode retornar algum valor
    public void testAdicionarUmaInformacaoAdicionalDoUsuario() {
        //Navegar para a página desejada
        navegador.get("http://www.juliodelima.com.br/taskit");

        // Início das buscas por elementos das páginas e interações
        //Procurar o elemento e Clicar no link que possui o texto "Sign in" e clicar no mesmo.
        //1 modo - Procurarmos um elemento e clicar no mesmo
        navegador.findElement(By.linkText("Sign in")).click(); //O findElement permite buscar um elemento dentro de outro By by

        //2 modo - Declararar uma variável Webelement e receber a busca pelo elemento e logo abaixo utilizar o clique.
        // É útil quando precisamos utilizar várias vezes um elemento.
        //WebElement linkSignin = navegador.findElement(By.linkText("Sign in"));
        //linkSignin.click();

        //Identificando o formulário de login
        WebElement formularioSignInbox = navegador.findElement(By.id("signinbox"));

        // Preenchendo o campo name "login" que está dentro do formulário de login --> julio0001
        formularioSignInbox.findElement(By.name("login")).sendKeys("julio0001");

        //Preencher o campo name "password" que está dentro do formulário de login --> 123456
        formularioSignInbox.findElement(By.name("password")).sendKeys("123456");

        //Clicar no link SIGNIN (utilizando o text do elemento)
        navegador.findElement(By.linkText("SIGN IN")).click();

        // Clicar em um link que possui a class "me"
        navegador.findElement(By.className("me")).click();

        // Clicar em um link que possua o texto more data about you
        navegador.findElement(By.linkText("MORE DATA ABOUT YOU")).click();

        // procurar elemento pelo xpath -- Clicar no botão ADD MORE DATA
        navegador.findElement(By.xpath("//div[@id='moredata']//button[@data-target='addmoredata']")).click();

        // Identificar a popup onde está o formulário de id addmoredata
        WebElement formularioPopup = navegador.findElement(By.id("addmoredata"));

        // Na combo de name "type" escolher a opção phone
        //1 identificamos o elemento type do combobox
        WebElement nameType = formularioPopup.findElement(By.name("type"));

        // Webdriver cria uma interface específica para combobox, utilizando o elemento type encontrado acima
        // Podemos encontrar por "INDEX" = 0, 1, etc
        //new Select(nameType).selectByIndex(0);

        //podemos encontrar também por texto - no caso phone
        new Select(nameType).selectByVisibleText("Phone");


        // Preencher o campo de name "contact" digitar: telefone.
        formularioPopup.findElement(By.name("contact")).sendKeys("552122345678");

        // clicar no link de text "Save" que está na popup
        formularioPopup.findElement(By.linkText("SAVE")).click();

        //Validar mensagem de sucesso de add contato
        //Encontrando o elemento da mensagem
        WebElement msgAddContato = navegador.findElement(By.id("toast-container"));

        //Resgatando o valor da mensagem --> Your Contact has been added
        String resgateTextoMsg = msgAddContato.getText();

        // Na mensagem de id "toast-container" validar que o texto é "Your Contact has been added"
        assertEquals("Your contact has been added!", resgateTextoMsg);

    }

    @After
    public void tearDown() {
        //Fechando o navegador (quit = fecha todas as abas e o close = fecha apenas a aba corrente.
        navegador.quit();
    }
}

```
### O padrão AAA foi herdado do teste de unidade, veja mais no link abaixo:
https://www.typemock.com/unit-test-patterns-for-net#aaa

### Veja abaixo a relação completa de métodos disponíveis para interação com comboboxes:
http://seleniumhq.github.io/selenium/docs/api/java/index.html

[Voltar](https://github.com/andresilveiraleite/java_webdriver_novos_conceitos/blob/master/docs/b-automatizando-testes/001_automatizando.md) 

