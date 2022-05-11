# Princípios fundamentais da POO

- Encapsulamento
- Abstração
- Herança
- Polimorfismo

## Encapsulamento

O encapsulamento é obtido quando cada objeto mantém seu estado **privado**, dentro de uma classe. Outros objetos não têm acesso direto a esse estado. Em vez disso, eles podem apenas chamar uma lista de funções públicas — chamadas de métodos.

Assim, o objeto gerencia seu estado por meio dos métodos — e nenhuma outra classe pode tocar, a menos que seja explicitamente permitido. Se quiser se comunicar com o objeto, você deve usar os métodos fornecidos. No entanto, por padrão, você não pode mudar o estado.

## Abstração

Extensão do _encapsulamento_. Cada objeto deve **expor somente um** mecanismo de alto nível. Esse mecanismo deve ocultar detalhes da implementação interna e revelar apenas as operações relevantes para outros objetos.

Exemplo: Você interage com seu telefone usando apenas alguns botões. O que está acontecendo por debaixo dos panos? Você não precisa saber — os detalhes da implementação estão ocultos. Você precisa apenas conhecer um conjunto pequeno de ações.

## Herança

É uma forma de reutilizar a lógica comum à mais de uma classe. Criar uma _classe filha_ derivada de outra _classe pai_. Dessa forma, a _classe filha_ reutiliza todos os campos e métodos da _classe pai_ (parte comum) e ainda pode implementar seus próprios campos e códigos (parte exclusiva).

## Polimorfismo

Traz um modo de usar uma _classe filha_ da mesma forma que usamos uma _classe pai_, aonde cada _filha_ pode implementar sua própria versão de um método herdado. 

Na prática, é a reescrita de um método herdado de uma classe, sem utilizar **nenhum** comportamento da _classe pai_ para este método.

