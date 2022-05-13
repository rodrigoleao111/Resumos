# Resumo Python

[TOC]



## Estruturas condicionais

### If-else

```python
frutas = ["banana", "maça", "uva"]
if "banana" in frutas:
	print("Tem banana")
elif "maça" in frutas:
	print("Tem maçã")  # Se tiver banana, não vai imprimir
else:
	print("Não tem maçã, nem banana")

# Resultado = Tem banana
```

### If inline

```python
a = 100
b = True
print a if b else 0
```



## Estruturas de repetiçao

### While

```python
lista = []

while True:
	print("Digite um número, se quiser sair digite -1")
	n = int(input())
	if n == -1:
		break
	else:
		lista.append(n)
```

### For

```python
for numero in lista:
	print(numero)

i = 0
for i in range(5):
	print(i+1)
```

Para mais informações sobre estruturas báscas, [clique aqui](https://grand-citron-0fc.notion.site/Conceitos-B-sicos-6e8639100e6b4102af680ede40f4f41c).



## Coleções

### Lista

```python
lista = [0, 1, 2, 3, 4]

# Outras formas de inicialização:
# lista = list()
# lista = []
# lista = list(['a', 'b', 'c', 'd'))

# Visualização:              
print(lista[0])  # 0
print(lista[-1]) # 4
print(lista[2:]) # [2, 3, 4]
print(lista[1:4]) # [1, 2, 3, 4]
print(lista[0:-1]) # [0, 1, 2, 3]
```

Para mais informações sobre listas e suas funções, [clique aqui](https://grand-citron-0fc.notion.site/Listas-937600778f35412b8e50faeabe12b8f5).

### Dicionário

```python
# nome = {'chave':valor}
dicionario = {'chaveInt': 1, 'chaveList':["item1", "item2"], 'chaveChar':'a'}
```

Para mais informações sobre dicionários e seus métodos, [clique aqui](https://grand-citron-0fc.notion.site/Dicion-rios-d1bf578adec94dbd8ed9a08be252d6f0).

### Tupla

```python
//valores imutáveis
tupla = (10, 20, 30)
print(tupla[1]) # 20
```

Para mais informações sobre tuplas e seus métodos, [clique aqui](https://grand-citron-0fc.notion.site/Tuplas-d223442137cf4703937befbb720fbe88).



## Métodos (funções)

```python
def media(num1, num2):
	media = (num1 + num2)/2
	return media

n1 = float(input("Insira a primeira nota: "))
n2 = float(input("Insira a segunda nota: "))
print(f"A média dos números é {media}")
```



## Programação Orientada a Objeto

### Classes

```python
class Pessoa:		# inicialização de classe
    def __init__(self, nome, sexo, cpf, ativo):		# construtor (método especial)
        self.__nome = nome		# dois underlines significam que a variável é privada
        self.__sexo = sexo
        self.__cpf = cpf
        self.__ativo = ativo
        
    def desativar(self):	# por padrão, métodos e atributos são públicos
        self.__ativo = False
        print("A pessoa foi desativada com sucesso")
  
	# getters e setters para variáveis privadas
	def get_nome(self):
        return self.__nome
    
    def set_nome(self, nome):
        self.__nome = nome

    # outra forma de acesso
    @property
    def nome(self):
        return self.__nome
    
    @nome.setter
    def nome(self, nome):
        self.__nome = nome

if __name__ == "__main__":		# controle de escopo de execução
    pessoa1 = Pessoa("João", "M", "123456", True)	# instanciar um objeto da classe Pessoa
    pessoa1.desativar()		# chamando um método da classe
    pessoa1.ativo = True
    print(pessoa1.ativo)
    
	# Utilizando geters e setters
    pessoa1.set_nome("José")
    print(pessoa1.get_nome())

    # Utilizando properties
    pessoa1.nome = "José"
    print(pessoa1.nome)

```

- Entenda o `if __name__ == "__main__":`  [aqui](https://www.alura.com.br/artigos/o-que-significa-if-name-main-no-python).
- Para mais informações sobre orientação a objeto em python, [clique aqui](https://www.treinaweb.com.br/blog/orientacao-a-objetos-em-python).



## Manipulação de arquivos

```python

```

## 