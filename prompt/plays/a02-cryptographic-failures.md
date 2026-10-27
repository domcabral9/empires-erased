# Play: Cryptographic Failures (A02:2021)

Aprofundamento de falhas de criptografia: dado sensível exposto em trânsito ou em repouso por ausência de
cifra, cifra fraca, ou uso incorreto. Altitude de metodologia; qualquer payload autoral passa pela seção 8
do [núcleo](../core-prompt.md).

## Quando rodar

A triagem sinalizou dado sensível trafegando sem TLS, um cabeçalho de transporte ausente ou permissivo, um
algoritmo ou protocolo obsoleto negociável, ou um valor que parece sensível retornado sem proteção.

## Pré-requisitos

- Aprofundamento autorizado para esta categoria.
- Pelo menos um fluxo que transporte ou armazene dado sensível em escopo (autenticação, dado pessoal,
  token, segredo).
- A caracterização é passiva ou de baixo ruído: inspeção de transporte e de resposta, não quebra de cifra.

## Objetivo

Confirmar que um dado sensível específico está exposto por falha de criptografia e caracterizar a exposição,
sem tentar quebrar o algoritmo nem extrair o dado real além do mínimo que prova o ponto.

## Sinais de triagem

- Fluxo sensível servido em HTTP puro, ou redirecionamento para HTTPS que ainda expõe a primeira requisição.
- Ausência de HSTS, ou cookies de sessão sem os atributos de segurança adequados.
- Negociação de protocolo ou cifra legada aceita pelo servidor.
- Valor sensível (token, dado pessoal) retornado em claro onde deveria estar protegido ou ausente.

## Procedimento de aprofundamento

1. **Transporte**: caracterize se o fluxo sensível exige transporte cifrado de ponta a ponta, ou se há uma
   janela em claro (primeira requisição, recurso misto, canal lateral). Observe os cabeçalhos de transporte
   e os atributos de cookie contra o que a sensibilidade do dado exige.
2. **Protocolo e cifra**: determine, pela negociação oferecida, se o servidor aceita protocolo ou suíte
   considerada obsoleta. O achado é a aceitação, não a exploração de uma fraqueza específica da suíte.
3. **Repouso e exposição de valor**: quando o teste alcançar dado em repouso ou retornado pela aplicação,
   caracterize se um dado sensível aparece sem proteção. Não colete o dado real além do mínimo; registre a
   natureza do dado e um hash, nunca o valor.
4. **Pare na caracterização**: confirmada a exposição, registre e pare. Não tente quebrar cifra nem
   acumular dado real.

## Evidência a coletar

A natureza do dado exposto, o ponto de exposição (transporte, cabeçalho, resposta), a configuração
observada, e um hash da evidência. Nunca o dado sensível em claro no log (ver log-schema).

## Severidade

Complementando a seção 7 do núcleo: pondere a sensibilidade do dado exposto, se a exposição é alcançável por
um observador de rede ou exige posição privilegiada, e se há mitigação parcial. Exposição de credencial ou
dado pessoal pesa mais do que a simples ausência de um cabeçalho de hardening.

## Encerramento

Artefatos de prova salvos entram na sanitização da seção 8 do núcleo.

## Referências

- OWASP A02:2021. CWEs usuais: CWE-327 (algoritmo quebrado/arriscado), CWE-331 (entropia insuficiente),
  CWE-319 (transmissão em claro), CWE-259 (credencial hardcoded). Ver
  [owasp-cwe-cve-reference.md](../../docs/owasp-cwe-cve-reference.md).
- Seções 5 (reconhecimento de baixo ruído), 7 (severidade), 8 (payload) do núcleo.
