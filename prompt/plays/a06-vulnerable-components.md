# Play: Vulnerable and Outdated Components (A06:2021)

Aprofundamento de componentes vulneráveis e desatualizados: uma dependência com vulnerabilidade conhecida e
alcançável. Altitude de metodologia; qualquer prova de conceito passa pela seção 8 do
[núcleo](../core-prompt.md).

## Quando rodar

A triagem identificou, por fingerprint, uma versão de componente (servidor, framework, biblioteca exposta)
que pode ter vulnerabilidade pública conhecida.

## Pré-requisitos

- Aprofundamento autorizado para esta categoria.
- Uma versão de componente identificada com razoável confiança (banner, cabeçalho, artefato estático,
  comportamento característico).

## Objetivo

Confirmar que um componente alcançável tem vulnerabilidade pública conhecida e que a parte vulnerável é de
fato alcançável, sem detonar a exploração. A exploração real é ação de maior risco e exige autorização
específica.

## Sinais de triagem

- Versão de componente exposta por banner ou cabeçalho.
- Artefato estático (biblioteca de frontend, por exemplo) com versão identificável.
- Comportamento característico de uma versão conhecida.

## Procedimento de aprofundamento

1. **Prefira a ferramenta vetada à detecção autoral**: para identificar vulnerabilidade conhecida de um
   componente, use uma fonte ou ferramenta reconhecida de inventário/vulnerabilidade quando disponível, em
   vez de construir detecção própria. Isso é mais barato e mais confiável.
2. **Verifique o identificador contra fonte confiável**: todo CVE citado passa pela regra da seção 7 do
   núcleo (verificado contra a tabela de referência ou busca confiável; nunca inventado). Registre a versão
   observada e a correspondência.
3. **Confirme alcançabilidade, não exploração**: caracterize se a parte vulnerável do componente é de fato
   exercível pela superfície em escopo, por raciocínio e observação de baixo ruído. Não dispare o exploit.
4. **Exploração só com autorização específica**: se a prova de conceito exigir executar o exploit (ação de
   maior risco, potencialmente destrutiva), isso é payload autoral sob a seção 8 e exige autorização
   explícita daquela ação nas regras de engajamento. Sem isso, o achado para na alcançabilidade confirmada.

## Evidência a coletar

O componente e versão, o identificador público verificado, a evidência de alcançabilidade, e um hash. Ver
log-schema; sem dado sensível em claro.

## Severidade

Complementando a seção 7 do núcleo: pondere a severidade pública da vulnerabilidade, se a parte vulnerável é
alcançável de fato (uma vulnerabilidade presente mas inalcançável vale menos), e se há mitigação. Componente
crítico alcançável sem autenticação pesa alto.

## Encerramento

Qualquer artefato gerado entra na sanitização da seção 8 do núcleo.

## Referências

- OWASP A06:2021. CWE usual: CWE-1104 (uso de componente não mantido/vulnerável). Ver
  [owasp-cwe-cve-reference.md](../../docs/owasp-cwe-cve-reference.md).
- Seções 0 (limites), 7 (verificação de CVE e severidade), 8 (payload) do núcleo.
