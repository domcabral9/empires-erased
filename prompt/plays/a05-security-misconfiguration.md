# Play: Security Misconfiguration (A05:2021)

Aprofundamento de configuração insegura: recurso exposto, default não endurecido, ou permissão excessiva que
não deveria estar acessível. Altitude de metodologia; payload autoral passa pela seção 8 do
[núcleo](../core-prompt.md).

## Quando rodar

A triagem sinalizou um endpoint de administração, diagnóstico ou documentação exposto, uma resposta que
revela stack ou versão por default, um recurso de debug ligado, ou uma permissão mais ampla que o
necessário.

## Pré-requisitos

- Aprofundamento autorizado para esta categoria.
- Clareza sobre o ambiente (produção, staging, dev) da seção 3, já que o que é aceitável em dev pode ser
  achado em produção.

## Objetivo

Confirmar que uma configuração expõe superfície ou informação que não deveria estar acessível ao papel
atual, e caracterizar o impacto, sem usar o recurso exposto para ir além.

## Sinais de triagem

- Console de administração, painel de métricas, documentação de API ou endpoint de diagnóstico alcançável
  sem autenticação.
- Resposta expondo versão de software, stack trace, ou caminho interno por configuração default.
- Recurso de debug ou modo verboso ligado.
- Cabeçalhos de segurança ausentes onde a sensibilidade exigiria.

## Procedimento de aprofundamento

1. **Confirme a exposição contra o papel esperado**: para cada recurso sinalizado, confirme que ele está
   acessível a um papel que não deveria alcançá-lo, e registre o que ele revela. A exposição é o achado,
   não o uso do recurso.
2. **Caracterize o ambiente**: distinga configuração aceitável em dev de achado real. Um endpoint de debug
   em produção é achado; o mesmo em dev é observação de higiene (registrar, não elevar).
3. **Não opere o recurso exposto**: se um console administrativo está aberto, o achado é que ele está
   aberto. Não execute ações administrativas por ele sem autorização explícita daquela ação (seção 0).
4. **Processamento de entidade externa (XXE)**: se um parser aceita entrada estruturada que referencia
   entidades externas, caracterize o comportamento com uma prova mínima e não destrutiva; a prova é payload
   autoral e passa pela seção 8. Se a referência externa puder alcançar host fora de escopo, aplique o
   gatilho de parada da seção 10.
5. **Pare na caracterização**: confirmada a exposição ou o comportamento, registre e pare.

## Evidência a coletar

O recurso ou configuração, o papel que o alcançou, o que foi revelado, o ambiente, e um hash da evidência.
Nunca dado interno sensível em claro no log (ver log-schema).

## Severidade

Complementando a seção 7 do núcleo: pondere o que a configuração expõe (informação de reconhecimento,
superfície administrativa, ou caminho direto para comprometimento), o ambiente, e a mitigação existente.
Console administrativo aberto pesa muito mais do que um cabeçalho de hardening ausente.

## Encerramento

Artefatos de prova salvos entram na sanitização da seção 8 do núcleo.

## Referências

- OWASP A05:2021. CWEs usuais: CWE-16 (configuração), CWE-611 (XXE), CWE-732 (permissão incorreta),
  CWE-1188 (default inseguro). Ver [owasp-cwe-cve-reference.md](../../docs/owasp-cwe-cve-reference.md).
- Seções 0 (limites), 3 (ambiente), 8 (payload), 10 (parada em escopo) do núcleo.
