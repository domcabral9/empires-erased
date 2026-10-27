# Plays

Uma "play" é o procedimento de aprofundamento de uma categoria do OWASP Top 10 (ou de uma tarefa bem
definida). O [prompt central](../core-prompt.md) é o núcleo sempre-carregado; as plays ficam aqui e são
carregadas sob demanda, uma de cada vez, só quando a triagem (fase 3) sinaliza aquela categoria e o
operador autoriza o aprofundamento (ver seção 12 do núcleo).

O objetivo dessa separação é de custo: categoria sem indício de problema nunca tem sua play carregada,
então não gasta token de input. O detalhe de aprofundamento pode crescer sem inflar o contexto que é
carregado em todo engajamento.

## Como usar

1. Rode as fases 1 a 3 guiado só pelo núcleo e pela [tabela de referência](../../docs/owasp-cwe-cve-reference.md).
2. Ao entrar no aprofundamento (fase 4), para cada categoria sinalizada e autorizada, abra a play
   correspondente abaixo, siga o procedimento, e só então passe para a próxima.
3. Se a categoria sinalizada não tiver play dedicada ainda, use o fallback da seção 12 do núcleo.

## Altitude e limites

As plays descrevem metodologia (o que procurar, como caracterizar, que evidência coletar), no mesmo
nível do OWASP Web Security Testing Guide. Elas não trazem payload pronto para colar: qualquer payload
autoral é construído em runtime e passa pelo ciclo de vida da seção 8 do núcleo (declarar ao operador,
confirmar, salvar, sanitizar). Uma play nunca autoriza pular esse gate nem ampliar o escopo definido
nas regras de engajamento.

## Índice de plays

| Categoria OWASP Top 10:2021 | Play | Estado |
|---|---|---|
| A01: Broken Access Control | [a01-broken-access-control.md](a01-broken-access-control.md) | Disponível |
| A02: Cryptographic Failures | [a02-cryptographic-failures.md](a02-cryptographic-failures.md) | Disponível |
| A03: Injection | [a03-injection.md](a03-injection.md) | Disponível |
| A04: Insecure Design | [a04-insecure-design.md](a04-insecure-design.md) | Disponível |
| A05: Security Misconfiguration | [a05-security-misconfiguration.md](a05-security-misconfiguration.md) | Disponível |
| A06: Vulnerable and Outdated Components | [a06-vulnerable-components.md](a06-vulnerable-components.md) | Disponível |
| A07: Identification and Authentication Failures | [a07-auth-failures.md](a07-auth-failures.md) | Disponível |
| A08: Software and Data Integrity Failures | [a08-integrity-failures.md](a08-integrity-failures.md) | Disponível |
| A09: Security Logging and Monitoring Failures | [a09-logging-failures.md](a09-logging-failures.md) | Disponível |
| A10: Server-Side Request Forgery (SSRF) | [a10-ssrf.md](a10-ssrf.md) | Disponível |

Antes de usar este índice num engajamento real, confirme em owasp.org se já há uma revisão do Top 10
mais recente que a de 2021 (mesma ressalva da tabela de referência).

## Anatomia de uma play

Toda play segue o esqueleto de [`_template.md`](_template.md): Quando rodar, Pré-requisitos, Objetivo,
Sinais de triagem, Procedimento de aprofundamento, Evidência a coletar, Severidade, Encerramento, e
Referências.

## Atribuição

O conceito de play modular, a ideia de condições de disparo por procedimento e o formato de achado que
o projeto adota na Fase 5 são inspirados no
[OWASP Secure Agent Playbook](https://github.com/OWASP/secure-agent-playbook) (licença CC-BY-4.0). A
estrutura foi adaptada; o conteúdo das plays deste repositório foi escrito do zero para o contexto de
teste de segurança autorizado ao vivo, que difere do foco de revisão de código daquele projeto. Nenhum
empacotamento específico de ferramenta foi adotado: as plays são texto agnóstico de LLM.
