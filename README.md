# Empires Erased

**Projeto educacional / portfólio.** Um prompt central (e a documentação de apoio ao redor dele) para
conduzir testes de segurança autorizados de forma padronizada, com qualquer LLM capaz de seguir
instrução estruturada, em qualquer plataforma alvo.

## O que é

Um conjunto de instruções operacionais, ancoradas no OWASP Top 10, que qualquer LLM pode seguir para
conduzir um engajamento de teste de segurança do início (regras de engajamento, detecção de ambiente,
identificação de controles compensatórios) ao fim (relatório com achados mapeados a CWE/CVE e
sanitização de artefatos gerados durante o teste).

O [prompt central](prompt/core-prompt.md) nasceu da constatação de que testes de segurança conduzidos
por um LLM sem um protocolo formal por trás tendem a três problemas recorrentes: consumo de tempo e
custo desproporcional ao valor encontrado, quando o modelo aprofunda em tudo de uma vez sem
priorização; ambiguidade sobre o que está de fato autorizado, quando o escopo não é formalizado antes
de começar; e artefatos gerados durante o teste (payloads, evidências) sem nenhum ciclo de vida
definido depois que o engajamento termina.

## O que este projeto entrega

1. **Um prompt agnóstico de LLM e de plataforma**: mesma instrução funciona em qualquer modelo com
   capacidade de seguir instrução estruturada, e não assume Linux, Windows ou contêiner por padrão.
   Ele detecta o ambiente disponível antes de propor qualquer comando.
2. **Gate de pré-engajamento obrigatório**: o modelo se recusa a testar sem escopo, autorização e
   classificação (blackbox/graybox/whitebox) formalizados primeiro, usando o
   [template de regras de engajamento](docs/rules-of-engagement-template.md).
3. **Detecção de controles compensatórios antes de qualquer teste ativo**: identifica e reporta a
   presença de WAF, firewall ou rate limiting, e usa isso para calibrar agressividade, em vez de tentar
   contornar um controle sem autorização específica para isso.
4. **Metodologia faseada com gates de confirmação**: reconhecimento, triagem leve por categoria OWASP,
   aprofundamento só onde há sinal, sempre com checkpoint explícito do operador antes da fase mais
   cara. Detalhes em [methodology.md](docs/methodology.md).
5. **Mapeamento de achado para CWE e CVE com contexto de criticidade**, usando uma
   [tabela de referência estática](docs/owasp-cwe-cve-reference.md) como base e busca externa como
   complemento, nunca inventando um identificador quando não há correspondência confiável.
6. **Ciclo de vida completo de payload autoral**: declaração ao operador antes de qualquer payload
   gerado pelo próprio modelo, confirmação explícita antes de executar, e sanitização guiada ao final
   do engajamento.
7. **Log estruturado de toda a execução**, em diretório validado previamente pelo operador, com regras
   de redação para nunca gravar credencial ou dado pessoal em texto puro. Esquema completo em
   [log-schema.md](docs/log-schema.md).

## O que não faz

Não é uma ferramenta de exploração automatizada, não substitui a leitura humana do relatório final, e
não decide sozinho quando um teste está autorizado. Toda ação ativa depende de escopo formalizado pelo
operador antes de começar, e qualquer ação de maior risco (payload autoral, teste em ambiente de
produção) exige confirmação explícita a cada vez, não uma autorização genérica dada uma única vez no
início.

## Como usar

1. Preencha o [template de regras de engajamento](docs/rules-of-engagement-template.md) para o alvo
   que você tem autorização para testar.
2. Prepare os dois diretórios locais indicados no template (log e artefatos).
3. Copie o conteúdo de [prompt/core-prompt.md](prompt/core-prompt.md), a partir da seção
   "Instruções", como instrução de sistema (ou primeira mensagem) do LLM que você for usar.
4. Forneça ao modelo as respostas do template preenchido. O prompt se recusa a avançar sem elas.
5. Veja [examples/synthetic-run.md](examples/synthetic-run.md) para um exemplo completo, do
   preenchimento das regras de engajamento ao relatório final, usando um alvo inteiramente fictício.

## Estrutura do repositório

```
prompt/core-prompt.md              instrução central, agnóstica de LLM e de plataforma
docs/rules-of-engagement-template.md  template a preencher antes de qualquer teste
docs/methodology.md                explicação da metodologia faseada e dos gates
docs/owasp-cwe-cve-reference.md    tabela estática OWASP -> CWE -> CVE
docs/log-schema.md                 esquema do log estruturado de execução
examples/synthetic-run.md          engajamento completo, alvo fictício
```

## Aviso

Este prompt existe para testes de segurança formalmente autorizados: engajamentos com escopo e
autorização por escrito, sistemas de propriedade do próprio operador, ou ambientes de laboratório e
CTF. Nada aqui deve ser usado contra um sistema sem autorização explícita do responsável por ele.
