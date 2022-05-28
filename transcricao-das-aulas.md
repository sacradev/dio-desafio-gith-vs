## Transcrição das aulas do curso **Introdução ao Git e ao GitHub** ministrado por Otávio Reis.



### Aula 1 e 2: Introdução ao Git / Navegação via command line interface e instalação

- Navegação básica

A forma de interagir com o git é por linha de comando hoje há interface gráfica, mas a base não é assim. Para o curso, usaremos linhas de comando.

O que vamos aprender:

- mudar de pastas
- listar pastas
- criar pastas/arquivos
- deletar pastas/arquivos

**Comandos para o windows**

`cd` (change directory) = caminhar nos diretorios

`cd ..` = retorna pro diretório anterior

`dir` = listar os diretorios
`mkdir` = make directory = criar pastas
`del` = deletar todos os arquivos de um diretorio, atenção ao usar.

`rmdir` = remove, desde que tudo esteja vazio. As flags /S /Q ajudam a pular esse impedimento

`echo` = ecoa/imprime/reproduz o que for digitado no terminal

`echo hello > hello.txt` = o > é um redirecionador de fluxo
que verifca se há o arquivo existente, caso não haja, ele o cria.

`cls` (clear screen_ = limpar o terminal

tab = auto complete = preenche automaticamente

instalação do git

entrar no site: *git-scm.com*

### Aula 3: Entendendo como o Git funciona por debaixo dos panos

#### Tópicos fundamentais para entender o funcionamento do Git

- SHA1

Secure Hash Algorithm (algoritmo de encriptação)
Vai pegar o algoritmo e embaralhar de uma forma específica, a fim de proteger.

definição: A sigla SHA significa *Secure Hash Algorithm*. É um conjunto de funções *hash* criptográficas projetadas pela NSA (agência de segurança nacional do EUA). Essa encriptação gera um conjunto único de 40 caracteres que serve como identificador. Cada mudança no seu arquivo, irá gerar um novo conjunto fazendo como que seu arquivo esteja sempre protegido. porém se voltar pra alguma versão anterior, o código gerado será o mesmo daquela versão retornada.

Ver exemplo na aula em 03:27.

echo "ela mundo" | openssl sha1
a saída sera um > (stdin) = "chave de 40 digitos"

#### Objetos fundamentais

Tipos básicos de objetos responsáveis por versionamento do código:

- Blobs: Contem: tipo, o tamanho da string/arquivo, uma \0 e o conteúdo do arquivo. obs: gera metadados, por isso os componentes extras.

  ex prático:

`echo 'conteudo' | git has-object --stdin`
`stdin` = a função espera receber um arquivo e a gnt tá enviando um texto, por isso usa a flag pra função entender. essa função vai retornar um sha1 da *string* 'conteudo'. Se fizermos a msm coisa, usando o `openssl`, vai retornar outro sha1, pois a estrutura gerado acima é diferente da gerada com `openssl`. Para gerar o msm sha, é preciso aplicar a mesma estrutura citada acima, ex:

`echo -e 'blob 9\0conteudo' | openssl sha1`

Assim, serão os msm arquivos e terão o msm sha1.

- Trees

  Características: armazenam os `blobs`, Também vai conter metadados: \0, apontando para `blobs`, com sha1 e guardando o nome dos artigos, diferente do `blobs`, que guarda apenas o sha1, Ou seja, toda a estrutura da localização dos arquivos são encontradas nas `tree`. Assim, uma árvore ponde apontar outras árvores, como acontece com diretorios. obs: as arvores possuem seu sha1. Em um efeito cascata, uma alteração no arquivo apontado pelo `blobs`, vai alterar o seu sha1 e por consequência, alterar o sha1 da `tree`.

ex de uma tree em 07:45

- Commits
  **o mais importante**
  é o objeto que vai juntar todos, dará sentido a tudo que está sendo feito, pois:
  - aponta para a tree
  - aponta para o parente (último commit realizado antes dele)
  - aponta para um autor
  - aponta para uma mensagem

Estes últimos reforçam a ideia de sentido.  Os objetos apontados podem ser explicados na mensagem e o autor reforçado, dando veracidade ao que foi feito. possuem sha1, o que garante a segurança. Voltando, se alterar a *blobs*, altera seu sha1, que altera o sha1 da *tree*, que altera o sha1 do *commit*. O sha1 do *commit* é o ponto máximo de segurança, pois a menor das alterações feitas no artigo irá alterar o *commit*, revelando a interferência no seu código.
ver ex no vídeo em 12:00


SISTEMA DISTRIUÍDO - SEGURANÇA

É por tudo supracitado que ele tem essa característica.

#### Chaves SSH e Tokens

Chave ssh: forma de estabelecer ligação segura e encriptada entre duas máquinas.

Criando ssh
comandos:

1) `ssh-keygen -t ed25519 -C "seu e-mail"`
2) digitar senha
3) ir até a pasta salva
4) verificar os arquivo: dir, deve retornar a chave publica e privada
5) aplicar cat para visualizar o conteúdo das chaves.
6) copiar a saída, deverá ser sua chave pública e colar no github.
7) iniciar o `ssh-agent` a partir do código `eval $(ssh-agent -s)`
8) após carregar o agente, precisamos passar a chave privada para ele a partir do codigo `ssh-add (chave)`
9) após o enter, ele vai pedir a senha e vai retornar o resultado de identidade adicionada, verifique id e seu *e-mai*l
10) está feita a ligação

### Aula 4: Primeiros comandos com GIT.

*tip: todo comando do git leva o termo "git" no começo.*

- Criando um repositório.

`git init` = iniciar o git dentro do repositório / cria um repositório git dentro do diretorio (pasta).

`ls -a` = flag que mostrar pastas/arquivos ocultos, que começam com . (ponto).

Antes de criar os arquivos, a primeira vez do usuário, o git irá pedir para criar um nickname e email.
essa informação é para saciar a necessidade de autor dos *commits*, para isso:

`git config --global user.email "seu e-mail do github"`

`git config --global user.name "seu nome de usuário do github"`

*essa flag --global visa cobrir uma configuração total, o mesmo procedimento poderia ser feito de forma única, porém não faz sentido.*

Adicionando arquivo a pasta:

Usaremos o arquivo do tipo markdown. Ele é a forma mais orgânica de escrever um arquivo do tipo html.
*Note: foi preciso estabelecer um padrão em relação ao crlf e lf, warning foi retornado, pois um terminador de texto indesejado estava presente é provável que ela tenha vindo do site que copiamos o texto. o codigo git config core.autocrlf input resolveu o problema ele gerou um padrão de encerramento corrigo pelo próprio git.* 

- Criando o *commit*

`git commit -m "commit inicial"`, Esse comando vai preencher o requisito mensagem, essa mensagem deve ser usada como aviso do que foi feito em cada *commit*.

--------------------------------------------------------------------

### Aula 5: Ciclo de vida dos arquivos no Git.

Tracked são arquivos rastreados pelo git ou arquivos que o git reconhece,sabe da existência. Dividem-se em:

- *Unmodified*: arquivo que ainda naõ foi modificado,
- *Modified*: arquivo que sofreu modificação, essa modificação ocorre pela observação do sha1, se for diferente, o arquivo passou de unmodified pra modified,
- *Staged*: conceito de *backstage* de palco. Em teoria, os arquivos estariam se preparando para um novo argrupamento, o commit, ou seja, envelopamos todos os arquivos e eles deixam de ser staged, se tornam commit e eles retornam pra unmodified,  pois qualquer mudança irá gerar um novo sha1 em toda estrutura. É como se o commit tirasse uma foto do momento e qualquer coisa diferente daquilo deixa de ser unmod e passa a ser mod.

Quando usamos o `git add *`, o arquivo era *untracked* antes disso (o git não sabia da sua existência), o comando em questão  moveu o arquivo.md para *staged*, para a região de espera para ser usado.

##### E o que são os repositórios?

é importante pensar que nossa máquina é um ambiente de desenvolvimento e tudo gerado precisa ser alocado em um repositório, que por sua vez vai estar localizado em um servidor remoto. Essa estrutura corrobora com a informação inicial de que o git é um sistema distribuído pois parte dos serviços realizados estão na máquina desenvolvedora e no servidor. As ações realizadas na maquina só serão espelhadas no servidor remoto mediante execução de código específico solicitando isso. (passamos por isso quando fizemos o exercício do repositório)

Estrutura

Ambiente de desenvolvimento:
- *working diretory* (livro de receitas)
- *staging area* (os arquivos vão estar sempre alternando entre aqui e o repositório)
- *local repository* (passa a integrar a partir do *commit*, e a partir daqui pode ser empurrado para o repositório remoto) tudo que está aqui, vai estar commitado. 

Sem isso, é impossível enviar para o repositório remoto.

*tip (o git status é um comando que fala como está nosso arquivo/repositório)*

--------------------------------------------------------------------------

### Aula 5: Introdução ao GitHub

- empurrar os arquivos do ambiente de desenvolvimento para o repositório remoto

1) `git remote add origin` "link do seu repositório remoto" *note: origin é um apelido para não ter que digitar a url todas as vezes*
2) `git push origin master`//O processo então será realizado.t

---------------------------------------------------------------------------------

### Aula 6: Resolvendo Conflitos

- Como os conflitos acontecem

Conflito de merge: quando existem duas alterações na mesma linha e tentam subir. O github aciona o erro para que
o programador resolva. Ao tentar comitar uma nova versão, a mensagem de confito será dada.

Processo de resolução:

1. `git pull` = a função irá trazer o conteúdo do GitHub para a máquina
2. as correções serão feitas no arquivo

3. o processo de empurrar deve ser iniciado novamente.


- como baixar um código do github

ex: repositório python

`git clone` https://github.com/python/cpython.git //url do repositório.