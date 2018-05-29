# Desenvolvendo um teste com Selenium Webdriver
## Abaixo um exemplo básico de um script de teste automatizado
## abriremos uma página web, iremos declarar um timeout para o carregamento dos elementos, procurar os elementos necessários e realizar a validação do que esperamos

```
package tests;
//Quando possuímos uma classe que existem diversos métodos estáticos, podemos mudar
// a forma de importar todos os métodos dessa classe (ex. abaixo) modo antigo: import org.junit.Assert.
import static org.junit.Assert.*;

import org.junit.Test;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

import java.util.concurrent.TimeUnit;

public class InformacoesUsuarioTest {
    @Test
    //todo método de teste precisa ser público / void = todo método de teste não pode retornar algum valor
    public void testAdicionarUmaInformacaoAdicionalDoUsuario() {

        //Abrindo o NAVEGADOR:
        //Aqui definimos que iremos utilizar o chromedriver
        System.setProperty("webdriver.chrome.driver", "C:\\drivers\\chromedriver.exe");

        // Utilizaremos a classe Webdriver, a mesma representa o "Browser", criaremos uma variável do tipo
        // webdriver (Navegador), que receberá uma especificação webdriver que é o chromedriver
        WebDriver navegador = new ChromeDriver();

        //Definimos um timeout de 5 segundos para aguardar que todos os elementos da página sejam carregados
        navegador.manage().timeouts().implicitlyWait(5, TimeUnit.SECONDS);

        //Maximizar o browser
        navegador.manage().window().maximize();

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


        //Validando se o login foi feito com sucesso (identificando o elemento pelo "class=me".
        //Procurando o elemento "me" dentro da classe
        WebElement me = navegador.findElement(By.className("me"));

        //Resgatando o valor desse elemento --> Hi Julio
        String textoNoElementoMe = me.getText();

        // Validar se o valor do elemento dentro da classe é o mesmo do que esperamos
        // É importante citarmos que precisamos definir um timeout (logo depois de abrir o browser) para evitarmos falhas nas validações
        assertEquals("Hi, Julio", textoNoElementoMe);

        //Fechando o navegador (quit = fecha todas as abas e o close = fecha apenas a aba corrente.
        navegador.quit();

    }
}
```
Alguns dos métodos mais utilizados em um WebElement são:
click() 
Para clicar em um determinado elemento.

sendKeys(String texto) 
Para atribuir um texto a um elemento, esse método pode ser mesclado com o uso da classe Keys, que possibilita, por exemplo, o digitar de teclas especiais como Shift, Enter, Backspace, etc.
https://seleniumhq.github.io/selenium/docs/api/java/org/openqa/selenium/Keys.html

getText() 
Retorna o texto contido entre o abrir e fechar da tag. No código <a id="mensagem">Este é meu texto</a>, o getText() retornaria "Este é meu texto".

clear() 
Limpa um campo que já possui um valor default.

getAttribute(String atributo) 
Retorna o texto contido em um atributo. No código <a id="mensagem">Este é meu texto</a>, o getAttribute("id") retornaria "mensagem".
```




[Voltar](https://github.com/andresilveiraleite/java_webdriver_novos_conceitos/blob/master/docs/b-automatizando-testes/001_automatizando.md) 

