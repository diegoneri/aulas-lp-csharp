# Operações lógicas

[📽 Veja esta vídeo-aula no Youtube](https://youtu.be/70RI1a5wN78)

## Operadores de comparação

Comparam dois valores e retornam um resultado booleano.

| Operador | Descrição        | Exemplo                     |
| -------- | ---------------- | --------------------------- |
| `==`     | Igual a          | `7.2 == -3` retorna `false` |
| `!=`     | Diferente de     | `7.2 != -3` retorna `true`  |
| `<`      | Menor que        | `7.2 < -3` retorna `false`  |
| `>`      | Maior que        | `7.2 > -3` retorna `true`   |
| `<=`     | Menor ou igual a | `5 <= 5` retorna `true`     |
| `>=`     | Maior ou igual a | `4 >= 5` retorna `false`    |

## Operadores lógicos (booleanos)

| Operador | Descrição                                                                  |
| -------- | -------------------------------------------------------------------------- |
| `!`      | Negação lógica (troca entre `true` e `false`)                              |
| `&&`     | "E" lógico (`true` se ambos `true`, senão `false`)                         |
| `\|\|`   | "OU" lógico (`false` se ambos `false`, senão `true`)                       |
| `^`      | "OU EXCLUSIVO" lógico (`true` se somente um valor é `true`, senão `false`) |

Tabela-verdade:

| `x`     | `y`     | `!x`    | `x && y` | `x \|\| y` | `x ^ y` |
| ------- | ------- | ------- | -------- | ---------- | ------- |
| `false` | `false` | `true`  | `false`  | `false`    | `false` |
| `false` | `true`  | `true`  | `false`  | `true`     | `true`  |
| `true`  | `false` | `false` | `false`  | `true`     | `true`  |
| `true`  | `true`  | `false` | `true`   | `true`     | `false` |

[Programa da vídeo-aula](https://youtu.be/70RI1a5wN78)

Fonte: https://www.slmandic.edu.br/tudo-sobre-coronavirus/

```cs
int idade;

bool febre, tosse, outroSintomaRespiratorio;
bool faltaAr, aumentoFrequenciaRespiratoria, dorToracica, sensacaoDesmaio;
bool hipertensao, diabetes, outrasDoencasCronicas;
bool doencaCoronariana, doencaCronica;

bool possuiSinalDeAlarme, possuiFatorDeRisco;

bool situacaoGrave;

Console.Clear();
Console.WriteLine("-- Triagem para Covid-19 --");
Console.WriteLine("\nAdaptado de https://www.slmandic.edu.br/tudo-sobre-coronavirus/");
Console.WriteLine("RESULTADO ESTRITAMENTE EDUCACIONAL. PROCURE AJUDA ESPECIALIZADA.\n");

Console.Write("Qual a idade do paciente? ");

bool idadeNumerica = Int32.TryParse(Console.ReadLine(), out idade);

if (!idadeNumerica || idade < 0 || idade > 130)
{
    Console.WriteLine("Idade inválida.");
    Environment.Exit(-1);
}

Console.WriteLine("\nResponda [S] para SIM, ou outro para NÃO.\n");

Console.Write("Paciente com febre? ");
febre = Console.ReadLine().ToUpper() == "S";

Console.Write("Paciente com tosse? ");
tosse = Console.ReadLine().ToUpper() == "S";

Console.Write("Paciente com outro sintoma respiratório? ");
outroSintomaRespiratorio = Console.ReadLine().ToUpper() == "S";

if (!febre && !tosse && !outroSintomaRespiratorio)
{
    Console.ForegroundColor = ConsoleColor.Green;
    Console.WriteLine("\n* Nenhuma recomendação específica. #fiqueemcasa");
}
else
{
    Console.WriteLine("\n-- Sinais de alarme --");

    Console.Write("Paciente com falta de ar? ");
    faltaAr = Console.ReadLine().ToUpper() == "S";

    Console.Write("Paciente com aumento de frequência respiratória? ");
    aumentoFrequenciaRespiratoria = Console.ReadLine().ToUpper() == "S";

    Console.Write("Paciente com dor torácica? ");
    dorToracica = Console.ReadLine().ToUpper() == "S";

    Console.Write("Paciente com sensação de desmaio? ");
    sensacaoDesmaio = Console.ReadLine().ToUpper() == "S";

    possuiSinalDeAlarme = faltaAr
        || aumentoFrequenciaRespiratoria
        || dorToracica
        || sensacaoDesmaio;

    if (idade < 18)
    {
        Console.WriteLine("\n-- Fatores de risco para menores --");

        Console.Write("Paciente com hipertensão arterial sistêmica? ");
        hipertensao = Console.ReadLine().ToUpper() == "S";

        Console.Write("Paciente com diabetes melito? ");
        diabetes = Console.ReadLine().ToUpper() == "S";

        Console.Write("Paciente com outras doenças crônicas? ");
        outrasDoencasCronicas = Console.ReadLine().ToUpper() == "S";

        possuiFatorDeRisco = hipertensao || diabetes || outrasDoencasCronicas;
    }
    else
    {
        Console.WriteLine("\n-- Fatores de risco para maiores --");

        Console.Write("Paciente com doença coronariana prévia? ");
        doencaCoronariana = Console.ReadLine().ToUpper() == "S";

        Console.Write("Paciente com doença crônica descompensada? ");
        doencaCronica = Console.ReadLine().ToUpper() == "S";

        possuiFatorDeRisco = (idade > 65)
            || aumentoFrequenciaRespiratoria
            || doencaCoronariana
            || doencaCronica;
    }

    if (possuiSinalDeAlarme || possuiFatorDeRisco)
    {
        Console.WriteLine("\n-- Situação --");

        Console.Write("Paciente com situação grave? ");
        situacaoGrave = Console.ReadLine().ToUpper() == "S";

        if (situacaoGrave)
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("\n* Encaminhar ambulância para o local.");
        }
        else
        {
            Console.ForegroundColor = ConsoleColor.Magenta;
            Console.WriteLine("\n* Encaminhar para o sistema de saúde.");
        }
    }
    else
    {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine("\n* Recomendar isolamento domiciliar.");
    }

}

Console.ResetColor();
Console.Write("\nObrigado!");
```
