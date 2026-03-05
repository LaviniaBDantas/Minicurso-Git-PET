# Lab 02: Branches, Merge e Reversão

Este laboratório tem como objetivo colocar em prática o uso de ramificações (branches) para desenvolvimento paralelo, a união dessas histórias (merge) e como desfazer alterações de forma segura.

## 1. Compreendendo e Criando Branches

Uma branch (ramificação) é apenas um ponteiro leve para um commit específico. A branch padrão chama-se `master` (ou `main`), e o `HEAD` é um ponteiro especial que indica onde você está agora. Trabalhar com branches permite o desenvolvimento paralelo sem afetar o código principal.

Vamos começar criando uma nova ramificação no nosso projeto:

1. Verifique em qual branch você está atualmente (o asterisco indica o `HEAD`):
   ```bash
   git branch
   ```

2. Crie um novo ponteiro chamado `testing`, mas não mude para ele ainda:
   ```bash
   git branch testing
   ```

3. Mova o `HEAD` para a nova branch e atualize os arquivos usando o comando checkout:
   ```bash
   git checkout testing
   ```

   **Dica:** Existe um atalho para criar e mudar imediatamente de branch: `git checkout -b <nome>`.

4. Agora, faça algumas alterações para criar um histórico independente:
   - Crie um arquivo chamado `recursos.txt` e adicione algum texto.
   - Adicione as mudanças à Staging Area (`git add recursos.txt`).
   - Faça o commit com a mensagem "Adiciona arquivo de recursos" (`git commit -m "..."`).

## 2. Unindo Histórias (Merge)

Agora que temos alterações na nossa branch `testing`, vamos uni-la de volta à nossa branch principal. O Git possui diferentes tipos de merge.

### Fast-forward Merge

Ocorre quando não há divergência e o ponteiro apenas avança.

1. Volte para a branch `master`:
   ```bash
   git checkout master
   ```
   
   (Note que o arquivo `recursos.txt` desapareceu do seu diretório. Isso ocorre porque o `HEAD` e seus arquivos voltaram para o estado do snapshot da `master`).

2. Execute o merge da branch `testing` para a `master`:
   ```bash
   git merge testing
   ```

Como a `master` não sofreu alterações enquanto você trabalhava na `testing`, o Git apenas move o ponteiro para frente.

## 3. Lidando com Conflitos (Three-way Merge)

Um Three-way merge ocorre quando há mudanças divergentes, e o Git precisa criar um novo commit de união. Conflitos acontecem quando a mesma linha é alterada de formas diferentes em branches diferentes.

Vamos simular esse cenário:

1. Crie e mude para uma nova branch chamada `nova-feature`:
   ```bash
   git checkout -b nova-feature
   ```

2. Edite a primeira linha do arquivo `recursos.txt` para: `Texto modificado na nova feature`.

3. Faça o `add` e o `commit` dessa alteração.

4. Volte para a `master` (`git checkout master`).

5. Edite a mesma linha do arquivo `recursos.txt` para: `Texto modificado diretamente na master`.

6. Faça o `add` e o `commit` dessa alteração na `master`.

7. Agora temos histórias divergentes. Tente fazer o merge:
   ```bash
   git merge nova-feature
   ```

O Git pausa o merge pois encontrou um conflito. Você deve editar o arquivo manualmente.

8. Abra o arquivo `recursos.txt`. Você verá marcações injetadas pelo Git:
   ```
   <<<<<<< HEAD
   Texto modificado diretamente na master
   =======
   Texto modificado na nova feature
   >>>>>>> nova-feature
   ```

   - `<<<<<<< HEAD`: Representa o conteúdo na branch atual (`master`).
   - `>>>>>>> nova-feature`: Representa o conteúdo na branch que está chegando.

9. Para resolver:
   - Apague as marcações do Git (`<<<<<<<`, `=======`, `>>>>>>>`).
   - Ajuste o texto para a versão final que você deseja manter.
   - Salve o arquivo.
   - Rode `git add` para marcar como resolvido e conclua com um commit:
     ```bash
     git add recursos.txt
     git commit -m "Resolve conflito no recursos.txt"
     ```

## 4. Desfazendo Mudanças (Reset vs Revert)

Se cometermos um erro no histórico, o Git nos oferece formas de voltar no tempo.

### Reset (Perigoso / Local)

O comando `git reset --hard HEAD~1` move o `HEAD` para trás.

**Atenção:** Ele apaga o histórico se você não tiver cuidado.

**Uso ideal:** Bom para erros locais que ainda não foram compartilhados na nuvem (GitHub).

### Revert (Seguro / Público)

O comando `git revert <commit-id>` cria um novo commit que é o inverso do anterior.

**Atenção:** É seguro para histórico que já foi enviado (push) para um repositório remoto, pois não apaga o passado, apenas adiciona uma correção.
