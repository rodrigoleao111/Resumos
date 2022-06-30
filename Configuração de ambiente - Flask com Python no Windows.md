# Configuração de ambiente: Flask com Python no Windows

## Instalação (usei o terminal do VS Code)

1. Criair o diretório do projeto e entrar nele

   `mkdir myproject`  e  `cd myproject`

2. Criar um ambiente com uma pasta venv

   `py -3 -m venv venv`

3. Ativar o ambiente

   1. `source venv/Scripts/activate` (caso não reconheça o _source_, use `venv\Scripts\activate`)

   2. Obs.: caso dê problema de o arquivo não poder ser carregado![image-20220607152452532](C:\Users\Rodrigo Leão\AppData\Roaming\Typora\typora-user-images\image-20220607152452532.png)

      Executar o comando `Set-ExecutionPolicy Unrestricted -Scope Process` e repetir o passo 3.1

      Para ver mais soluções, [clique aqui](https://stackoverflow.com/questions/18713086/virtualenv-wont-activate-on-windows).

4. Instale o Flask

   `pip install Flask`



## Hello, World!

1. Dentro do diretório, crie um novo arquivo teste.py com o seguinte código:

   ```python
   from flask import Flask
   
   app = Flask(__name__)
   
   @app.route("/")
   def hello_world():
       return "<p>Hello, World!</p>"
   ```

2. No terminal (Power Shell), informe que a aplicação irá trabalhar explorando o ambiente FLASK_APP

   `$env:FLASK_APP = "teste"` 

3. Rode a aplicação com `flask run`
4. Deverá aparecer o endereço hhtp do servidor, onde você poderá ver o "Hello, World!"