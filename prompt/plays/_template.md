# Play: <nome da categoria> (<código OWASP>)

Esqueleto de play. Copie este arquivo, renomeie para o código da categoria e preencha cada seção. Mantenha
a altitude de metodologia (o que procurar e como caracterizar), nunca payload pronto para colar: payload
autoral é construído em runtime sob o ciclo de vida da seção 8 do [núcleo](../core-prompt.md).

## Quando rodar

O sinal de triagem (fase 3) que justifica carregar esta play. Se a triagem não produziu esse sinal, a
play não deve ser carregada.

## Pré-requisitos

- Fase de aprofundamento autorizada pelo operador para esta categoria (ver seção 6 do núcleo).
- Superfície relevante para a categoria existe no alvo (ex.: uma categoria de injeção não se aplica a um
  alvo sem nenhum ponto de entrada de dado).
- Qualquer condição específica da categoria.

## Objetivo

O que esta play busca confirmar ou caracterizar, em uma ou duas frases. O objetivo é evidência
suficiente para um achado, não exploração além do mínimo necessário para a prova de conceito.

## Sinais de triagem

Checagens leves e de baixo custo, usadas na fase 3 (antes de carregar esta play), que indicam se vale
aprofundar nesta categoria. São o gatilho descrito em "Quando rodar", listados aqui para referência.

## Procedimento de aprofundamento

Passos numerados, em nível de metodologia. Cada passo diz o que observar e como decidir, não um comando
pronto. Quando um passo exigir um payload autoral, referencie a seção 8 do núcleo em vez de embutir o
payload aqui.

## Evidência a coletar

O que registrar no log (ver [log-schema.md](../../docs/log-schema.md)): hash da evidência bruta e
descrição do que foi observado, nunca o dado sensível em texto puro.

## Severidade

Considerações específicas da categoria para a estimativa de severidade (exposição, o que está em risco,
controle compensatório presente), complementando a seção 7 do núcleo.

## Encerramento

Se a play gerou algum artefato (payload salvo, captura), ele entra na sanitização da seção 8 do núcleo.

## Referências

- Categoria OWASP e CWEs aplicáveis (ver [owasp-cwe-cve-reference.md](../../docs/owasp-cwe-cve-reference.md)).
- Seções relevantes do núcleo.
