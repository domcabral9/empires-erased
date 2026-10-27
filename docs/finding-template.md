# Template de achado

Um achado por ocorrência confirmada. Preencha um bloco destes para cada achado do engajamento; o conjunto
alimenta a tabela e a seção de achados detalhados do [relatório](report-template.md). A estrutura é
inspirada na anatomia de achado do OWASP Secure Agent Playbook (ver atribuição no fim).

A disciplina de evidência vale para todo campo: nunca reproduza dado sensível bruto aqui. Referencie a
evidência por hash e por caminho de artefato, conforme [log-schema.md](log-schema.md) e a seção 4 do
[núcleo](../prompt/core-prompt.md).

## Bloco de achado

- **ID**: sequencial por engajamento (001, 002, ...).
- **Título**: `[SEVERIDADE] descrição curta e objetiva`
- **Categoria OWASP**: ex.: A01:2021.
- **CWE**: o mais específico aplicável, verificado contra a tabela de referência ou busca confiável (seção
  7 do núcleo). Nunca um identificador inventado.
- **CVE**: referência pública verificada quando houver caso análogo, ou, literalmente, "sem CVE público
  verificado associado a este achado".
- **OpenCRE** (opcional): CRE e padrões vinculados (ASVS/WSTG/NIST 800-53) quando o enriquecimento estiver
  disponível (ver [owasp-cwe-cve-reference.md](owasp-cwe-cve-reference.md)). Deixe vazio se não enriquecido.
- **OWASP ref**: referência aplicável (Top 10, ASVS, WSTG).
- **Localização**: ativo, rota, parâmetro ou componente afetado.
- **Exposição**: anônimo, usuário autenticado (qual papel), ou administrador. Entra no raciocínio de
  severidade.
- **Impacto**: o que um atacante consegue, em uma ou duas frases.
- **Evidência**: hash da evidência bruta (`evidence_hash` do log) e caminho do artefato quando houver. Nunca
  o dado sensível em claro.
- **Severidade**: Baixa / Média / Alta / Crítica, com o raciocínio explícito (exposição, o que está em
  risco, controle compensatório presente), conforme a seção 7 do núcleo e o método de
  [owasp-cwe-cve-reference.md](owasp-cwe-cve-reference.md). Registre o rótulo e o porquê, nunca só o rótulo.
- **Confiança**: ALTA / MÉDIA / BAIXA (ver tabela abaixo).
- **Remediação**: correção específica, com referência a um padrão quando houver.

## Escala de severidade

O projeto usa a escala qualitativa Baixa / Média / Alta / Crítica, estimada pelo método de
[owasp-cwe-cve-reference.md](owasp-cwe-cve-reference.md) (exposição, o que está em risco, controle
compensatório), não um CVSS calculado. Se um CVSS real estiver disponível, pode acompanhar o rótulo, mas o
rótulo qualitativo e o raciocínio são o que o relatório exige.

## Níveis de confiança

| Nível | Significado |
|---|---|
| ALTA | Confirmado por análise de caminho/comportamento, prova de conceito reproduzível, ou saída de ferramenta vetada. |
| MÉDIA | Indícios fortes, mas não plenamente confirmado; precisa de verificação manual adicional. |
| BAIXA | Correspondência heurística ou por padrão; pode ser falso positivo. |

A confiança é independente da severidade: um achado pode ser de severidade Alta e confiança BAIXA (potencialmente
grave, ainda não confirmado). Marcar a confiança real, em vez de inflar para ALTA, é a mesma disciplina
anti-alucinação da regra de nunca inventar CWE/CVE. Um achado de confiança BAIXA é registrado como tal, com o
que falta para confirmá-lo, nunca apresentado como certo.

## Exemplo preenchido

- **ID**: 001
- **Título**: [Baixa] Enumeração de organização via mensagem de erro diferenciada no login
- **Categoria OWASP**: A07:2021
- **CWE**: CWE-203 (Observable Discrepancy)
- **CVE**: sem CVE público verificado associado a este achado
- **OpenCRE**: (não enriquecido)
- **OWASP ref**: OWASP Top 10 A07:2021; WSTG-IDNT
- **Localização**: `POST /auth/login`
- **Exposição**: anônimo (sem credencial)
- **Impacto**: um ator anônimo distingue organizações existentes de inexistentes, útil como reconhecimento
  para um ataque direcionado.
- **Evidência**: log hash `3ab1...e09f`; artefato no diretório isolado do engajamento.
- **Severidade**: Baixa. Raciocínio: alcançável sem autenticação (aumenta), mas só expõe metadado de
  existência, sem leitura de dado nem ação (reduz), com rate limit presente mitigando enumeração em massa
  (reduz).
- **Confiança**: ALTA. Confirmado por comparação sistemática de respostas existentes versus inventadas.
- **Remediação**: unificar a mensagem de erro para ser idêntica em todos os casos de falha, replicando o
  comportamento anti-enumeração já usado no fluxo irmão.

## Atribuição

A anatomia deste template (campos de achado, níveis de confiança) é inspirada no
[OWASP Secure Agent Playbook](https://github.com/OWASP/secure-agent-playbook) (licença CC-BY-4.0). O texto
foi escrito do zero e adaptado ao contexto de teste autorizado ao vivo, com a escala de severidade
qualitativa própria do projeto.
