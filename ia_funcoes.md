
# Interação IA - Funções

## Pergunta: Posso chamar minha função dentro dela mesma em Dart?

Sim, você pode!  
Isso se chama **recursão**: é quando uma função chama a si mesma para resolver partes menores de um problema.

Aqui está um exemplo simples em Dart:

```dart
int fatorial(int n) {
  if (n <= 1) {
    return 1;
  } else {
    return n * fatorial(n - 1);
  }
}

void main() {
  print(fatorial(5)); // Saída: 120
}
```

Nesse exemplo, a função `fatorial` chama ela mesma até `n` ser 1.  
**Só cuidado**: sempre tem que ter uma condição de parada (`if (n <= 1)` aqui), senão seu código entra em loop infinito e estoura a pilha (erro de stack overflow).

> **Observações:** Já tinha ouvido falar sobre recursão por conta da sequência de Fibonacci, porém não entendia muito bem como funcionava ou quando utilizar isso, mas acho que a resposta da IA conseguiu esclarecer minhas dúvidas.

---

## Pergunta: Existe limite de parâmetros a serem recebidos em uma função?

Boa pergunta!  
**Tecnicamente, não existe um limite fixo definido pela linguagem Dart** para o número de parâmetros que uma função pode receber.

**Porém**, na prática:
- O tamanho final do *stack frame* tem um limite físico — depende do dispositivo, da plataforma e do runtime do Dart.
- Se você exagerar (tipo, passar centenas ou milhares de parâmetros), pode:
  - Deixar o código difícil de entender;
  - Atrapalhar otimizações do compilador;
  - Causar problemas de performance ou estouro de memória.

Por isso, o ideal é:
- Agrupar parâmetros relacionados em uma classe ou struct;
- Usar objetos como parâmetros.

Exemplo ruim:

```dart
void criarPessoa(String nome, int idade, String endereco, String telefone, String email, String cpf, String rg, String cidade, String estado) {
  // 😵
}
```

Exemplo bom:

```dart
class Pessoa {
  String nome;
  int idade;
  String endereco;
  String telefone;
  String email;
  String cpf;
  String rg;
  String cidade;
  String estado;

  Pessoa({
    required this.nome,
    required this.idade,
    required this.endereco,
    required this.telefone,
    required this.email,
    required this.cpf,
    required this.rg,
    required this.cidade,
    required this.estado,
  });
}

void criarPessoa(Pessoa pessoa) {
  // 😎
}
```

> **Observações:** Vi esse prompt e fiquei com certa dúvida em relação a esse limite, mas entendi que realmente não tem um limite, porém não é recomendado utilizar muitos por conta da organização e da memória.

---

## Pergunta: Quais métodos existem para variáveis do tipo Function em Dart?

Ótima pergunta!

No Dart, **`Function`** é um tipo de objeto especial — funções são objetos — mas ele **não tem métodos próprios** além dos herdados de `Object`.

Ou seja:
- Você pode **chamar** a função normalmente usando `()`.
- Métodos herdados de `Object`:
  - `toString()`
  - `hashCode`
  - `runtimeType`
  - `==` (comparação)

Exemplo:

```dart
void digaOla() {
  print('Olá!');
}

void main() {
  Function funcao = digaOla;
  funcao(); // chama a função
}
```

**Detalhe:**  
Existe uma classe interna chamada `Callable` (`call` method) para permitir que uma classe se comporte como função:

```dart
class Somador {
  int call(int a, int b) => a + b;
}

void main() {
  var somar = Somador();
  print(somar(3, 4)); // 7
}
```

> **Observações:** Fiz essa pergunta pois achei que haveriam mais métodos para as funções que pudessem ajudar na construção do algoritmo, porém entendi que possuem apenas os métodos herdados de objeto.

---

## Pergunta: Explique os métodos `runtimeType` e `hashCode` utilizando exemplos práticos.

### `runtimeType`

O método `runtimeType` retorna o **tipo real** do objeto em tempo de execução.

Exemplo:

```dart
void minhaFuncao() {
  print('Oi!');
}

void main() {
  Function funcao = minhaFuncao;
  print(funcao.runtimeType); // Output: () => void
}
```

Outro exemplo:

```dart
int soma(int a, int b) {
  return a + b;
}

void main() {
  var funcao = soma;
  print(funcao.runtimeType); // Output: (int, int) => int
}
```

---

### `hashCode`

O `hashCode` gera um **número inteiro único** representando o objeto.

Exemplo:

```dart
void minhaFuncao() {
  print('Oi!');
}

void outraFuncao() {
  print('Tchau!');
}

void main() {
  Function funcao1 = minhaFuncao;
  Function funcao2 = outraFuncao;
  Function funcao3 = minhaFuncao;

  print(funcao1.hashCode);
  print(funcao2.hashCode);
  print(funcao3.hashCode); // Igual ao de funcao1!
  
  print(funcao1 == funcao3); // true
}
```

---

### Resumo:

| Método        | Para que serve?                         | Exemplo                                           |
|:--------------|:-----------------------------------------|:--------------------------------------------------|
| `runtimeType` | Saber qual é o tipo real do objeto        | Descobrir se a variável é uma função, int, String |
| `hashCode`    | Gerar um número único representando o objeto | Comparar funções ou usar em mapas e sets          |

> **Observações:** Esses métodos citados são dois dos quatro métodos citados pela IA na última pergunta, e eu ainda não havia parado para pesquisar e entender, mas consegui entender bem com essa explicação.

---

## Pergunta: Traga exemplos de mau uso de funções.

### 1. Funções gigantes (fazem mil coisas)

```dart
void processarPedido(String cliente, List<String> itens, String endereco, double valorTotal) {
  print('Validando cliente...');
  print('Calculando frete...');
  print('Salvando no banco de dados...');
  print('Enviando email...');
  print('Pedido finalizado!');
}
```
> Melhor quebrar em funções menores!

---

### 2. Funções sem reuso (código duplicado)

```dart
void mostrarMensagemErro() {
  print('Erro: Algo deu errado.');
}

void mostrarMensagemLoginInvalido() {
  print('Erro: Algo deu errado.');
}
```
> Melhor centralizar em uma função única.

---

### 3. Funções com parâmetros demais

```dart
void criarProduto(String nome, double preco, int estoque, String descricao, String categoria, bool ativo, String fornecedor) {
  // ...
}
```
> Prefira usar objetos para agrupar informações.

---

### 4. Nome da função ruim

```dart
void doSomething() {
  // ???
}
```
> Melhor nomear de forma clara, tipo `enviarEmailDeConfirmacao()`.

---

### 5. Funções que mudam estado sem avisar

```dart
int total = 0;

void adicionarAoTotal(int valor) {
  total += valor;
}
```
> Prefira funções puras, que retornam valores.

---

### 6. Funções recursivas sem condição de parada

```dart
void contarAteInfinito(int numero) {
  print(numero);
  contarAteInfinito(numero + 1); // Nunca para!
}
```
> Sempre defina uma condição de parada.

---

> **Observações:** Fiz essa pergunta mais para ter uma noção de se eu estava fazendo algo errado ao declarar funções, e acho que consegui corrigir alguns erros com esses exemplos da IA.
