# Play: Insecure Design (A04:2021)

Aprofundamento de design inseguro: a falha está na ausência de um controle no desenho do fluxo, não em um
bug de implementação. Altitude de metodologia; payload autoral passa pela seção 8 do
[núcleo](../core-prompt.md).

## Quando rodar

A triagem sinalizou um fluxo de negócio que confia demais no cliente, um limite de abuso ausente (um fluxo
caro ou sensível sem throttling ou sem etapa de verificação), ou uma sequência que pode ser pulada ou
executada fora de ordem.

## Pré-requisitos

- Aprofundamento autorizado para esta categoria.
- Entendimento mínimo da regra de negócio do fluxo (das regras de engajamento ou do reconhecimento), já que
  design inseguro só se caracteriza contra a intenção do fluxo.

## Objetivo

Confirmar que o desenho de um fluxo permite abuso que o controle correto preveniria, e caracterizar o abuso
com a prova de conceito mínima, sem explorar em escala.

## Sinais de triagem

- Decisão de segurança tomada no cliente e confiada pelo servidor sem revalidação.
- Fluxo caro ou sensível (redefinição de senha, geração de recurso, compra) sem limite de taxa ou sem etapa
  de confirmação.
- Etapas de um fluxo multi-passo alcançáveis fora de ordem, ou puláveis.

## Procedimento de aprofundamento

1. **Reconstrua o fluxo pretendido**: liste as etapas e os controles que a regra de negócio pressupõe. Esse
   é o gabarito.
2. **Teste a confiança indevida**: para cada decisão que deveria ser do servidor, verifique se o servidor
   revalida ou se aceita o que o cliente afirma. O achado é a aceitação sem revalidação.
3. **Teste limites de abuso**: caracterize se um fluxo caro ou sensível carece de throttling ou de etapa de
   verificação, respeitando o pacing da seção 5 (não dispare volume só para confirmar, uma amostra que
   demonstra a ausência do limite basta).
4. **Teste ordem e salto de etapa**: verifique se uma etapa pode ser alcançada sem as anteriores.
5. **Pare na prova de conceito**: demonstrado o abuso de desenho, registre e pare. Não execute o abuso em
   escala nem contra dado real além do mínimo.

## Evidência a coletar

O fluxo, o controle ausente, a sequência que demonstra o abuso, e um hash da evidência. Descrição do efeito,
nunca dado real em claro (ver log-schema).

## Severidade

Complementando a seção 7 do núcleo: pondere o que o abuso permite (fraude, bypass de verificação, consumo de
recurso), a exposição do fluxo, e se outro controle compensa na prática. Design inseguro costuma ter CVE
associado só quando vira um caso concreto; muitas vezes o relatório registra "sem CVE público verificado".

## Encerramento

Artefatos de prova salvos entram na sanitização da seção 8 do núcleo.

## Referências

- OWASP A04:2021. CWEs usuais: CWE-209 (vazamento via mensagem de erro), CWE-501 (violação de fronteira de
  confiança), CWE-840 (falha de lógica de negócio). Ver
  [owasp-cwe-cve-reference.md](../../docs/owasp-cwe-cve-reference.md).
- Seções 0 (limites), 5 (pacing), 7 (severidade) do núcleo.
