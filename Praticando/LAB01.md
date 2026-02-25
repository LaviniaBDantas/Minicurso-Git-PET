# Lab 01: Fundamentos de Git

Este laboratório tem como objetivo introduzir na prática os conceitos fundamentais de controle de versão com **Git** que vimos na Aula 1 (usaremos Java como exemplo, mas serve para qualquer linguagem).

---

## 1. O que é Controle de Versão (VCS)?

O Git é um sistema que registra mudanças em arquivos ao longo do tempo.

- **Por que usar?** Ele permite reverter arquivos ou o projeto inteiro para um estado anterior e identifica quem modificou algo e quando.
- **Distribuído:** Diferente de sistemas antigos, no Git (que é o "motor") os clientes espelham totalmente o repositório em suas próprias máquinas.  
Você não precisa de internet para usá-lo localmente.

---

## 2. Preparando o Terreno (Instalação e Configuração)

Se você ainda não tem o Git instalado, baixe a versão para o seu sistema operacional em [https://git-scm.com/](https://git-scm.com/) e faça a instalação padrão.

Antes de começarmos a viajar no tempo, precisamos dizer ao Git quem somos.  
Cada *commit* (registro) carregará a sua assinatura.

Abra seu terminal (Git Bash no Windows, ou o terminal integrado do VS Code/IntelliJ) e execute:

```bash
# Defina sua identidade (mantenha as aspas)
git config --global user.name "Seu Nome Completo"
git config --global user.email "seu.email@exemplo.com"

# Se você usa o VS Code, defina-o como seu editor padrão do Git:
git config --global core.editor "code --wait"

# Verifique se as configurações foram salvas corretamente:
git config --list
````

---

## 3. Criando seu Primeiro Projeto Java

Você pode usar o IntelliJ IDEA, o VS Code ou até mesmo um bloco de notas.

1. Crie uma pasta no seu computador chamada `primeiro-projeto-git`.
2. Abra essa pasta na sua IDE ou editor de texto favorito (VS Code ou IntelliJ).
3. Crie um arquivo chamado `Application.java` (ou dentro de uma pasta `src`, caso sua IDE crie a estrutura automaticamente).
4. Adicione um código simples:

```java
public class Application {
    public static void main(String[] args) {
        System.out.println("Hello World, Git!");
    }
}
```

---

## 4. O Ciclo de Vida do Git na Prática

O Git pensa nos dados como "fotos" (*snapshots*) e trabalha com 3 áreas principais:

* **Working Directory (Diretório de Trabalho):** Onde você edita os arquivos.
* **Staging Area (Área de Preparação):** O "palco" que prepara o que vai para o próximo registro.
* **Git Directory (.git):** O banco de dados com o histórico permanente.

### Fluxo Básico de Comandos

No terminal, dentro da pasta do seu projeto, execute os passos abaixo:

### 1. Transforme a pasta comum em um repositório Git:

```bash
git init
```

Isso cria a pasta oculta `.git`.

### 2. Verifique o estado dos arquivos:

```bash
git status
```

O arquivo `Application.java` aparecerá em vermelho como **Untracked** (não rastreado), pois é novo e o Git ainda não o conhece.

### 3. Mova o arquivo para a Staging Area (Palco):

```bash
git add Application.java
# Dica: use 'git add .' para adicionar todos de uma vez
```

Rodando `git status` novamente, ele ficará verde (**Staged**), pronto para a foto.

### 4. Tire a foto (Commit):

```bash
git commit -m "feat: Adiciona classe inicial Application"
```

Isso move o que está no Staging para o repositório permanente com uma mensagem clara do que foi feito.

---

## 5. Ignorando o que não importa (.gitignore)

Em projetos reais, arquivos temporários, senhas e pastas geradas pelas IDEs (como `.idea/` do IntelliJ ou `.vscode/` do VS Code) não devem ir para o Git.

1. Crie um arquivo chamado `.gitignore` (o ponto no início é obrigatório) na raiz do projeto.
2. Adicione os itens que o Git deve ignorar:

```plaintext
.idea/
.vscode/
build/
out/
*.class
*.log
```

3. Adicione e faça o commit do seu arquivo de configuração:

```bash
git add .gitignore
git commit -m "Config: Adiciona arquivo .gitignore"
```

---

## 6. Viajando na História e Botão de Pânico

### Ver a linha do tempo (Log)

Mostra o autor, data, hash e a mensagem de todos os commits realizados:

```bash
git log --oneline
```

### Botão de Pânico (Desfazendo mudanças locais)

Editou um arquivo, salvou, mas se arrependeu e ainda não fez o commit?
Recupere a versão anterior (salva no último commit):

```bash
git restore Application.java
```

### Desafio Extra

Tente visualizar suas branches e commits de forma interativa em: [Learn Git Branching](https://learngitbranching.js.org/?locale=pt_BR)

