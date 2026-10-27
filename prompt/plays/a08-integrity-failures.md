# Play: Software and Data Integrity Failures (A08:2021)

Aprofundamento de falhas de integridade de software e dado: confiar em dado ou código de origem não
verificada, incluindo desserialização insegura e recurso trazido sem verificação. Altitude de metodologia;
payload autoral passa pela seção 8 do [núcleo](../core-prompt.md).

## Quando rodar

A triagem sinalizou um fluxo que aceita objeto serializado do cliente, um recurso trazido de origem externa
sem verificação de integridade, ou um mecanismo de atualização que não valida a procedência.

## Pré-requisitos

- Aprofundamento autorizado para esta categoria.
- Um ponto que aceite dado estruturado/serializado ou que incorpore recurso externo, em escopo.
- Desserialização costuma levar a execução de código: qualquer prova além da caracterização é ação de maior
  risco sob a seção 0 e exige autorização explícita.

## Objetivo

Confirmar que o sistema confia em dado ou código sem verificar integridade, e caracterizar o impacto com a
prova menos invasiva, sem detonar execução de código.

## Sinais de triagem

- Campo que carrega objeto serializado (assinatura de formato reconhecível) vindo do cliente.
- Recurso (script, pacote, template) incorporado de origem externa sem verificação de integridade.
- Mecanismo de atualização ou de import que não valida procedência.

## Procedimento de aprofundamento

1. **Mapeie o limite de confiança**: identifique onde dado ou código de origem não confiável cruza para um
   contexto confiável, e o que o sistema faz para verificá-lo (assinatura, hash, esquema).
2. **Desserialização, prova mínima**: caracterize se a entrada serializada é desserializada sem validação de
   tipo/esquema. Comece pela prova mais inócua que evidencia o processamento (um objeto bem formado que
   altera a resposta de modo observável e reversível), construída como payload autoral sob a seção 8.
3. **Não detone execução**: se a caracterização indicar caminho para execução de código, o achado para na
   demonstração do processamento indevido. Executar código é ação de maior risco, só com autorização
   explícita daquela ação.
4. **Integridade de recurso externo**: caracterize se um recurso incorporado poderia ser substituído na
   origem sem o sistema perceber, por raciocínio sobre a ausência de verificação, não comprometendo a origem
   real.
5. **Pare na prova de conceito**: registre e pare.

## Evidência a coletar

O limite de confiança, a ausência de verificação, a prova de conceito (referenciada por `payload_ref`, não
em claro no log), e um hash. Ver log-schema e seção 8.

## Severidade

Complementando a seção 7 do núcleo: desserialização que leva a execução de código é das mais severas;
integridade de recurso ausente varia com o que o recurso controla. Pondere exposição e o efeito concreto
demonstrado, não o pior caso teórico.

## Encerramento

Os payloads autorais salvos entram na sanitização da seção 8 do núcleo.

## Referências

- OWASP A08:2021. CWEs usuais: CWE-502 (desserialização insegura), CWE-829 (inclusão de recurso de origem
  não confiável), CWE-345 (verificação de autenticidade insuficiente). Ver
  [owasp-cwe-cve-reference.md](../../docs/owasp-cwe-cve-reference.md).
- Seções 0 (limites), 7 (severidade), 8 (payload) do núcleo.
