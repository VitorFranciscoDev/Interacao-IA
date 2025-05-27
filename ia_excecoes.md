
# Interação IA - Tratamento de erros e exceções

## ❓ Pergunta: Qual a diferença entre um erro e uma exceção em Dart?

Em Dart, **erro (error)** e **exceção (exception)** são ambos tipos de problemas que podem ocorrer durante a execução de um programa, mas eles têm propósitos e significados diferentes. Aqui está a diferença principal entre eles:

---

### ✅ **Exceção (`Exception`)**

- Representa **condições esperadas**, mas **anormais**, durante a execução.
- **Podem e devem ser tratadas** com `try-catch`.
- São usadas para **controlar o fluxo do programa** em situações como:
  - Arquivo não encontrado.
  - Divisão por zero.
  - Falha de rede.
  - Formato inválido de entrada.

#### Exemplo:
```dart
try {
  int resultado = int.parse("abc"); // Isso lança uma FormatException
} on FormatException catch (e) {
  print("Erro de formato: $e");
}
```

---

### ❌ **Erro (`Error`)**

- Representa **problemas graves** que normalmente são causados por **falhas no código do programa**.
- **Não devem ser tratados normalmente**, pois indicam bugs ou situações que o programa não deve tentar corrigir.
- Ex: `NoSuchMethodError`, `OutOfMemoryError`, `StackOverflowError`.

#### Exemplo:
```dart
void main() {
  dynamic x = null;
  print(x.length); // NoSuchMethodError
}
```

---

### 🔍 Resumo:

| Aspecto            | Exceção (`Exception`)        | Erro (`Error`)                            |
| ------------------ | ---------------------------- | ----------------------------------------- |
| Significado        | Problemas esperados/anormais | Problemas graves/internos                 |
| Tratável com `try` | Sim                          | Tecnicamente sim, mas **não recomendado** |
| Uso comum          | Falhas de I/O, formato, rede | Acesso nulo, métodos inexistentes         |
| Causa provável     | Condições externas           | Bugs de programação                       |

---

### 📌 Dica prática:
Use `Exception` para situações que seu programa pode recuperar ou lidar adequadamente. `Error` deve ser deixado para situações que indicam que algo está fundamentalmente errado no seu código.

---

**Observação:** Já tinha uma noção da diferença entre os dois, porém quis perguntar para não haver pontas soltas relacionadas a esse assunto, e creio que após essa explicação, consegui compreender bem o assunto.

---

## ❓ Pergunta: Por que eu precisaria lançar uma exceção em meu código?

Você pode precisar **lançar uma exceção (`throw`)** no seu código em Dart para **sinalizar que ocorreu uma situação anormal que impede a execução normal de um trecho de código**.

---

## ✅ Motivos para lançar uma exceção personalizada

### 1. Validar dados de entrada
```dart
void dividir(int a, int b) {
  if (b == 0) {
    throw ArgumentError('O divisor não pode ser zero');
  }
  print(a / b);
}
```

### 2. Rejeitar um estado inválido
```dart
class ContaBancaria {
  double saldo;
  ContaBancaria(this.saldo);

  void sacar(double valor) {
    if (valor > saldo) {
      throw Exception('Saldo insuficiente');
    }
    saldo -= valor;
  }
}
```

### 3. Forçar tratamento por parte do chamador
```dart
String buscarUsuario(String id) {
  if (id.isEmpty) {
    throw Exception('ID do usuário não pode ser vazio');
  }
  // lógica para buscar usuário
}
```

### 4. Criar exceções específicas e reutilizáveis
```dart
class LoginInvalidoException implements Exception {
  final String mensagem;
  LoginInvalidoException(this.mensagem);

  @override
  String toString() => 'LoginInvalidoException: $mensagem';
}
```

```dart
void autenticar(String usuario, String senha) {
  if (usuario != 'admin' || senha != '1234') {
    throw LoginInvalidoException('Usuário ou senha incorretos');
  }
}
```

---

## 🎯 Quando lançar uma exceção faz sentido?

- Quando algo **impede a continuação da execução normal**.
- Quando você quer **informar quem chamou** que há um problema.
- Quando você quer **evitar resultados incorretos ou silenciosamente errados**.
- Quando você quer tornar seu código mais **claro, confiável e seguro**.

---

## ⚠️ Mas cuidado:

- **Evite abusar** de exceções para controle de fluxo normal (por exemplo, usar exceções como substituto de `if/else`).
- Exceções têm **custo de desempenho**; use para erros e não como lógica regular.

---

**Observação:** Fiquei realmente com dúvida na necessidade de lançar exceções personalizadas, porém com a ajuda da IA, consegui entender essa necessidade e a utilização dela nos algoritmos.

---

## ❓ Pergunta: Quais são as melhores práticas para lançar exceções em Dart?

### ✅ Melhores práticas ao lançar exceções em Dart

#### 1. Use exceções para erros esperados, não para lógica normal
```dart
if (senha.isEmpty) {
  throw ArgumentError('Senha não pode ser vazia');
}
```

#### 2. Escolha a exceção adequada (ou crie a sua)
```dart
class ProdutoInvalidoException implements Exception {
  final String mensagem;
  ProdutoInvalidoException(this.mensagem);

  @override
  String toString() => 'ProdutoInvalidoException: $mensagem';
}
```

#### 3. Forneça mensagens descritivas
```dart
throw ArgumentError('O parâmetro "nome" não pode ser nulo ou vazio.');
```

#### 4. Prefira exceções com tipos específicos a strings genéricas
```dart
// ❌ Evite:
throw 'Erro de login';
// ✅ Melhor:
throw LoginInvalidoException('Usuário ou senha inválidos');
```

#### 5. Evite lançar `Error`, a menos que seja necessário

#### 6. Documente exceções que sua função pode lançar
```dart
/// Lança [FormatException] se a entrada estiver em formato inválido.
int converterId(String entrada) {
  return int.parse(entrada);
}
```

#### 7. Combine `try-catch` com `rethrow` se quiser tratar parcialmente
```dart
try {
  autenticarUsuario();
} catch (e) {
  print('Erro durante autenticação: $e');
  rethrow;
}
```

#### 8. Não engula exceções sem necessidade
```dart
// ❌ Péssima prática:
try {
  fazerAlgoImportante();
} catch (e) {
  // Nada aqui — falha silenciosa
}
```

---

## 📌 Resumo prático

| Prática                                      | Explicação                                               |
| -------------------------------------------- | -------------------------------------------------------- |
| ✅ Use exceções para falhas, não lógica comum | Só use `throw` quando for uma situação realmente anormal |
| ✅ Use tipos específicos                      | `ArgumentError`, `FormatException`, ou exceções próprias |
| ✅ Escreva mensagens claras                   | Ajuda na depuração e manutenção                          |
| ✅ Documente as exceções                      | Deixe claro o que sua função pode lançar                 |
| ❌ Não use `throw` com `String`               | Use classes que implementem `Exception`                  |

---

**Observação:** Acho bem importante as boas práticas na programação, e com isso, resolvi pedir à IA alguns exemplos que pudessem me ajudar a não cometer tantos erros bobos quando estivesse programando.
