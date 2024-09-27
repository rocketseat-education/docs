# Expressão Regular

## RegEx

Em ciência da computação, uma expressão regular provê uma forma concisa e flexível de identificar cadeias de caracteres de interesse.
O termo deriva do trabalho do matemático norte-americano Stephen Cole Kleene, que desenvolveu as expressões regulares como uma notação ao que ele chamava de álgebra de conjuntos regulares.

Em resumo, regex é a abreviação do inglês Regular Expressions, que possibilita você buscar, validar e alterar qualquer padrão de caracteres em qualquer texto.

Três pontos fortes em expressões regulares são:

* **Localização:** Você consegue encontrar determinados padrões dentro de qualquer sequência de caracteres, por exemplo a busca por e-mails dentro de um texto.
* **Validação:** Você pode fazer a validação dos mais variados padrões, como telefone, e-mail e senha.
* **Substituição:** Você pode substituir qualquer trecho palavra ou letra dentro de um texto.

# Localização

Imagina sua empresa ter sido convocada para desenvolver um algorítimo onde *localize* os candidatos que mencionarem experiência na área da programação conforme descrição abaixo de alguns candidatos:

|Nome| Experiência Profissional                                                                      |
|-----|-----------------------------------------------------------------------------------------------|
|Marcos| Dois anos como `Programador` Java no SoftSystem                                               |
|Leticia| Vasta experiência em `programação` e desenvolvimento de softwares                             |
|Marcela| UX e Design na Google                                                                         |
|Jonas| Estágio em análise e desenvolvimento de sistemas utilizando a linguagem de `programacao` Java |

> [!NOTE]
> Considerando os quatro candidadtos acima, três deles possuem experiência compatível os critérios de seleção.

<details>
  <summary>Cenario</summary>

  ```java
 public class Recrutamento {
    public static void main(String[] args) {
        String [] candidatos = {
                "Marcos - Dois anos como Programador Java no SoftSystem",
                "Letícia - Vasta experiência em programação e desenvolvimento de softwares",
                "Marcela - UX e Design na Google",
                "Jonas - Estágio em análise e desenvolvimento de sistemas utilizando a linguagem de programacao Java"
        };
        for(String candidato:candidatos){
            boolean selecionado = candidato.contains("programação");
            if(selecionado)
                System.out.println(candidato + " - FOI SELECIONADO(A)");
        }
    }
    
    //de acordo com a lógica de análise de candidatura
    //somente a Letícia foi SELECIONADA
}
  ```

</details>

<details>
  <summary>Solução</summary>

  ```java
// refatoração na linha 10

boolean selecionado = candidato.contains("programação") 
         || candidato.contains("programacao") 
         || candidato.contains("Programador");
  ```

</details>

> [!WARNING]
> Conseguimos localizar os três candidatos que atuam com programação mas será se o nosso algorítimo é assertivo?
>Novos candidatos poderão descrever sua experiência como `programador`, `PROGRAMAÇÃO` e por ai vai.

<details>
  <summary>Grupo de caracteres</summary>

  ```java
/**
    Em regex temos uma estratégia de busca chamada Character Group
    onde é possível agrupar ( a|b ) alternativas de combinações.
    Veja que agora usaremos o método "matches" da classe String.
 */

for(String candidato:candidatos){
    boolean selecionado = candidato.matches(".*(p|P)(rograma|ROGRAMA)(ção|cao|dor).*");
    if(selecionado)
    System.out.println(candidato + " - FOI SELECIONADO(A)");
}
  ```

</details>

> [!NOTE]
> Na linha 8 de código acima vemos uma validação que aos *olhos humanos* não é algo tão simples de compreender, por isso é relevante conhecer, classificar e interpretar cada cadeia de símbolos e caracteres citados no exemplo acima.

## Validação

A validação em RegEx ajuda a definir um conjunto de opções de validação para um determinado campo, Imagina a vulnerabilidade de sua aplicação diante do cenário abaixo:

1. Uma hora no formato HH:mm
1. Um CPF com mais 11 dígitos ou que NÃO contenha alfa-numericos
1. A placa de um veículo no padrão AAA-0000
1. Aceitar que algumas palavras com ou sem acentuação

<details>
  <summary>Case Hora</summary>

  ```java
String regex = "[0-9][0-9]:[0-5][0-9]";
System.out.println("16:30".matches(regex));
System.out.println("23:59".matches(regex));
System.out.println("23:60".matches(regex));//não existe hora com segundo 60
System.out.println("99:58".matches(regex));//hora máxima, reveja sobre quantificadores
  ```

</details>

<details>
  <summary>Case CPF</summary>

  ```java
String regex = "[0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9]";
System.out.println("00698722654".matches(regex));

// Símbolos de representação + quantificadores
regex = "\\d{11}";
System.out.println("00698722654".matches(regex));
System.out.println("06987226-54".matches(regex));//possui comprimento igual porém contém caracteres
System.out.println("0698722654".matches(regex));//comprimento diferente de 11
  ```

</details>

<details>
  <summary>Case Placa</summary>

  ```java
String regex = "[A-Z][A-Z][A-Z]-[0-9][0-9][0-9][0-9]";
System.out.println("HIJ-7768".matches(regex));

//quantificadores
regex = "[A-Z]{3}-[0-9]{4}";
System.out.println("HIJ-7768".matches(regex));
  ```

</details>

<details>
  <summary>Case Palavras</summary>

  ```java
//operador lógico
String regex = "S(A|Ã)O PAULO";
System.out.println("SAO PAULO".matches(regex));
System.out.println("SÃO PAULO".matches(regex));
  ```

</details>

## Substituição

Já imaginou receber uma cadeia de caracteres onde os mesmos precisam ser substituídos considerando algum padrão ou regra de negócio, exemplo:

1. Remover espaços em branco ou caracteres NÃO númericos
2. Substituir caracteres literalmente
2. Formatar blocos de dígitos conforme padrões tipo: cpf, cnpj, cep, data e etc.

<details>
  <summary>Remover espaços</summary>

  ```java
String literal ="$ 1000";
String numero = literal.replaceAll("\\D","");

System.out.println(numero);//1000
  ```

</details>

<details>
  <summary>Mudar caracteres</summary>

  ```java
String literal ="Bolo";
String numero = literal.replaceAll("o","a");

System.out.println(numero);//Bala
  ```

</details>

<details>
  <summary>Formatar blocos</summary>

  ```java
String cpf="88709822336";

String cpfFormatado = cpf.replaceAll("(\\d{3})(\\d{3})(\\d{3})(\\d{2})", "$1.$2.$3-$4");

System.out.println(cpfFormatado); //887.098.223-36
  ```

</details>

> [!WARNING]
> Observe que foi usado o método replaceAll para realizar as subsituições.

## Pattern e Matcher

A classe `String` incorporou muitos dos recursos destinados a tratamento de caracteres na linguagem onde muitas das vezes não precisamos utilizar explicitamente as classes `java.util.regex.Pattern` e `java.util.regex.Matcher` para realizar operações que precisam aplicar expressões regulares.
Para explorar estas duas classes, iremos ilustrar a formatação do cpf apresentadando no exemplo anterior:

  ```java
 String cpf="88709822336";
//criando o padrão com base na regex
Pattern pattern = Pattern.compile("(\\d{3})(\\d{3})(\\d{3})(\\d{2})");

//conjunto de recursos relacionado ao padrão apresentado
Matcher matcher = pattern.matcher(cpf);

//a expressão dividiu em blocos os digitos do cpf
//b(3),b(3),b(3),b(2)
while(matcher.find()) {
    System.out.println(matcher.group(1));
    System.out.println(matcher.group(2));
    System.out.println(matcher.group(3));
    System.out.println(matcher.group(4));
    
    //junção dos grupos (blocos)
    System.out.println(matcher.group());
}

//o cpf combina com o padrão
System.out.println("cpf está conforme a especificação? " + matcher.matches());

  ```

## Classificação

Estruturar uma expressão regular exige uma compreensão relevante diante dos símbolos e caracteres que a formará, abaixo segue uma classificação destes símbolos e caracteres:

<details>
  <summary>Caracteres</summary>

  ```html
Símbolo | Descrição
\.      | Qualquer caractere, símbolo ou dígito
\d      | Um dígito de 0 a 9
\D      | Não dígito
\w      | Caractere da palavra: letra ASCII, dígito ou sublinhado
\W      | Um caractere que não é um caractere de palavra conforme definido pelo \w do seu mecanismo
\s      | Caractere de espaço em branco: espaço, tabulação, nova linha, retorno de carro, tabulação vertical
\S      | Um caractere que não seja um caractere de espaço em branco, conforme definido pelo \s do seu mecanismo
  ```

</details>

<details>
  <summary>Quantificadores</summary>

  ```html
Símbolo | Descrição
\*      | Nenhuma ou várias ocorrências
\?      | Nenhuma ou uma única ocorrência
\+      | Uma ou várias ocorrências
\{1}    | Exatamente uma ocorrência
\{2,4}  | Mínimo duas e máxima quatro ocorrências
\{3,}   | Três ou mais ocorrências
  ```

</details>

<details>
  <summary>Lógicos</summary>

  ```html
Símbolo | Descrição
\(  |  )| Operador OU (or)
\(a | b)| Grupo de captura de combinação
  ```

</details>

<details>
  <summary>Âncoras</summary>

  ```html
Símbolo | Descrição
\^      | Inicia com
\$      | Termina com
  ```

</details>

<details>
  <summary>Limitadores</summary>

  ```html
Símbolo | Descrição
\[abc]  | Uma das alternativas apresentadas
\[0-9]  | Dígitos numéricos
\[x-y]  | Um dos caracteres no intervalo conforme tabela ASCII
\[^x]   | Qualquer caractere que NÃO seja
\[^x-y] | Qualquer caractere que NÃO esteja entre
  ```

</details>
