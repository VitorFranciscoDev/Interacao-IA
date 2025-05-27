Interação IA - Orientação a Objetos 2

Pergunta: Qual a diferença entre herança e composição?

Herança e composição são dois princípios fundamentais da **programação orientada a objetos (POO)**. Ambos são usados para reutilização de código, mas funcionam de formas diferentes e têm propósitos distintos.

---

### ✅ **Herança**

Herança é um relacionamento **"é um"** (is-a). Uma classe (subclasse) herda atributos e métodos de outra classe (superclasse).

#### Exemplo:

```python
class Animal:
    def fazer_som(self):
        print("Som genérico")

class Cachorro(Animal):
    def fazer_som(self):
        print("Latido")
```

* `Cachorro` **é um** `Animal`.
* `Cachorro` herda o método `fazer_som` e pode sobrescrevê-lo.

#### Vantagens:

* Reutilização de código entre classes semelhantes.
* Facilita a criação de hierarquias.

#### Desvantagens:

* Pode gerar um acoplamento forte.
* Difícil de manter quando a hierarquia fica complexa.
* Herança múltipla pode causar conflitos (em algumas linguagens).

---

### ✅ **Composição**

Composição é um relacionamento **"tem um"** (has-a). Uma classe contém instâncias de outras classes como atributos.

#### Exemplo:

```python
class Motor:
    def ligar(self):
        print("Motor ligado")

class Carro:
    def __init__(self):
        self.motor = Motor()

    def ligar(self):
        self.motor.ligar()
```

* `Carro` **tem um** `Motor`.
* Em vez de herdar, `Carro` **usa** `Motor`.

#### Vantagens:

* Mais flexível que herança.
* Permite reusar código de forma modular.
* Menor acoplamento entre as classes.
* Facilita testes e manutenção.

#### Desvantagens:

* Pode exigir mais código (delegação explícita).
* Um pouco mais complexo em casos simples.

---

### 🧠 Em resumo:

| Aspecto       | Herança                       | Composição       |
| ------------- | ----------------------------- | ---------------- |
| Relação       | "É um" (is-a)                 | "Tem um" (has-a) |
| Reutilização  | Via herança direta            | Via delegação    |
| Acoplamento   | Mais forte                    | Mais fraco       |
| Flexibilidade | Menor                         | Maior            |
| Manutenção    | Mais difícil em grande escala | Mais fácil       |

---

### 🔁 Quando usar:

* **Herança**: quando existe uma clara **relação hierárquica** entre as classes.
* **Composição**: quando você quer **modularidade, flexibilidade e reuso de comportamento**, sem criar dependências rígidas.

Se quiser, posso te mostrar um exemplo mais completo comparando os dois casos.

//Observações: O conceito de herança é um conceito muito abordado na POO, porém quase nunca vejo algo especificamente sobre composição, logo fiquei com certa dúvida e decidi perguntar sobre a IA, e consegui entender bem essa diferença entre is-a e has-a.

Pergunta: O que são métodos estáticos e em quais cenários são utilizados?

Em Dart, **métodos estáticos** são métodos que pertencem **à classe** e **não a uma instância da classe**. Isso significa que você pode chamá-los **sem criar um objeto da classe**. Eles são declarados usando a palavra-chave `static`.

---

### ✅ **Declaração e uso de métodos estáticos em Dart:**

```dart
class Matematica {
  static int somar(int a, int b) {
    return a + b;
  }
}

void main() {
  int resultado = Matematica.somar(2, 3);
  print(resultado); // Saída: 5
}
```

* O método `somar` é estático, então é chamado diretamente pela classe (`Matematica.somar`), sem precisar de um objeto.

---

### 📌 **Características dos métodos estáticos:**

* **Não podem acessar membros de instância (não estáticos)** da classe diretamente.
* **Podem acessar outros membros estáticos** da mesma classe.
* São úteis para **funções auxiliares** ou **operações utilitárias** que **não dependem do estado de uma instância**.

---

### 🎯 **Quando usar métodos estáticos em Dart?**

Use métodos estáticos quando:

1. **A lógica do método não depende do estado do objeto.**

   * Exemplo: funções matemáticas, validações, conversões de tipos, etc.

2. **Você quer agrupar funções utilitárias relacionadas a uma classe.**

   * Exemplo: `DateTime.parse()`, `int.parse()`, `jsonEncode()`.

3. **Está implementando padrões como Singleton.**

   * Um método estático pode ser usado para retornar a única instância de uma classe.

---

### 🚫 **Quando evitar métodos estáticos?**

* Quando a função precisa acessar ou modificar dados da instância (variáveis não-estáticas).
* Quando você precisa de **polimorfismo** (sobrescrita em subclasses), pois métodos estáticos **não são polimórficos**.

---

Se quiser, posso mostrar exemplos mais avançados ou comparações com métodos de instância. Deseja?

//Observações: Não havia entendido muito bem como funcionavam os métodos estáticos, mas com essa resposta, consegui entender que eles são utilizados para chamar diretamente pela classe, e não pelo objeto.

Pergunta: Como posso criar um setter para modificar um atributo privado de uma classe com validação?

Ótima pergunta! Em Dart, para criar um **setter** que modifica um atributo **privado** de uma classe com **validação**, você pode usar a sintaxe especial de setters, que permite executar lógica antes de modificar o valor.

---

### Passo a passo:

1. Declare o atributo privado com `_` (underscore) antes do nome.
2. Crie um **setter** usando a palavra-chave `set` seguida do nome da propriedade pública.
3. Dentro do setter, implemente a **validação**.
4. Caso a validação passe, atualize o atributo privado.

---

### Exemplo prático:

```dart
class Pessoa {
  String _nome = '';

  // Setter com validação
  set nome(String novoNome) {
    if (novoNome.isEmpty) {
      print('Erro: o nome não pode ser vazio.');
    } else {
      _nome = novoNome;
    }
  }

  // Getter para acessar o nome (opcional)
  String get nome => _nome;
}

void main() {
  Pessoa p = Pessoa();

  p.nome = '';          // Tenta definir nome vazio -> erro
  print(p.nome);        // Saída: '' (não mudou)

  p.nome = 'Maria';     // Define nome válido
  print(p.nome);        // Saída: Maria
}
```

---

### Detalhes importantes:

* O setter é chamado como se fosse uma atribuição normal (`p.nome = 'Maria';`).
* A validação pode ser qualquer lógica que quiser (exemplo: tamanho mínimo, formato, valores permitidos, etc).
* O atributo privado (`_nome`) **não pode ser acessado diretamente fora da classe**, por isso o setter é o meio para modificá-lo com controle.

Quer que eu crie um exemplo com outro tipo de validação? Ou com outros tipos de atributos?

//Observações: Não sabia dessa possibilidade de fazer a validação diretamente no Set, achei bem interessante essa funcionalidade e creio que terá uma grande importância em vários algoritmos.