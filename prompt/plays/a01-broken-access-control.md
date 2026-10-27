# Play: Broken Access Control (A01:2021)

Aprofundamento de controle de acesso quebrado: casos em que um usuário consegue agir fora do que o papel
dele deveria permitir. Altitude de metodologia; payload autoral, quando necessário, passa pela seção 8
do [núcleo](../core-prompt.md).

## Quando rodar

A triagem sinalizou pelo menos um indício de que a autorização não é consistente: uma rota que responde
diferente com e sem sessão, um identificador de objeto que parece sequencial ou adivinhável, uma função
administrativa alcançável sem o papel correspondente, ou uma diferença de comportamento entre papéis que
não corresponde à regra de negócio documentada.

## Pré-requisitos

- Aprofundamento autorizado para esta categoria.
- Pelo menos um papel de autenticação disponível na sessão (anônimo, usuário comum, ou admin), conforme
  a seção 3 do núcleo. A caracterização de A01 depende de comparar o que cada papel consegue fazer.
- Rotas ou objetos conhecidos (das regras de engajamento no modo graybox/whitebox, ou do reconhecimento
  no modo blackbox).

## Objetivo

Confirmar se um ator consegue ler ou alterar um recurso, ou executar uma ação, para a qual o papel dele
não deveria ter permissão, e caracterizar o impacto sem ir além da prova de conceito mínima.

## Sinais de triagem

- Resposta de uma rota de negócio muda conforme a presença ou ausência de token, mas sem retornar 401/403
  onde deveria.
- Identificadores de objeto em URLs ou corpos de requisição que parecem sequenciais, previsíveis ou
  enumeráveis.
- Função sensível (exportar, aprovar, administrar) referenciada no frontend ou na documentação sem um
  gate de papel aparente.
- Diferença de comportamento entre dois papéis que não bate com a regra de negócio.

## Procedimento de aprofundamento

1. **Mapeie a matriz de acesso esperada**: para cada rota ou ação em escopo, registre qual papel deveria
   poder acessá-la, segundo a regra de negócio das regras de engajamento. Essa matriz é o gabarito contra
   o qual todo teste é comparado.
2. **Acesso vertical**: a partir de um papel de menor privilégio (ou anônimo), tente alcançar uma função
   que a matriz reserva a um papel maior. O que importa é a resposta de autorização, não o conteúdo: um
   200 onde a matriz esperava 401/403 é o achado.
3. **Acesso horizontal (IDOR)**: com um papel legítimo, tente alcançar um recurso de outro titular do
   mesmo papel, variando o identificador de objeto. Caracterize se o sistema valida a propriedade do
   recurso contra a sessão, ou só a existência do identificador. A variação de identificador em texto puro
   não é payload autoral (ver seção 8 do núcleo), mas cada tentativa é registrada no log.
4. **Métodos e caminhos alternativos**: verifique se um gate presente em um método ou caminho está ausente
   em outro para o mesmo recurso (ex.: leitura bloqueada mas escrita não, ou uma rota versionada antiga
   sem o gate).
5. **Pare na prova de conceito**: ao confirmar o acesso indevido a um recurso, registre a evidência e pare.
   Não enumere em massa nem extraia dado real além do mínimo para provar o ponto (limite da seção 0 e das
   regras de engajamento).

## Evidência a coletar

Para cada confirmação: a rota/objeto, o papel usado, a resposta de autorização observada versus a esperada
pela matriz, e um hash da evidência bruta. Nunca grave o dado de outro titular em texto puro no log; grave
a descrição e o hash (ver log-schema).

## Severidade

Pondere, complementando a seção 7 do núcleo: exposição (o acesso indevido exige autenticação ou é
alcançável anônimo), o que está em risco (leitura de dado de terceiro, escrita/alteração de estado,
execução de ação privilegiada), e se há controle compensatório que limite o alcance prático. Acesso de
escrita ou a ação administrativa pesa mais do que leitura de metadado.

## Encerramento

Se algum artefato de prova foi salvo, ele entra na sanitização da seção 8 do núcleo.

## Referências

- OWASP A01:2021. CWEs usuais: CWE-284 (Improper Access Control), CWE-639 (IDOR), CWE-22 (Path Traversal),
  CWE-285 (Improper Authorization). Ver [owasp-cwe-cve-reference.md](../../docs/owasp-cwe-cve-reference.md).
- Seções 0 (limites), 3 (papel de autenticação), 7 (severidade), 8 (payload) do núcleo.
