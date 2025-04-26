
# Interação IA - Controle de fluxo

## Pergunta: Em quais situações é útil usar o comando `continue` em um loop em Dart?

Boa pergunta!  
Em Dart (e em muitas outras linguagens), o comando `continue` é usado dentro de loops (`for`, `while`, `do-while`) para **pular o restante do corpo do loop** e **ir direto para a próxima iteração**.

Ele é útil principalmente em situações como:

- **Ignorar casos específicos** sem interromper todo o loop (diferente do `break` que *sai* do loop).
- **Evitar código aninhado** (ao invés de colocar o resto do loop dentro de um `if`, você pula logo o que não interessa).
- **Melhorar a legibilidade** deixando claro que, em certas condições, você não quer executar mais nada daquela iteração.

### Exemplos práticos:

1. **Pular números pares em um `for`**:
   ```dart
   for (int i = 0; i < 10; i++) {
     if (i % 2 == 0) continue;
     print(i); // Só imprime números ímpares
   }
   ```

2. **Validar entradas e ignorar inválidas**:
   ```dart
   List<String> inputs = ['123', '', '456', 'abc', '789'];

   for (var input in inputs) {
     if (input.isEmpty || int.tryParse(input) == null) {
       continue; // Ignora entradas vazias ou não numéricas
     }
     print('Número válido: $input');
   }
   ```

3. **Processar lista mas pular itens específicos**:
   ```dart
   List<String> nomes = ['Ana', 'Lucas', 'admin', 'Bruna'];

   for (var nome in nomes) {
     if (nome == 'admin') continue;
     print('Bem-vindo, $nome');
   }
   ```

---

**Resumindo**: `continue` é ótimo quando **você quer seguir o fluxo do loop mas sem fazer nada para certos casos**.  
Quer que eu também te mostre como o `continue` pode ser usado com *labels* em Dart? Fica ainda mais interessante! 🚀

> **Observação:** Eu havia entendido como funcionava o `continue`, mas com a ajuda dos exemplos práticos consegui visualizar melhor quando utilizá-lo nos algoritmos.

---

## Pergunta: Quando utilizar o `forEach`? Demonstre utilizando exemplos práticos.

Legal, vamos nessa!

O `forEach` em Dart é um método que pode ser chamado em coleções como `List`, `Set`, ou `Map` para **executar uma função para cada elemento**.

### Em que situações usar `forEach`?

- Quando você quer **executar uma ação simples** para cada item da coleção.
- Quando **não precisa manipular o índice** diretamente.
- Quando **não precisa controlar o loop** (tipo usar `break` ou `continue` — no `forEach` isso não é possível de forma direta).
- Para **códigos mais funcionais/limpos**, especialmente com funções anônimas (lambdas).

...

