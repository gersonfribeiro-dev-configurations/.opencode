# 🛠️ Guia de Configuração do Ambiente (OpenCode MCP)

Este guia detalha a configuração necessária para que os MCPs (Model Context Protocol) funcionem corretamente, especialmente o chaveamento de tokens do GitHub entre diferentes organizações.

## 1. Chaveamento de Contexto com `direnv` (Obrigatório)

Como trabalhamos com múltiplos repositórios e organizações, utilizamos o arquivo `.envrc` para definir qual token deve ser utilizado no projeto atual. Para que o terminal carregue essas variáveis automaticamente, é necessário instalar o `direnv`.

### O que é o `direnv` e como funcionam os Hooks?

O `direnv` funciona como um "vigia" do seu terminal. Ele utiliza um **hook** (um gatilho de execução) que é disparado toda vez que o prompt do shell é renderizado. Quando você muda de pasta, o hook verifica se existe um arquivo `.envrc` e, se autorizado, injeta as variáveis ali definidas no seu ambiente atual de forma dinâmica.

### Instalação no Windows (Git Bash)

1. **Download**: Baixe o binário do `direnv` no [GitHub oficial](https://github.com/direnv/direnv/releases), ex: `direnv-windows-amd64.zip`, extraia para obter o `direnv.exe`.
2. **Configuração de Path**: Adicione o executável `direnv.exe` à pasta `~/.tools/`. Certifique-se de que a pasta `~/.tools/` esteja nas Variáveis de Ambiente do Sistema -> Path.
3. **Habilitação do Hook (Global)**: O hook deve ser configurado no seu arquivo de configuração global do Bash.
   - Abra o arquivo `~/.bashrc` (ou crie-o em `C:\Users\SeuUsuario\.bashrc`).
   - Adicione a seguinte linha:

     ```bash
     eval "$(direnv hook bash)"
     ```

   - Reinicie o terminal ou execute `source ~/.bashrc`.
4. **Ativação**: Ao entrar na pasta do projeto, o `direnv` detectará o `.envrc`. Digite o comando abaixo para autorizar a carga das variáveis:

   ```bash
   direnv allow
   ```

**Importante sobre o `direnv allow`**:

- Deve ser executado **somente uma vez** por diretório.
- Deve ser executado novamente **sempre que o conteúdo do arquivo `.envrc` for alterado**.

## 2. Variáveis de Ambiente

O `opencode.json` utiliza a variável `{env:GITHUB_PERSONAL_ACCESS_TOKEN}`.

- **Como funciona:** O `direnv` lê o arquivo `.envrc` da pasta atual e exporta o token específico daquele projeto para a variável `GITHUB_PERSONAL_ACCESS_TOKEN`.
- **Segurança:** Nunca versione o `.envrc` com tokens reais. Use variáveis de sistema (ex: `$PAT_ORG_X`) dentro do `.envrc` para referenciar seus tokens secretos armazenados no Windows.

## 3. Fluxo de Trabalho Resumido

| Componente | Responsabilidade | Status |
| :--- | :--- | :--- |
| **Variáveis de Sistema** | Armazenar tokens secretos de cada Org | Obrigatório |
| **`.envrc`** | Mapear o Token da Org para `GITHUB_PERSONAL_ACCESS_TOKEN` | Obrigatório por projeto |
| **`direnv`** | Injetar as variáveis do `.envrc` no terminal | Obrigatório |
| **`opencode.json`** | Consumir a variável `GITHUB_PERSONAL_ACCESS_TOKEN` | Já configurado |
| **Pen App** | Interface de Design | Aberta para usar MCP Pencil |
