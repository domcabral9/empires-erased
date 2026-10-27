# Play: Identification and Authentication Failures (A07:2021)

Aprofundamento de falhas de identificação e autenticação: autenticação contornável, sessão mal gerida, ou
vazamento que ajuda um atacante a identificar alvos. Altitude de metodologia; payload autoral passa pela
seção 8 do [núcleo](../core-prompt.md).

## Quando rodar

A triagem sinalizou mensagem de erro que diferencia existência de conta/organização, sessão que não expira
quando deveria, política de credencial fraca, ou um fluxo de autenticação que parece contornável.

## Pré-requisitos

- Aprofundamento autorizado para esta categoria.
- Um fluxo de autenticação ou gestão de sessão em escopo.
- Força bruta de credencial real é proibida salvo autorização explícita daquela ação (seção 0): esta play
  caracteriza a fraqueza, não a explora por força bruta.

## Objetivo

Confirmar uma falha de autenticação ou de sessão e caracterizar o impacto, sem adivinhar credencial real em
volume nem comprometer conta de verdade.

## Sinais de triagem

- Mensagem de erro de login diferente para conta/organização inexistente versus credencial incorreta
  (enumeração por discrepância observável).
- Token de sessão que continua válido após logout ou após troca de senha.
- Ausência de política mínima de credencial, ou de limite de tentativas.
- Fluxo de recuperação ou de segundo fator aparentemente contornável.

## Procedimento de aprofundamento

1. **Enumeração por discrepância**: compare de forma sistemática as respostas para entradas que existem
   versus inventadas, caracterizando se o sistema revela existência sem credencial. Variar o identificador
   em texto puro não é payload autoral, mas cada tentativa é registrada e o pacing da seção 5 vale (não
   repetir em volume dentro da janela). Contraste com um fluxo irmão que trate corretamente, se houver, para
   mostrar o padrão certo já existente.
2. **Ciclo de vida de sessão**: caracterize se o token sobrevive a logout, a troca de senha, ou a expiração
   esperada. O achado é a persistência indevida, demonstrável sem comprometer conta de terceiro.
3. **Política de credencial e de tentativas**: caracterize a ausência de limite ou de política mínima com
   uma amostra que demonstra, nunca com força bruta real.
4. **Pare na caracterização**: confirmada a falha, registre e pare. Não enumere em massa nem faça força
   bruta de credencial real.

## Evidência a coletar

A falha, a discrepância ou comportamento observado, o contraste com o fluxo correto quando houver, e um
hash. Nunca credencial ou token real em claro no log (ver log-schema e seção 4).

## Severidade

Complementando a seção 7 do núcleo: enumeração de conta isolada costuma ser baixa (informação de
reconhecimento, mitigada parcialmente por rate limit); sessão que não expira após troca de senha, ou bypass
de autenticação, pesa bem mais. Pondere exposição e o que a falha permite de concreto.

## Encerramento

Artefatos de prova salvos entram na sanitização da seção 8 do núcleo.

## Referências

- OWASP A07:2021. CWEs usuais: CWE-287 (autenticação imprópria), CWE-384 (fixação de sessão), CWE-306
  (função crítica sem autenticação), CWE-203 (discrepância observável, enumeração), CWE-613 (expiração de
  sessão insuficiente). Ver [owasp-cwe-cve-reference.md](../../docs/owasp-cwe-cve-reference.md).
- Seções 0 (limites), 4 (log sem segredo), 5 (pacing), 7 (severidade) do núcleo.
