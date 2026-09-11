# Esquema de log

Todo teste ativo executado a partir do prompt central grava uma linha de log estruturada, em formato
JSON Lines (um objeto JSON por linha), no diretório validado durante o [gate de pré-engajamento](../prompt/core-prompt.md#1-gate-de-pré-engajamento-regras-de-engajamento).

## Campos

| Campo | Tipo | Descrição |
|---|---|---|
| `timestamp` | string (ISO 8601) | Momento da ação, com fuso horário |
| `phase` | string | Fase da metodologia no momento da ação (ver [methodology.md](methodology.md)) |
| `owasp_category` | string ou null | Categoria OWASP Top 10 relacionada, quando aplicável |
| `action` | string | Descrição curta e objetiva da ação executada |
| `target` | string | Host/rota/parâmetro alvo da ação (nunca um dado sensível em si) |
| `access_model` | string | `blackbox`, `graybox` ou `whitebox`, herdado das regras de engajamento |
| `custom_payload` | boolean | `true` se a ação usou um payload gerado pelo próprio modelo, não vindo de referência conhecida |
| `payload_ref` | string ou null | Caminho do artefato no diretório isolado, quando `custom_payload` é `true` |
| `result_summary` | string | Resumo do resultado, sem dado sensível bruto |
| `evidence_hash` | string ou null | Hash (SHA-256) da evidência bruta, quando houver, para referência sem expor o conteúdo no log |
| `operator_confirmed` | boolean | `true` se a ação exigiu e recebeu confirmação explícita do operador antes de rodar |

## Exemplo

```json
{"timestamp":"2026-09-11T14:32:07-03:00","phase":"triagem-owasp","owasp_category":"A03:2021","action":"teste de injeção refletida em parâmetro de busca","target":"https://target.example/search?q=","access_model":"blackbox","custom_payload":false,"payload_ref":null,"result_summary":"payload refletido sem encoding, indício de XSS refletido","evidence_hash":"9f2c...a41d","operator_confirmed":true}
```

## Geração segura de linhas JSONL

Nunca construa uma linha de log concatenando string à mão (ex.: um heredoc de shell escapando aspas
manualmente). Sempre serialize cada registro com um serializador JSON real disponível no ambiente do
operador (`JSON.stringify` em JavaScript/Node, `json.dumps` em Python, etc.) e escreva o resultado no
arquivo. O gatilho mais comum de JSONL inválido é caminho de arquivo no Windows (barra invertida) em
campos como `payload_ref`, `action` ou `target` - ou serialize o valor programaticamente, ou normalize
para barra normal (`/`) antes de escrever.

Exemplo ruim (escaping manual, quebra com barra invertida não escapada):

```
echo '{"payload_ref":"D:\Artifacts\run\evidence.md"}' >> log.jsonl
```

Exemplo correto (serializador real produz o escaping certo sozinho):

```js
const fs = require("fs");
const entry = { payload_ref: "D:\\Artifacts\\run\\evidence.md" /* ...demais campos */ };
fs.appendFileSync("log.jsonl", JSON.stringify(entry) + "\n");
```

Depois de escrever, valide o arquivo inteiro (`JSON.parse` linha a linha, ou equivalente) antes de
seguir para a próxima fase - uma linha inválida não detectada compromete a rastreabilidade de todo o
engajamento.

Um bloqueio por rate limit encontrado durante o teste (ver a orientação de pacing no
[prompt central](../prompt/core-prompt.md)) também vira uma linha de log normal, com
`result_summary` descrevendo o bloqueio - não é só tratado operacionalmente sem deixar rastro.

## Regras de redação

- Nunca gravar credencial, token de sessão, ou dado pessoal identificável em texto puro em nenhum
  campo. Quando a evidência contém esse tipo de dado, gravar apenas o hash e uma descrição do que foi
  observado, nunca o valor bruto.
- Se o próprio ato de logar um valor completo for necessário para reprodutibilidade (ex.: um payload
  específico), o valor vai para o diretório isolado de artefatos, referenciado por `payload_ref`, nunca
  diretamente na linha de log.
- Um log incompleto (campo faltando porque a informação genuinamente não existe naquele momento, como
  `payload_ref` numa ação sem payload autoral) é aceitável. Um log com dado sensível redigido de forma
  incorreta não é.
