# Iniciando um novo projeto e realizando a operação de abertura do navegador
## Inicialmente iremos entender como abrir o browser utilizando o Webdriver
```
public class AberturaBrowser {
    @Test
    //todo método de teste precisa ser público / void = todo método de teste não pode retornar algum valor
    public void testAdicionarUmaInformacaoAdicionalDoUsuario() {

        //Abrindo o NAVEGADOR:
        //Aqui definimos que iremos utilizar o chromedriver
        System.setProperty("webdriver.chrome.driver", "C:\\drivers\\chromedriver.exe");

        // Utilizaremos a classe Webdriver, a mesma representa o "Browser", criaremos uma variável do tipo
        // webdriver (Navegador), que receberá uma especificação webdriver que é o chromedriver
        WebDriver navegador = new ChromeDriver();

        //Maximizar o browser
        navegador.manage().window().maximize();

        //Navegar para a página desejada
        navegador.get("http://www.juliodelima.com.br/taskit");

        //Fechando o navegador
        //navegador.close();

    }
}
```


[Voltar](https://github.com/andresilveiraleite/java_webdriver_novos_conceitos/blob/master/docs/a-introducao/001_introducao.md)  

