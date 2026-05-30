# Tratamento de Exceções

[📽 Veja esta vídeo-aula no Youtube](https://youtu.be/HtVJc_7FzYs)

---
> Let's face it, bad things happen. Networks partition, servers crash, remote endpoints become non-responsive. And when bad things happen, exceptions get thrown. And when exceptions get thrown, people die. Okay, maybe that's a bit dramatic, but the point is, exceptions are a fact of software development.

[Chris Patterson, MassTransit docs](https://masstransit.io/documentation/concepts/exceptions)

---
O .NET Core trata situações inesperadas dentro de um programa através do seu mecanismo de exceções. Segundo a [documentação](https://docs.microsoft.com/pt-br/dotnet/standard/exceptions/), uma exceção é "_qualquer condição de erro ou comportamento inesperado encontrado por um programa em execução_", ou seja, erros que não podem ser pegos pelo compilador já que só ocorrem com o programa em execução.

O correto uso das ferramentas para tratamento de exceções do C# é essencial para a criação de programas confiáveis e com uma experiência do usuário adequada.

## Capturando uma exceção

Quando um erro ocorre, o C# terá que finalizar o programa, a menos que haja algum tratamento por parte do programa. Fazemos esse tratamento envolvendo o código que poderá gerar o erro em um bloco `try...catch`:

```cs
try
{
    // comando(s) que podem gerar comportamentos inesperados
}
catch
{
    // código que será executado em caso de ocorrência de alguma exceção
}
```

O bloco `try` deve conter os comandos que contém erros em potencial. Ele será executado integralmente caso nenhuma exceção seja disparada, e depois seguirá para o primeiro comando após o bloco `catch`.

O bloco `catch` só será executado caso alguma exceção ocorra.

## Obtendo informações sobre a exceção

Você pode saber mais sobre o erro ocorrido através da seguinte sintaxe:

```cs
try
{
    // ...
}
catch (Exception e)
{
    // ...
}
```

Dessa maneira, `e` (ou qualquer outro nome que você quiser) é uma variável que contém todas as informações sobre o erro. As mais importantes:

* `Message` contém a mensagem;
* `GetType()` retorna o tipo da exceção.

Ex.:

```cs
// ...
catch(Exception ex)
{
    Console.WriteLine($"Erro encontrado: {ex.Message}");
    Console.WriteLine($"Tipo da exceção: {ex.GetType()}");
}
```

## Relançando a exceção

Caso seja incluído um bloco `catch`, a exceção poderá não mais será processada pelo .NET. Nessas situações o programa continuará normalmente, a menos que o programador especifique que a exceção deve ser relançada.

Para relançar uma exceção:

```cs
// ...
catch (Exception ex)
{
    // comandos de tratamento de erro...
    throw ex;
}
```

## Lançando uma exceção personalizada

Você pode lançar uma exceção personalizada em qualquer parte do seu código, de acordo com sua necessidade. Escolha o tipo adequado, instancie um objeto de exceção do tipo adequado e use `throw`.

Ex.:
```cs
throw new ArgumentException("O valor informado não é válido.");
```

Mais detalhes sobre boas práticas no lançamento de exceções na [documentação](https://docs.microsoft.com/pt-br/dotnet/standard/design-guidelines/using-standard-exception-types).

## Tipos de exceção

O .NET utiliza uma árvore de tipos de exceção. Segue uma lista resumida:

- `Exception`: todas as exceções
  - `IndexOutOfRangeException`: quando um arranjo é acessado com um índice inexistente
  - `NullReferenceException`: quando um objeto é acessado e contém o valor `null`
  - `InvalidOperationException`: quando uma operação é executada em um estado inválido
  - `DivideByZeroException`: quando acontece uma divisão por zero
  - `OverflowException`: quando um número está além do limite do tipo de dado esperado
  - `FormatException`: quando um dado está em formato inválido
  - `ArgumentException`: todas as exceções referentes a argumentos inválidos
    - `ArgumentNullException`: quando um argumento é passado incorretamente com o valor `null`
    - `ArgumentOutOfRangeException`: quando um argumento está fora dos limites esperados

Uma lista detalhada pode ser encontrada na [documentação](https://docs.microsoft.com/pt-br/dotnet/standard/exceptions/#common-exceptions).

## Múltiplos `catch`'s

Você pode utilizar múltiplos blocos `catch` para tratar diferentes tipos de erros. Use os tipos mais específicos primeiro, já que ao entrar em um bloco, os demais não serão executados. Veja um exemplo abaixo, no programa da vídeo-aula.

Programa da [vídeo-aula](https://youtu.be/HtVJc_7FzYs):

```cs
decimal x, y, r;

try
{
    // comandos passíveis de exceção
    Console.Write("x = ");
    x = Convert.ToDecimal(Console.ReadLine());

    Console.Write("y = ");
    y = Convert.ToDecimal(Console.ReadLine());

    if (x < 0 || y < 0)
    {
        throw new ArgumentException("Não são aceitos números negativos.");
    }

    r = x / y;

    Console.WriteLine($"r = {r:N}");
}
catch(DivideByZeroException)
{
    Console.WriteLine("Não é possível dividir por zero.");
}
catch(ArgumentException)
{
    Console.WriteLine("Não são aceitos números negativos.");
}            
catch(OverflowException)
{
    Console.WriteLine("Número inválido.");
}
catch(FormatException)
{
    Console.WriteLine("Número em formato inválido.");
}
catch(Exception ex)
{
    Console.WriteLine($"Erro genérico: {ex.Message}");
    Console.WriteLine($"Tipo: {ex.GetType()}");
    throw ex;
}
Console.WriteLine("Final do programa.");
```
