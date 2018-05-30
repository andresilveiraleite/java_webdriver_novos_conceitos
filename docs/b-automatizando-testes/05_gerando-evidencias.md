# Gerando Evidências de Teste e os devidos screencshots

1. Criando uma classe para obter a data + hora para o arquivo a ser gerado da evidência:

```
package suporte;

import java.sql.Timestamp;
import java.text.SimpleDateFormat;

public class Generator {
    public static String dataHoraParaArquivo() {
        Timestamp ts = new Timestamp(System.currentTimeMillis());
        return new SimpleDateFormat("yyyyMMddhhmmss").format(ts);
    }
}
```

2. Criando uma classe para gerar o arquivo de evidência:
```
package suporte;

import org.apache.commons.io.FileUtils;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;

import java.io.File;

public class Screenshot {
    public static void tirar(WebDriver navegador, String arquivo) { // Pegar o status atual do navegador + string de arquivo
        // Criar variável do tipo File que vai ser nosso screenshot
        File screenshot = ((TakesScreenshot)navegador).getScreenshotAs(OutputType.FILE);
        try {
            // Copiar o arquivo gerado para uma pasta
            FileUtils.copyFile(screenshot, new File(arquivo));
        } catch (Exception e) {
            System.out.println("Houveram problemas ao copiar o arquivo para a pasta: " + e.getMessage());
        }
    }
}
```

3. Agora um exemplo de script de teste e onde incluir o screenshot:
```
package tests;
//Quando possuímos uma classe que existem diversos métodos estáticos, podemos mudar
// a forma de importar todos os métodos dessa classe (ex. abaixo) modo antigo: import org.junit.Assert.
import static org.junit.Assert.*;

import org.junit.After;
import org.junit.Before;
import org.junit.Rule;
import org.junit.Test;
import org.junit.rules.TestName;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedCondition;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.Select;
import org.openqa.selenium.support.ui.WebDriverWait;
import suporte.Generator;
import suporte.Screenshot;

import java.util.concurrent.TimeUnit;

public class InformacoesUsuarioTest {
    private WebDriver navegador;

    @Rule
    public TestName test = new TestName();

    @Before
    public void setUp(){
        //Abrindo o NAVEGADOR:
        //Aqui definimos que iremos utilizar o chromedriver
        System.setProperty("webdriver.chrome.driver", "C:\\drivers\\chromedriver.exe");

        // Utilizaremos a classe Webdriver, a mesma representa o "Browser", criaremos uma variável do tipo
        // webdriver (Navegador), que receberá uma especificação webdriver que é o chromedriver
        navegador = new ChromeDriver();

        //ESPERA IMPLÍCITA - Definimos um timeout de 5 segundos para aguardar que todos os elementos da página sejam carregados
        navegador.manage().timeouts().implicitlyWait(5, TimeUnit.SECONDS);

        //Navegar para a página desejada
        navegador.get("http://www.juliodelima.com.br/taskit");

        //Maximizar o browser
        navegador.manage().window().maximize();

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

    }

    //@Test
    // É importante salientar que todo scrip de teste "@test" vai sempre chamar o @before e @after
    //todo método de teste precisa ser público / void = todo método de teste não pode retornar algum valor
    public void testAdicionarUmaInformacaoAdicionalDoUsuario() {
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

    @Test
    public void removerContato() {
        //Clicar no elemento pelo xpath //span[text()='texto']/following-sibling::a
        navegador.findElement(By.xpath("//span[text()='felipe@felipe.com.br']/following-sibling::a")).click();

        //Confirmar a janela java script - Utilizando o método switchTo:
        navegador.switchTo().alert().accept(); //accept = OK dismiss = Cancelar

        //Validar mensagem de sucesso de exclusão do telefone
        //Encontrando o elemento da mensagem
        WebElement msgDelContato = navegador.findElement(By.id("toast-container"));

        //Resgatando o valor da mensagem --> Your Contact has been added
        String resgateTextoMsg = msgDelContato.getText();

        // Na mensagem de id "toast-container" validar que o texto é "Rest in peace, dear phone"
        assertEquals("Rest in peace, dear email!", resgateTextoMsg);

        // DEVEMOS INCLUIR O SCREENSHOT, AONDE ACHARMOS NECESSÁRIO UMA EVIDÊNCIA... NEsse caso, logo depois do assert, para constatarmos que realizamos os testes de forma correta.
        
        String screenshotArquivo = "C:\\Users\\155 X-MX\\Desktop\\screenshots_webdriver\\curso_java_webriver" + Generator.dataHoraParaArquivo() + test.getMethodName() + ".png";
        Screenshot.tirar(navegador, screenshotArquivo);


         // --> Lembrando que essa mensagem pode demorar a aparece e desaparecer
        // Nesse caso, precisaremos criar uma ESPERA EXPLÍCITA e aguardar 10 segundos para essa msg desaparecer, para depois realizar o logout

        WebDriverWait aguardar = new WebDriverWait(navegador, 10);
        //Definimos uma variável que recebe uma espera explícita, definindo
        // Aonde vamos aguarda dentro do "Navegador" e os segundos

        aguardar.until(ExpectedConditions.stalenessOf(msgDelContato));
        // Aguardar até que "stalenessof = o elemento "Webelement" desapareça do DOM" no nosso caso a msg de sucesso de exclusão.

        //Clicar no link com o texto "Logout"
        navegador.findElement(By.linkText("Logout")).click();
    }

    @After
    public void tearDown() {
        //Fechando o navegador (quit = fecha todas as abas e o close = fecha apenas a aba corrente.
        navegador.quit();
    }
}

```

[Voltar](https://github.com/andresilveiraleite/java_webdriver_novos_conceitos/blob/master/docs/b-automatizando-testes/001_automatizando.md) 

