+++
title = "Como usar o rsync no Windows com Cygwin"
description = "Como eu atualizo minha página e blog no Dreamhost com o conteúdo que crio localmente, usando rsync no Windows com Cygwin."
type = "post"
tags = [
    "rsync",
    "cygwin",
    "web-dev"
]
date = "2022-01-31"
draft = false
[ author ]
  name = "Mírian Bruckschen Motta Barros"
+++

## Introdução

Este post explica como atualizar sua página ou blog em seu servidor de hospedagem usando o rsync via Cygwin no Windows.  Quem usa Linux também pode achar esse tutorial útil, mas recomendo que pule direto para [minha explicação sobre o uso do rsync e minha escolha de opções](#como-executar-o-rsync-para-publicar-seu-site).

Não é objeto deste guia a criação do website ou do conteúdo.

### Pré-requisitos

O texto assume que você não tem medo de usar a linha de comando em um terminal, pois o rsync é um comando que é executado dessa forma.  Não vamos fazer mais que mudar de diretório e entrar o comando do rsync, mas se você quiser revisar ou aprender sobre linha de comando, [este tutorial das Djangogirls](https://tutorial.djangogirls.org/pt/intro_to_command_line/) é um ótimo ponto de partida.

### Por que usar o rsync?

O rsync é uma ferramenta muito usada em sistemas Linux para fazer backup e sincronizar arquivos em diferentes locais e servidores.  Ele também é muito útil para publicar novas versões de sites mantidos localmente, pois permite que você envie para seu servidor apenas os arquivos que foram modificados.  É justamente para este propósito que uso o rsync, e que descrevo neste guia para você usar também.

Como escrevi em [um post anterior](2022-01-27-hugo_ssg_tutorial-ptbr), eu crio minhas páginas e blog posts em Markdown localmente e gero as páginas HTML usando o Hugo.  O SSG Hugo gera um diretório `public/` com todo meu site, e é isso que preciso enviar para o servidor que hospeda o [mirianbr.com](https://mirianbr.com) quando faço alguma alteração.  Para não enviar todos os arquivos sempre, resolvi usar o rsync ou solução alternativa.

### E o Cygwin?

Depois de pesquisar [algumas](https://www.ubackup.com/windows-10/rsync-windows-10-1021.html) [opções](https://rdiff-backup.net/), decidi pelo [rsync](https://rsync.samba.org/) via [Cygwin](https://www.cygwin.com/).  Como não existe rsync para Windows, eu preciso de algum tipo de emulador, que é no que o [Cygwin](https://www.cygwin.com/) vem ajudar. Ele fornece um bom número de programas disponíveis apenas em Linux para um ambiente Windows -- inclusive o rsync.

## Instalação

Para usar o rsync via Cygwin no Windows, você precisa de dois pacotes: o rsync e o openssl. Para instalar ambos, siga os passos abaixo:

1. Baixe a versão mais atual do Cygwin em <https://www.cygwin.com/>
1. Siga o passo-a-passo da instalação até chegar na tela de seleção dos pacotes ("Cygwin Setup - Select Packages", conforme Figura 1 abaixo)

    ![Figura 1. Tela de seleção de pacotes no Cygwin.](/blog-images/select-packages.png)

1. Selecione a opção "Full" em **View**
1. Digite ``rsync`` em **Search** > Tecle ENTER

    ![Figura 2. Pesquisa pelo pacote rsync.](/blog-images/search-rsync.png)
1. Selecione uma das versões disponíveis na coluna **New** correspondente ao pacote chamado ``rsync`` (a opção padrão para este pacote é **Skip**, que não instalará o programa)

    ![Figura 3. Opção pela instalação, em substituição à opção padrão de "Skip".](/blog-images/install-rsync.png)

1. Digite ``openssl`` em **Search** > Tecle ENTER
1. Selecione uma das versões disponíveis na coluna **New** correspondente ao pacote chamado ``openssl`` (a opção padrão para este pacote também é **Skip**, e você vai precisar deste pacote para executar o rsync corretamente)
1. Clique em **Avançar** até finalizar sua instalação

A partir de agora você terá acesso a um terminal do Cygwin, disponível no seu menu iniciar. Abra o terminal e siga as instruções da próxima seção!

## Como executar o rsync para publicar seu site?

Antes de executar o rsync, mude para o diretório onde está o conteúdo que você quer publicar, usando o comando `cd`.  Veja o exemplo abaixo, que assume que o conteúdo está em ``C:\Users\Usuario1\meu-website``:

``$ cd /cygdrive/c/Users/Usuario1/meu-website``

> 💡 Seus drives C, D e demais drives no Windows estarão acessíveis dentro de `/cygdrive`. Altere o comando acima para o diretório em que está o conteúdo em seu computador.

> 🛑 Se você tentou mudar para um diretório não existente, você verá a mensagem ``No such file or directory``. Neste caso, corrija o comando para garantir que ele referencie o diretório correto.

Ao mudar de diretório com sucesso, o terminal do Cygwin vai mostrá-lo ao abrir novamente a linha de comando para você, como no exemplo abaixo:  

```
mirian@MEU-COMPUTADOR ~
$ cd /cygdrive/<diretório onde está meu site>

mirian@MEU-COMPUTADOR /cygdrive/<diretório onde está meu site>
$ 
```

Agora é hora de atualizar seu site!

O rsync tem muitas opções (veja todas em sua [man page](https://linux.die.net/man/1/rsync)). Vamos usar o comando com as opções abaixo:

``rsync -avzh --no-p <diretório com o conteúdo> <usuário no servidor>@<servidor de destino>:<diretório destino> --delete``

> 💡 Os sinais `<` e `>` não devem ser incluídos. Substitua o texto entre `<>` com seus próprios diretórios, usuário e servidor remoto.

Veja o que cada opção acima significa:

* ``-a`` significa ``archive``: esta opção é um resumo de várias outras opções; com ela, você copia recursivamente o conteúdo de todos os sub-diretórios e preserva todas as propriedades dos arquivos na transferência
* ``-v`` significa ``verbose``: você vai ver de forma bem detalhada a transferência, arquivo a arquivo
* ``-z`` significa ``compress``: seus dados serão compactados para transferir
* ``-h`` significa ``human-readable``: o tamanho dos seus arquivos vão aparecer não em bytes, mas em unidades de tamanho mais fáceis de ler (como Kb e Mb)
* ``--no-p`` significa ``no permissions``: sobrescreve parte da opção ``-a`` acima; ao sobrescrever arquivos no destino, você provavelmente não quer substituir as permissões também (veja o motivo na seção ao final dessa explicação)
* ``--delete``: esta opção remove arquivos no servidor de destino se eles foram removidos na origem

Antes de transferir os arquivos, você receberá uma mensagem exigindo a sua senha no servidor de destino.

Ao final da transferência, você verá um resumo da transferência, contendo:
* o tamanho total de arquivos enviados
* o tamanho total de arquivos recebidos (se houver algum alterado diretamente no servidor de destino), e 
* o tamanho total dos arquivos monitorados por mudanças para sincronização.

### Uma nota sobre permissões

Se os seus arquivos no servidor não tiverem as permissões de leitura e execução necessárias, seu site ficará inacessível.  Para testar, acesse seu site em um navegador.  Se em você ver uma página dizendo `403 Forbidden` em seu teste após fazer esta atualização, as permissões incorretas são a causa mais provável.

Para resolver este problema, você precisa acessar seu servidor de destino e mudar suas permissões. Você fazer isso também pela linha de comando, executando os comandos abaixo em sequência:

```
ssh <usuário no servidor>@<servidor de destino>
chmod -R 755 <diretório destino>
```

Depois disso, seu site deve ter voltado a ficar acessível -- e agora atualizado com seu novo conteúdo!

![Sucesso!](/blog-images/computer-presentation.jpg)

## Conclusão

Neste texto, expliquei como usar rsync no Windows com Cygwin para atualizar um site hospedado em um servidor remoto. Se você gera seu conteúdo localmente em Windows, esse guia deve ter ajudado você a publicá-lo mais facilmente. ✍🏽 

E então, o que achou? Este material foi útil pra você? Se você tem comentários ou mesmo sugestões para tornar este guia melhor, me contate no email hello@mirianbr.com! 🙃