# Template de relatório

Estrutura do relatório final do engajamento. Concretiza o que a seção 9 do [núcleo](../prompt/core-prompt.md)
descreve. O relatório fica no diretório de log, junto das regras de engajamento preenchidas (ver
[rules-of-engagement-template.md](rules-of-engagement-template.md)). Cada achado detalhado segue o
[template de achado](finding-template.md).

Regra de evidência em todo o relatório: nenhum dado sensível bruto é reproduzido aqui; a evidência é
referenciada por hash e caminho de artefato (ver [log-schema.md](log-schema.md)).

## Cabeçalho

- **Alvo**:
- **Data**:
- **Janela de teste**: (início, fim, fuso)
- **Classificação**: blackbox / graybox / whitebox
- **Regras de engajamento**: caminho do arquivo preenchido

## 1. Resumo executivo

Visão geral em linguagem acessível a quem não é técnico: o que foi testado, a postura geral observada, e o
nível de risco encontrado. Sem jargão desnecessário. Não inflar: se a postura geral foi boa, dizer isso.

## 2. Escopo e método

Classificação do teste, janela, e as fases da metodologia que foram executadas (reconhecimento, fingerprint,
triagem, aprofundamento). Registrar o que ficou fora de escopo ou não foi testado, e por quê.

## 3. Controles compensatórios observados

Resultado da fase de fingerprint (seção 5 do núcleo): WAF, CDN, rate limiting ou outro controle de borda
presente, ausente, ou inconclusivo, e como isso ajustou a agressividade das fases seguintes.

## 4. Tabela de achados

| ID | Título | Categoria OWASP | CWE | Severidade | Confiança | Ativo afetado |
|----|--------|-----------------|-----|------------|-----------|---------------|
|    |        |                 |     |            |           |               |

## 5. Achados detalhados

Um bloco por achado, no formato de [finding-template.md](finding-template.md), na ordem da tabela acima.

## 6. Observações sem achado formal

Itens de higiene ou de baixo valor isolado que não chegam a achado formal, registrados para o operador
decidir (ex.: exposição aceitável em dev, fingerprint de framework). Registrar como observação, nunca
elevar a achado só para o relatório parecer mais produtivo.

## 7. Sanitização de artefatos

Confirmação de que a sanitização da seção 8 do núcleo foi concluída (artefatos listados e removidos, com
confirmação do operador), ou a pendência explícita se não foi. Nunca declarar removido sem confirmação.

## 8. Apêndice: log

Caminho do log do engajamento, número de linhas, e confirmação de que o JSONL é válido (ver
[log-schema.md](log-schema.md)).

## Atribuição

A estrutura de relatório (sumário de resultados, achados, cobertura, recomendações) é inspirada no
[OWASP Secure Agent Playbook](https://github.com/OWASP/secure-agent-playbook) (licença CC-BY-4.0), escrita
do zero e adaptada ao contexto de teste autorizado ao vivo.
