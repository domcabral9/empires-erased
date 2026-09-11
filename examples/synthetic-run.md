# Execução sintética de exemplo

Este documento é inteiramente fictício. O alvo, os achados e os trechos de log abaixo existem só para
mostrar o formato esperado de saída do prompt central em cada fase. Nenhum dado aqui corresponde a um
sistema real.

Este exemplo é ilustrativo, não um registro de execução real - foi escrito antes de o prompt ser
executado de verdade contra qualquer alvo. Uma execução real pode revelar fricção operacional que um
exemplo escrito à mão não antecipa (ex.: encoding de log, pacing de rate limit, decisões de escopo por
julgamento do operador) - várias das seções do prompt central e dos outros documentos deste repositório
já refletem lições aprendidas de execuções reais, mesmo que este exemplo em si continue fictício.

## Regras de engajamento (preenchidas para este exemplo)

- **Alvo**: `target.example`, aplicação web de gestão de pedidos
- **Referência de autorização**: "autorização verbal do dono do sistema, confirmada por e-mail em
  2026-09-01"
- **Escopo**: `app.target.example` e `api.target.example`; fora de escopo: qualquer subdomínio de
  terceiro linkado na aplicação
- **Janela**: 2026-09-11 09:00 a 2026-09-12 18:00, America/Sao_Paulo
- **Classificação**: graybox, com uma credencial de usuário comum fornecida
- **Diretório de log**: `./logs/2026-09-11-target-example/`
- **Diretório de artefatos**: `./artifacts/2026-09-11-target-example/`

## Fase 1: reconhecimento passivo (resumo)

Cabeçalhos HTTP indicam Nginx como proxy reverso e um framework backend em Node.js. Nenhum header de
WAF conhecido presente nas respostas iniciais.

## Fase 2: fingerprint de controles compensatórios (resumo)

Três requisições consecutivas com payload de teste inócuo (`' OR '1'='1` em um campo de busca, sem
intenção de exploração real) não geraram bloqueio nem resposta de challenge. Nenhum indício de WAF
ativo. Registrado no relatório como "ausência de controle compensatório identificável, aumentar
cautela nas fases seguintes em vez de assumir ambiente protegido por padrão".

## Fase 3: triagem OWASP (trecho)

| Categoria | Resultado da triagem | Avança para deep-dive? |
|---|---|---|
| A03: Injection | Reflexo de payload sem encoding no parâmetro `q` de busca | Sim |
| A07: Auth Failures | Sessão não expira após troca de senha | Sim |
| A05: Security Misconfiguration | Nenhum indício na triagem leve | Não |

## Fase 4: aprofundamento (trecho, após confirmação do operador)

Achado confirmado em A03: XSS refletido em `GET /search?q=`. Payload usado veio de uma lista de
referência conhecida (não autoral), portanto sem gate de confirmação adicional por payload individual.

Achado em A07 investigado com um payload autoral (script de verificação de reuso de token de sessão
após troca de senha). Como não vem de referência conhecida, o modelo pausou e pediu confirmação
explícita ao operador antes de executar, registrando o motivo, o efeito esperado e onde o artefato
seria salvo.

## Linha de log correspondente

```json
{"timestamp":"2026-09-11T14:32:07-03:00","phase":"deep-dive","owasp_category":"A03:2021","action":"confirmacao de XSS refletido com payload de referencia conhecida","target":"https://api.target.example/search?q=","access_model":"graybox","custom_payload":false,"payload_ref":null,"result_summary":"payload refletido sem encoding no corpo da resposta","evidence_hash":"3ab1...e09f","operator_confirmed":true}
```

## Relatório final (trecho)

| Achado | OWASP | CWE | Severidade estimada | Evidência |
|---|---|---|---|---|
| XSS refletido em `/search` | A03:2021 | CWE-79 | Alta (sem autenticação, execução de script no navegador da vítima) | log `3ab1...e09f` |
| Token de sessão válido após troca de senha | A07:2021 | CWE-613 | Média (exige sessão prévia comprometida para ter efeito) | log e artefato em `./artifacts/2026-09-11-target-example/` |

## Sanitização de artefatos (encerramento)

Ao final da janela de teste, o modelo lista os artefatos gerados no diretório isolado, propõe o
comando de limpeza adequado ao sistema operacional detectado na sessão, e pede confirmação explícita
do operador antes de indicar a remoção como concluída no relatório.
