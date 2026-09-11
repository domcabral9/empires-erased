# Metodologia faseada

O [prompt central](../prompt/core-prompt.md) organiza qualquer engajamento em fases com um gate de
confirmação entre elas. Este documento explica o raciocínio por trás disso.

## Por que fases, e não um teste único de ponta a ponta

Um teste amplo sem fases tende a dois problemas: custo de execução desproporcional ao valor
encontrado (um LLM aprofundando em todas as categorias OWASP ao mesmo tempo, inclusive nas que não
têm nenhum indício de problema), e risco operacional maior do que o necessário (testes agressivos
disparados antes de o modelo entender o ambiente que está testando). Um gate entre fases resolve os
dois: só se avança para a etapa mais cara depois de a etapa anterior indicar que vale a pena, e o
operador tem um ponto natural para interromper ou redirecionar o teste.

## As fases

### 1. Reconhecimento passivo

Coleta de informação sem gerar tráfego que um sistema de defesa classificaria como anômalo: banners,
cabeçalhos HTTP, tecnologia identificável por fingerprint público, estrutura de rotas já documentada
nas regras de engajamento. Nenhum payload é enviado nesta fase.

### 2. Fingerprint de controles compensatórios

Ainda com tráfego de baixo ruído, o modelo tenta identificar se existe WAF, CDN com proteção
embutida, rate limiting ou outro controle de borda. O resultado desta fase não é "contornar o
controle", é registrar a presença dele e ajustar a agressividade das fases seguintes de acordo.
Detalhes de como isso é feito e reportado ficam na seção correspondente do prompt central - inclusive
a orientação de pacing para não disparar o próprio rate limit repetidamente só para confirmação.

### 3. Triagem por categoria OWASP

Um teste leve e de baixo custo em cada uma das dez categorias do OWASP Top 10 relevantes ao tipo de
alvo (uma API sem frontend, por exemplo, não tem superfície para todas as categorias). O objetivo
desta fase é sinalizar onde há indício de problema, não confirmar exploração completa.

### 4. Aprofundamento (deep-dive)

Só roda nas categorias que a triagem sinalizou. Exige confirmação explícita do operador antes de
começar, porque é a fase mais cara em tempo, tokens e risco operacional. É aqui que entram os
payloads autorais, sujeitos ao ciclo de vida descrito no prompt central.

### 5. Relatório

Consolidação dos achados com mapeamento OWASP -> CWE -> CVE (ver
[owasp-cwe-cve-reference.md](owasp-cwe-cve-reference.md)), severidade estimada e evidência
referenciada por hash, nunca por dado bruto reproduzido no relatório final.

## Blackbox, graybox e whitebox

A classificação vem das [regras de engajamento](rules-of-engagement-template.md), preenchidas pelo
operador antes de começar, não inferida ad hoc pelo modelo durante o teste:

- **Blackbox**: nenhum conhecimento interno além do que é publicamente alcançável. A fase de
  reconhecimento carrega mais peso, porque é a única fonte de informação sobre a superfície do alvo.
- **Graybox**: alguma documentação e/ou credencial de baixo privilégio fornecida pelo operador. Permite
  pular parte do reconhecimento e ir direto para a triagem em rotas conhecidas.
- **Whitebox**: código-fonte disponível. A triagem por categoria OWASP pode ser complementada por
  leitura direta de código, o que muda a natureza da evidência (achado por análise estática, não só
  por comportamento observado em tempo de execução) e deve ser registrado como tal no relatório.

## Maturidade do ambiente como fator de risco

Um ambiente em decisão de custo (hospedagem barata, sem WAF, banco e storage recém-configurados) tem
um perfil de risco operacional diferente de um ambiente corporativo maduro. O prompt central trata a
ausência de controle compensatório não como "ambiente mais fácil de testar", mas como "ambiente onde
um erro do próprio teste tem consequência mais direta, porque não há camada extra absorvendo o
impacto". Isso é uma instrução explícita na seção de contexto de sessão do operador.
