# Play: Security Logging and Monitoring Failures (A09:2021)

Aprofundamento de falhas de log e monitoramento: eventos de segurança que não são registrados, ou log que
pode ser corrompido por entrada do atacante. Altitude de metodologia; qualquer payload autoral passa pela
seção 8 do [núcleo](../core-prompt.md).

## Quando rodar

A triagem sinalizou que eventos de segurança relevantes parecem não gerar registro observável, ou que
entrada controlada pelo atacante chega ao log sem neutralização.

## Pré-requisitos

- Aprofundamento autorizado para esta categoria.
- Alguma forma de observar o efeito de log, mesmo indireta (resposta do sistema, alerta, comportamento),
  reconhecendo que no modo blackbox essa visibilidade costuma ser limitada e o achado pode ficar como
  indício, não confirmação.

## Objetivo

Caracterizar se eventos de segurança relevantes deixam de ser registrados ou se o log é corrompível, sem
gerar volume de eventos que polua o ambiente do operador.

## Sinais de triagem

- Ação sensível (falha de login repetida, acesso negado, mudança de privilégio) sem efeito observável de
  registro ou alerta.
- Entrada controlada pelo atacante refletida em um log sem neutralização (risco de injeção de log).

## Procedimento de aprofundamento

1. **Cobertura**: para um conjunto pequeno de eventos de segurança relevantes, caracterize se há sinal de
   que o sistema os registra ou reage. No blackbox, deixe explícito o que é indício versus confirmação, já
   que a visibilidade do log costuma ser indireta.
2. **Injeção de log**: se entrada do atacante chega ao log, caracterize se ela pode forjar ou corromper
   entradas (quebra de linha, caractere de controle). A prova é payload autoral sob a seção 8, e mínima.
3. **Não gere ruído**: esta categoria tenta demonstrar ausência de registro, o que tenta por volume. Use a
   menor amostra que evidencia o ponto; não inunde o alvo com eventos (seção 5, pacing, e seção 0).
4. **Pare na caracterização**: registre o indício ou a confirmação e pare.

## Evidência a coletar

O evento testado, o sinal (ou a ausência dele) de registro, se corrupção de log foi demonstrada, e um hash.
Deixe claro o nível de certeza. Ver log-schema.

## Severidade

Complementando a seção 7 do núcleo: falha de log raramente é explorável sozinha; o peso dela é agravar o
impacto de outros achados (um ataque que não deixa rastro). Costuma ser baixa isolada, mas registre quando
amplifica um achado maior. Muitas vezes o relatório anota "sem CVE público verificado".

## Encerramento

Qualquer artefato gerado entra na sanitização da seção 8 do núcleo.

## Referências

- OWASP A09:2021. CWEs usuais: CWE-778 (log insuficiente), CWE-117 (neutralização inadequada de saída de
  log), CWE-223 (omissão de informação relevante). Ver
  [owasp-cwe-cve-reference.md](../../docs/owasp-cwe-cve-reference.md).
- Seções 0 (limites), 5 (pacing), 7 (severidade), 8 (payload) do núcleo.
