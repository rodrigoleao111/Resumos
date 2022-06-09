# Comandos Git básicos

_Estou usando o Windows e o Git Bash._

[TOC]

## Roteiro para configuração inicial

1. Instale o Git na sua máquina ([Download for Windows](http://git-scm.com))
2. Informe ao Git seu nome: `git config --global user.name "Nome Sobrenome"`
3. Informe ao Git seu email: `git config --global user.email "seu_email@email.com"`

## Roteiro para iniciar versionamento

1. Abrir diretório a ser versionado: `cd NomeDaPasta/pastaDestino`  |  Untracked
2. Inicializar um repositório git: `git init`  |  Unstaged
3. Verificar status do repositório: `git status` |  Unstaged
4. Incluir arquivos para serem enviados: `git add .` |  Unstaged
5. Adicione um commit para finalizar o versionamento: ` git commit -m 'meu comentário'` | Staged

## Roteiro para upar repositório no GitHub

### Preparação inicial

1. Crie sua conta no [GitHub]( https://github.com/) 
2. Volte ao git bash e crie sua chave SSH : `ssh-keygen -t rsa -C "seu_email@email.com"`
3. Invente uma senha, que será requisitada toda vez que você for fazer um push (veremos esse comando na próxima etapa)
4. Abra o bloco de notas que contém sua chave SSH com o comando `notepad ~/.ssh/id_rsa.pub`
5. Volte no GitHub e vá em "Settings" > "SSH and GPG Keys" > "New SSH key"
6. Copie e cole sua chave e clique em "Add Key"
7. Volte no terminal e finalize a conexão com o comando: `ssh -T git@github.com`
8. Digite `yes` e informe sua senha. Caso ocorreu tudo certo, você receberá uma mensagem do tipo: `Hi nomeDeUsuario! You’ve successfully authenticated, but GitHub does not provide shell access.`

### Criando um repositório remoto

1. Acesse o GitHub e crie um novo repositório (coloque um nome e uma descrição)
2. Copie o endereço _(SSH no meu caso)_ e volte para o terminal
3. Vincule o repositório local com o projeto do GitHub: `git remote add origin 'endereçoCopiado'`
4. Insira sua senha
5. Verificar se o endereço foi adicionado corretamente: `git remote show origin -n`
6. **Caso algo dê errado**, utilize o seguinte comando para remover o vínculo: ` git remote rm`

## Roteiro para atualizar repositório remoto (Push)

1. Envie as atualizações do projeto local para o GitHub com o comando: ` git push origin main`
2. 

## Roteiro para atualizar repositório local (Clone / Pull)




