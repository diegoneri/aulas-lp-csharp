# Templates .NET 10

[📽 Veja esta vídeo-aula no YouTube](https://youtu.be/KZGT_pm9UlM)

Com o suporte às versões mais recentes do C#, o .NET 10 traz os _templates_ atuais de vários tipos de projetos para acompanhar a evolução da linguagem.

Com isso, aplicações `console` podem ser substancialmente simplificadas em termos de código inicial requerido. Essa alteração chama-se _top-level statements_, e traz ao C# uma maior simplicidade para aprendizagem e código mais limpo, bem como uma semelhança com outras linguagens populares como JavaScript, PHP e Python, por exemplo (se distanciando de uma de suas inspirações iniciais, a linguagem Java).

Boa parte desse curso foi feita em .NET em versões anteriores à 6 (3.1 e 5). Vamos entender aqui as mudanças e vamos explorar maneiras de se utilizar o código dos materiais já criados no contexto atual do .NET 10.

## SDK e _Runtimes_

SDKs são os softwares que instalamos para que possamos programar em .NET. Contém o compilador e tudo que é necessário para que, dado o código-fonte, possa ser criado o programa executável.

_Runtimes_, por sua vez, são necessários somente para que os programas sejam executados. Todo programa compilado em .NET necessita de um _runtime_ em versão compatível para rodar. De maneira geral, instalações Windows incorporam _runtimes_ de .NET nas versões mais recentes, mas sempre é possível instalá-los separadamente.

Quando se instala o SDK, ele sempre acompanha um _runtime_ compatível.

Para verificar o que está instalado em seu sistema, use:

SDKs instaladas:
```
dotnet --list-sdks
```

_Runtimes_ instaladas:
```
dotnet --list-runtimes
```

Informação completa sobre seu sistema:
```
dotnet --info
```

Exemplo de resposta de `dotnet --info` (`...` _representa conteúdo omitido_):

```
SDK do .NET (refletindo qualquer global.json):
 Version:   10.0.100
...

Ambiente de runtime:
 OS Name:     Windows
 OS Version:  10.0.19043
...

Host (useful for support):
  Version: 10.0.0
...

.NET SDKs installed:
  10.0.100 [C:\Program Files\dotnet\sdk]

.NET runtimes installed:
...
  Microsoft.NETCore.App 10.0.0 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
...

To install additional .NET runtimes or SDKs:
  https://aka.ms/dotnet-download
```

Perceba que esse ambiente está usando o SDK `10.0.100` e o _runtime_ `10.0.0` (veja em _Host_). São as versões mais recentes no momento em que escrevo esse texto.

Descubra quais versões são as mais recentes usando:

```
dotnet sdk check
```

[Aqui](https://aka.ms/dotnet-download) você pode ver e baixar versões mais recentes ou mais antigas. Você pode acompanhar por [aqui](https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-core) a política de versões da plataforma .NET.

## Diferenças nos _templates_ de aplicação `console` em .NET 5/6 e .NET 10

Ao usar `dotnet new console` até a versão 5 era criado um programa semelhante a esse:

Exemplo de solução do exercício `EtecAB`:

`EtecAB.csproj`:
```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

</Project>
```

`Program.cs`:
```cs
using System;

namespace EtecAB
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Etec Adolpho Berezin");
        }
    }
}
```

O mesmo programa, no _template_ atual do .NET 10, ficaria assim:

`EtecAB.csproj`:
```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net10.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>
```

`Program.cs`:
```cs
Console.WriteLine("Etec Adolpho Berezin");
```

### Mudanças em `.csproj`

A principal mudança é a diretiva `TargetFramework`. Ela indica qual _runtime_ será utilizado para rodar o programa. A versão indicada deve estar disponível no ambiente de execução.

- `netcoreapp3.1` exige algum .NET Runtime versão 3 (chamado .NET Core 3).
- `net5.0` exige algum .NET Runtime versão 5.
- `net10.0` exige algum .NET Runtime versão 10.

Caso você tenha um programa console escrito para .NET 5 ou .NET 6 e queira rodar em .NET 10, você pode somente mudar `<TargetFramework>net5.0</TargetFramework>` ou `<TargetFramework>net6.0</TargetFramework>` para `<TargetFramework>net10.0</TargetFramework>` que geralmente ele vai rodar (_não funcionará se forem necessários outros pacotes, pois eles têm que ser atualizados também_).

As outras duas linhas novas no _template_ do .NET 10 habilitam novos recursos da linguagem que permitem a simplificação do arquivo `Program.cs`.

### Mudanças em `Program.cs`

Programas escritos com o _template_ do .NET 10 não vão rodar em versões anteriores. A combinação de novos recursos da linguagem C# introduzidos nas versões mais recentes torna o código mais sucinto, porém incompatível com os _runtimes_ e SDKs anteriores.

Diversas partes do código inicial podem agora ser removidas.

- `using` agora é implícito para `System` e algumas outras bibliotecas frequentemente utilizadas;
- a indicação de uma função `Main` (em um `namespace` e `class`) não é mais necessária devido ao recurso chamado _top-level statements_ criado para que o compilador possa descobrir automaticamente o seu ponto de início.

Assim, só precisamos escrever o código que iria no bloco `Main` diretamente no arquivo `Program.cs`, sem nenhum texto adicional.

O código gerado com esses itens explícitos continua funcionando.

## Migrando código no modelo .NET 5 para o modelo .NET 10

Para migrar um código no modelo .NET 5 para um em modelo .NET 10, faça o seguinte:

Em `.csproj`:
- mude o `TargetFramework` para `net10.0`;
- adicione as diretivas `<ImplicitUsings>enable</ImplicitUsings>` e `<Nullable>enable</Nullable>`.

Em `Program.cs`:
- Exclua todas as linhas exceto aquelas no bloco `Main`.

Execute os procedimentos necessários no [VsCode](#VsCode).

Execute normalmente.

_Esse procedimento precisará de adaptações em programas que usem diretivas `using` diferentes daquelas implícitas, que usem pacotes NuGet, e que modifiquem membros da classe que contém `Main` (dados ou comportamentos). Se isso ocorrer, faça as mudanças pontualmente. Nesse curso, tenha atenção com o exemplos das aulas de [Pacotes](../content/pacotes.md), [Publicação e distribuição](../content/publish_dist.md), [Sub-rotinas](../content/subroutines.md) e na maioria dos Tópicos Adicionais._

## Como usar os exemplos de aula escritos no modelo .NET 5 tendo somente o .NET 10 instalado

Crie o projeto normalmente, no _template_ novo, usando `dotnet new console`.

Execute os procedimentos necessários no [VsCode](#VsCode).

Ao digitar os códigos, ignore o que estiver fora de `Main`. Digite somente os códigos que estão dentro de `Main` em seu `Program.cs`.

Por exemplo, [nesta correção](https://github.com/ermogenes/correcoes-dev-cs/blob/main/Cidades/Program.cs) é usado o código abaixo:

```cs
using System;

namespace Cidades
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Praia Grande");
            Console.WriteLine("Mongaguá");
            Console.WriteLine("Itanhaém");
            Console.WriteLine("Peruíbe");
        }
    }
}
```

Ao fazer em seu computador, deve ficar algo assim:

```cs
Console.WriteLine("Praia Grande");
Console.WriteLine("Mongaguá");
Console.WriteLine("Itanhaém");
Console.WriteLine("Peruíbe");
```

Execute normalmente.

## VsCode

Garanta que o VsCode está atualizado.

Instale o **C# Dev Kit** procurando por `ms-dotnettools.csdevkit` na aba de extensões.

O C# Dev Kit inclui automaticamente a extensão base de C# (`ms-dotnettools.csharp`) e o .NET Install Tool (`ms-dotnettools.vscode-dotnet-runtime`).

Com o C# Dev Kit instalado, o VsCode oferece suporte completo para _top-level statements_ e todos os recursos do .NET 10 sem configurações adicionais.
