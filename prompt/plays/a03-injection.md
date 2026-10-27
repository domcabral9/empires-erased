# Play: Injection (A03:2021)

Aprofundamento de injeção: casos em que entrada controlada pelo atacante é interpretada como comando ou
código por um interpretador a jusante (SQL, NoSQL, OS, LDAP, expressão, ou renderização que leva a XSS).
Altitude de metodologia; payload autoral passa pela seção 8 do [núcleo](../core-prompt.md).

## Quando rodar

A triagem sinalizou que uma entrada chega a um interpretador sem neutralização aparente: um valor refletido
sem encoding na resposta, uma mensagem de erro que revela sintaxe de banco ou de shell, ou um campo que
muda o comportamento do backend conforme caracteres de controle.

## Pré-requisitos

- Aprofundamento autorizado para esta categoria.
- Pelo menos um ponto de entrada de dado em escopo (parâmetro de query, campo de formulário, cabeçalho,
  corpo de requisição) que alcança um interpretador.
- Clareza sobre o que é destrutivo: qualquer injeção que possa alterar ou apagar dado real exige
  autorização explícita da ação (seção 0 e regras de engajamento) antes de ir além de uma prova de
  leitura inócua.

## Objetivo

Confirmar que a entrada é interpretada, e caracterizar o sink (que interpretador, que efeito) com a prova
de conceito menos invasiva que demonstre o ponto, sem extrair dado real em massa nem executar ação
destrutiva.

## Sinais de triagem

- Valor de entrada refletido na resposta sem encoding (indício de XSS refletido).
- Mensagem de erro expondo fragmento de query, stack de banco, ou saída de shell.
- Diferença de comportamento ou de tempo de resposta conforme caracteres de controle (aspas, ponto e
  vírgula, operadores) no campo.

## Procedimento de aprofundamento

1. **Identifique o sink e o contexto**: para cada entrada suspeita, determine a que interpretador ela
   chega e em que contexto é colocada (corpo HTML, atributo, string SQL, argumento de comando). O contexto
   define o que seria uma prova de conceito válida e mínima.
2. **Prova mínima e não destrutiva primeiro**: comece pela demonstração mais inócua que evidencia a
   interpretação (ex.: um valor que altera a resposta de forma observável e reversível, sem escrever
   estado nem ler dado de terceiro). O payload construído para isso é autoral: declare, confirme e salve
   conforme a seção 8 do núcleo antes de enviar.
3. **Caracterize o alcance sem explorar além do necessário**: determine se o sink permitiria leitura,
   escrita, ou execução, por raciocínio sobre o contexto e a resposta observada, não executando a versão
   destrutiva. Para injeção que só confirma por tempo ou por diferença booleana, respeite o pacing da
   seção 5 (não repita a mesma sonda dentro da janela de cooldown só para reconfirmar).
4. **XSS**: se o contexto for renderização no navegador, caracterize se é refletido, armazenado ou via
   DOM, e qual contexto de saída falha na neutralização. A prova de conceito é a execução observável mais
   simples, nunca um payload que exfiltre sessão real de um usuário de verdade.
5. **Pare na prova de conceito**: confirmada a interpretação e caracterizado o alcance, registre a
   evidência e pare. Não encadeie para extração de dado real nem para ação destrutiva sem autorização
   explícita daquela ação.

## Evidência a coletar

Para cada confirmação: o ponto de entrada, o sink/interpretador, o contexto, a prova de conceito usada
(referenciada por `payload_ref` no diretório de artefatos, não em texto puro no log) e um hash da evidência
bruta. Ver log-schema e a seção 8 do núcleo.

## Severidade

Complementando a seção 7 do núcleo: pondere exposição (o sink é alcançável sem autenticação), o efeito do
sink (leitura de dado, escrita de estado, execução de comando, ou execução de script no navegador de uma
vítima), e o privilégio do processo a jusante. Execução de código ou de comando pesa mais do que um reflexo
sem efeito de estado.

## Encerramento

Os payloads autorais salvos durante esta play entram na sanitização da seção 8 do núcleo ao final do
engajamento.

## Referências

- OWASP A03:2021. CWEs usuais: CWE-89 (SQL Injection), CWE-79 (XSS), CWE-78 (OS Command Injection),
  CWE-943 (Improper Neutralization em lógica de query). Ver
  [owasp-cwe-cve-reference.md](../../docs/owasp-cwe-cve-reference.md).
- Seções 0 (limites), 5 (pacing), 7 (severidade), 8 (payload) do núcleo.
