# OpenCode Workspace Configuration

Este repositório centraliza as configurações globais do assistente OpenCode, garantindo adaptabilidade entre diferentes contextos de desenvolvimento utilizando variáveis dinâmicas e automação de terminal.

## Ecossistema e Documentações Complementares

Esta configuração não atua de forma isolada. Ela é o motor de um ecossistema altamente acoplado. Devido a essa forte interdependência, a arquitetura foi desenhada com uma mentalidade modular, separando configurações, comportamentos e ferramentas.

Para compreender a totalidade da arquitetura, consulte as documentações complementares:

* **[Agents & Skills](https://github.com/gersonfribeiro-dev-configurations/.agents/blob/master/README.md):** Contém as diretrizes de comportamento (`AGENTS.md`), as habilidades mapeadas (`skills/`) e o escopo de atuação do assistente.

* **[Tools](https://github.com/gersonfribeiro-dev-configurations/.tools/blob/master/README.md):** Repositório de executáveis e scripts locais, responsável por fornecer os comandos de automação, como o inicializador adaptável do MCP do Pencil.

## Tecnologias e Ferramentas Integradas

* **Model:** OpenAI GPT-5.6 Terra
* **Gestão de Ambiente:** `direnv` e Variáveis de Sistema do Windows
* **Caminhos Base:** Utilização do atalho universal `~/` para compatibilidade cross-environment

### Model Context Protocol (MCP) Configurados

| Servidor | Tipo | Autenticação / Configuração |
| :--- | :--- | :--- |
| **GitHub Copilot** | Remoto | Bearer Token gerido via `direnv`. |
| **Context7** | Remoto | Chave de API injetada pelo ambiente (`CONTEXT7_API_KEY`). |
| **Pencil** | Local | Execução em background via `.bat` no ambiente Desktop. |
| **Docker Gateway** | Local | Profile dinâmico via variável (`PROFILE_TOOLKIT`). |

### Setup do Ambiente

1. Clone este repositório em sua máquina.
2. Defina uma variável de sistema chamada `PROFILE_TOOLKIT` com o valor padrão da sua máquina (ex: `developer_mcp`).
3. Para injetar configurações específicas de um projeto, crie um `.envrc` na raiz do diretório de destino e declare as variáveis.
4. Libere a leitura do diretório executando `direnv allow`.

### .envrc

A configuração padrão de cada MCP é feita de forma global permitindo sobreescritas por meio do arquivo .envrc que deve ser encontrado na raiz de cada projeto, com exceção do MCP do Github, que precisa de um PAT por escopo, já que estes são recomendados de serem usados no formato Fine-grained, dando permissões estritas ao MCP, inclusive a de acessos a repositórios.

#### Como configurar o arquivo .envrc?

Crie o arquivo na raiz do projeto e adicione esse conteúdo

```ts
export GITHUB_PERSONAL_ACCESS_TOKEN=$VARIAVEL_PROJETO
```

Para ter ainda mais segurança, você pode criar diversos PAT's, um para cada escopo, organização ou permissões que deseja fornecer ao MCP. Dessa forma não teremos um hardcode de PAT em cada projeto, pois esse arquivo .envrc geralmente é versionado.

#### Sobreescritas

A nossa configuração para o MCP do Toolkit também funciona permitindo a sobreescrita de profiles, ela assume o default carregando as variáveis de ambiente mas se dentro do .envrc tiver um outro valor, ela sobreescreve, aqui não precisa de variáveis de ambiente pois a segurança do profile depende de uma autenticação OAuth no Docker Desktop e afins.

```ts
export PROFILE_TOOLKIT=profile_diferente_do_default
```

Após essas configurações, abra o OpenCode no terminal integrado ao diretório onde o arquivo foi configurado, o powerShell vai carregar essas env's e permitir que o OpenCode conecte-se ao MCP do Github.
