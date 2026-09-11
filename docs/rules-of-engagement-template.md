# Template de regras de engajamento

Preencha este documento antes de colar o [prompt central](../prompt/core-prompt.md) em qualquer LLM.
O prompt se recusa a iniciar teste ativo sem essas respostas. Guarde o arquivo preenchido junto com os
logs do engajamento, fora do controle de versão se contiver qualquer dado real.

## Identificação

- **Alvo (nome do sistema/aplicação)**:
- **Referência de autorização** (contrato, e-mail, ticket, ou "sou o dono do sistema"):
- **Responsável pelo alvo durante a janela de teste**:
- **Contato de escalonamento em caso de incidente**:

## Escopo

- **Hosts/domínios em escopo**:
- **Hosts/domínios explicitamente fora de escopo**:
- **Ambientes em escopo** (produção, staging, desenvolvimento):
- **Ações fora de escopo** (ex.: DoS, engenharia social, teste físico):

## Janela de teste

- **Início**:
- **Fim**:
- **Fuso horário**:

## Classificação do teste

- **Modelo de acesso**: blackbox (nenhum conhecimento interno) / graybox (documentação e/ou credencial
  de baixo privilégio fornecida) / whitebox (código-fonte disponível)
- **Credenciais fornecidas** (se houver, nível de privilégio de cada uma):
- **Documentação fornecida** (arquitetura, diagramas, especificação de API):

## Nível de agressividade permitido

- **Testes ativos autorizados** (ex.: injeção em campos de formulário, enumeração de rotas):
- **Testes que exigem confirmação explícita antes de cada execução** (ex.: qualquer payload autoral
  não vindo de wordlist conhecida, qualquer ação que altere dado real):
- **Testes proibidos mesmo com autorização de escopo geral** (ex.: exfiltração de dado real além do
  mínimo necessário para prova de conceito, força bruta de credencial real):
- **Cadência de confirmação exigida**: por fase (checkpoint a cada transição da
  [metodologia](methodology.md)) / por ação (confirmação a cada teste ativo individual) / só antes do
  deep-dive (padrão do prompt central, sem checkpoint adicional nas fases anteriores):

## Ambiente de execução

- **Diretório de log validado e gravável**:
- **Diretório isolado para artefatos/payloads gerados durante o teste**:
- **Sistema operacional/shell do operador** (Linux, Windows, contêiner):

Este template preenchido e o relatório final ficam no diretório de log. Só evidência gerada durante o
teste (payload salvo, captura de resposta, etc.) vai para o diretório de artefatos.

## Assinatura

- **Operador**:
- **Data**:
