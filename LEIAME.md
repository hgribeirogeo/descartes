# Descartes v0.2 — guia rápido

Você escreve a pergunta, a hipótese ou o contrafactual. O Descartes confronta cada item com a literatura e devolve, critério por critério, se atende, o porquê, onde melhorar e os artigos que sustentam a avaliação.

## Atualizar (você já tem a v0.1 publicada)

1. Substitua no repositório `descartes`: `index.html` e `sw.js`. Os outros arquivos não mudaram.
2. Abra `https://hgribeirogeo.github.io/descartes/` e recarregue com Ctrl+F5.
3. Seus projetos da v0.1 continuam lá. A pergunta escrita no estágio 2 vira a pergunta do projeto. Os textos antigos ficam guardados, mesmo sem aparecer na tela.

## Ligação com o LabResearch

- Abrindo o Descartes no mesmo navegador em que você usa o LabResearch, ele procura o ID do Gist e o token do LabResearch e passa a usar o mesmo Gist. Se não encontrar, preencha os dois em **Configurações → Sincronização**.
- O Descartes grava somente o arquivo `descartes.json` dentro do Gist. O arquivo do LabResearch não é alterado.
- O catálogo de revistas (nome, ISSN, links e Qualis) é lido do LabResearch. Na tela Pergunta, escolha a revista-alvo.

## Como uma análise funciona

1. **Consultas, antes de analisar:**
   - **Sugerir consultas** (cerca de 10 s): o Qwen lê o seu texto e propõe 2 ou 3 consultas em inglês, cada uma com o ângulo do item que cobre.
   - **Testar consultas** (sem IA): mostra quantos artigos cada consulta traz e os 5 mais citados, com aviso se estiver ampla demais (mais de 5.000) ou restrita demais (menos de 20).
   - Edite até ficarem precisas e só então clique em **Analisar**. Se você não sugerir nem escrever nenhuma consulta, o Qwen gera as consultas durante a análise.
2. **Busca:** o Semantic Scholar traz até 300 registros por consulta, que se somam ao corpus do projeto (aba Literatura).
3. **Ordenação:** embeddings calculados no seu PC ordenam o corpus pela proximidade com o item.
4. **Leitura:** o Qwen lê os **100 mais próximos** e classifica cada um como "já responde", "em parte" ou "não responde", com o trecho do resumo que justifica.
5. **Índice de novidade:** 100 × (1 − (já respondem + ½ em parte) ÷ lidos). A tabela de saturação mostra o índice entre os 15, 50 e 100 primeiros. Se ainda aparecerem artigos que já respondem no último terço da lista, o Descartes avisa que vale ler mais.
6. **Cartões por critério**, um critério por chamada. O selo diz de onde vem a avaliação:
   - *literatura*: os artigos mais próximos;
   - *texto*: o próprio item;
   - *coerência*: a ligação com o item anterior;
   - *revista*: os artigos da revista-alvo nos últimos 5 anos.
7. **Resumo da literatura:** cada frase aponta os artigos que a sustentam.

**Tempo:** cerca de 10 a 12 minutos por análise no seu PC, com 100 artigos. Reanalisar a mesma versão do item não relê os artigos já lidos. O número de artigos lidos é ajustável em Configurações.

**Critérios**, das revisões que você trouxe:

| Item | Critérios |
|---|---|
| Pergunta | Novidade, originalidade, impacto/relevância, rigor metodológico, alinhamento com a revista |
| Hipótese | Testabilidade (5E), novidade, fundamentação, viabilidade, rigor, coerência com a pergunta |
| Contrafactual | Plausibilidade, consistência teórica, proximidade dos dados, identificabilidade, pressupostos explícitos, coerência com a hipótese |

**Regra da novidade:**
- **atende** só com índice ≥ 70 e nenhum artigo que já responde;
- **parcial** com 1 ou 2 artigos que já respondem, ou índice entre 40 e 69;
- **não atende** com 3 ou mais, ou índice abaixo de 40.

## Programa

Registre o resultado de cada estudo do programa (MRV, temperatura do ar): corroborada, refutada ou inconclusiva, com a severidade do teste (1 a 3) e o contexto. O índice de corroboração é 100 × Σ severidade dos corroborados ÷ Σ severidade dos conclusivos, mostrado junto com o número de contextos distintos. Ele mede o quanto as hipóteses do programa resistiram aos testes. Não mede a aceitação pela comunidade.

## Limites

- **Índice de novidade:** depende das consultas, da cobertura do Semantic Scholar e do que os resumos dizem. Confira sempre os artigos marcados como "já responde".
- **Qwen 14B:** os julgamentos podem errar. Por isso cada cartão mostra a evidência, e a palavra final é sua.
- **Testes:** 33 verificações automáticas passaram, com Qwen, Semantic Scholar e GitHub simulados. Ainda falta a primeira análise real no seu PC, com o Gist e o catálogo reais do LabResearch.
