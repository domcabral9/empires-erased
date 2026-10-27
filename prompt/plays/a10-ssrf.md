# Play: Server-Side Request Forgery (A10:2021)

Aprofundamento de SSRF: a aplicação faz uma requisição para um destino que o atacante influencia. Altitude
de metodologia; payload autoral passa pela seção 8 do [núcleo](../core-prompt.md). Esta categoria tem forte
relação com o gatilho de parada da seção 10.

## Quando rodar

A triagem sinalizou um parâmetro que faz o servidor buscar um recurso (URL, webhook, importador, pré-visualização,
callback) cujo destino vem, total ou parcialmente, de entrada do cliente.

## Pré-requisitos

- Aprofundamento autorizado para esta categoria.
- Um ponto em que o servidor faça requisição a um destino influenciável, em escopo.
- Consciência aguda de escopo: SSRF naturalmente tende a alcançar hosts internos ou de terceiros, que
  costumam estar fora de escopo.

## Objetivo

Confirmar que o servidor pode ser induzido a requisitar um destino controlado, e caracterizar o alcance, sem
usar o SSRF para alcançar de fato um sistema fora de escopo.

## Sinais de triagem

- Parâmetro que recebe URL ou host e provoca uma requisição de saída do servidor.
- Diferença de comportamento ou de tempo conforme o destino fornecido.
- Funcionalidade de webhook, importação por URL, pré-visualização de link, ou callback configurável.

## Procedimento de aprofundamento

1. **Confirme o controle do destino, para um alvo autorizado**: demonstre que o servidor faz a requisição
   para um destino que você controla e que está dentro do escopo (por exemplo, um coletor seu explicitamente
   autorizado nas regras de engajamento). A confirmação é o servidor alcançar esse destino, não alcançar
   algo interno.
2. **Caracterize o alcance por raciocínio, não por intrusão**: determine o que o SSRF poderia alcançar
   (rede interna, serviço de metadados, host de terceiro) a partir do comportamento observado, sem de fato
   enviar a requisição a esses destinos.
3. **Gatilho de parada da seção 10**: se a prova de conceito for capaz de alcançar um host ou sistema fora
   do escopo declarado, pare e peça orientação ao operador antes de prosseguir, mesmo que seja tecnicamente
   alcançável. Alcançar um destino fora de escopo é ampliação de escopo, proibida pela seção 0.
4. **Payload sob a seção 8**: a requisição forjada é payload autoral; declare, confirme e salve conforme a
   seção 8.
5. **Pare na prova de conceito**: confirmado o controle do destino autorizado e caracterizado o alcance,
   registre e pare.

## Evidência a coletar

O parâmetro vulnerável, a confirmação de que o servidor requisitou o destino autorizado, a caracterização do
alcance (por raciocínio), e um hash. Nunca dado obtido de um sistema fora de escopo (ver seção 0 e
log-schema).

## Severidade

Complementando a seção 7 do núcleo: pondere o que o SSRF alcança de fato (um destino externo arbitrário já é
sério; alcance a serviço de metadados ou rede interna é crítico), e se há mitigação de saída. A severidade é
estimada pelo alcance caracterizado, nunca comprovada alcançando o destino fora de escopo.

## Encerramento

Os payloads autorais salvos entram na sanitização da seção 8 do núcleo.

## Referências

- OWASP A10:2021. CWE usual: CWE-918 (SSRF). Ver
  [owasp-cwe-cve-reference.md](../../docs/owasp-cwe-cve-reference.md).
- Seções 0 (não ampliar escopo), 7 (severidade), 8 (payload), 10 (parada obrigatória) do núcleo.
