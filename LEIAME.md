# Descartes v0.1 — instalação e primeiro teste

Aplicativo de uso pessoal para conceber pesquisas do esboço à teoria. Funciona no PC e no celular, offline, sem servidor. A IA (Qwen) roda no seu PC e é opcional: rubricas, matriz, DAG, diário e buscas funcionam sem ela.

## 1. Publicar no GitHub Pages (5 min)

1. Crie um repositório público chamado `descartes` na sua conta (`hgribeirogeo`).
2. Envie para a raiz do repositório: `index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`.
3. Em *Settings → Pages*, escolha a branch `main` e a pasta `/ (root)`.
4. O endereço será `https://hgribeirogeo.github.io/descartes/`.

A pasta `testes/` não precisa ir para o repositório. Ela serve para repetir os testes automatizados.

## 2. Instalar o Qwen no PC (Windows)

1. Instale o Ollama: https://ollama.com/download
2. No PowerShell, baixe os modelos:
   ```
   ollama pull qwen3:14b
   ollama pull qwen3-embedding:0.6b
   ```
   O `qwen3:14b` tem cerca de 9 GB e cabe inteiro na RTX 3060 de 12 GB. Para comparar depois: `ollama pull qwen3:30b-a3b`, que roda dividido entre a placa e a RAM.
3. Libere o site para conversar com o Ollama. No PowerShell:
   ```
   setx OLLAMA_ORIGINS "https://hgribeirogeo.github.io"
   ```
   Feche o Ollama pelo ícone da bandeja e abra de novo.
4. Abra o Descartes no Chrome ou no Edge. Na primeira chamada, o navegador pede permissão para acessar dispositivos da rede local. Aceite: é o Ollama no seu próprio PC.
5. Vá em **Configurações → Testar conexão**. O selo no topo deve mostrar "IA: Qwen local".

## 3. Bases bibliográficas (nada a configurar)

- **Semantic Scholar (principal, sem chave):** busca reproduzível no título e no resumo, com "frase exata", `+` (e), `|` (ou), `-` (excluir) e parênteses. Também fornece a rede de citações do núcleo e a cadeia de referências e citantes. O limite de uso é compartilhado entre todos os usuários sem chave; em horários de pico o app espera e tenta de novo.
- **Scopus e Web of Science:** busque pelo Periódicos CAPES, exporte em CSV (Scopus) ou em texto delimitado por tabulação (Web of Science) e importe em Literatura → Fontes.
- **Consensus:** exporte em CSV e importe. Entra como busca exploratória, e a coluna Takeaway (gerada por IA) é descartada.
- **Crossref:** confere os DOIs sugeridos pelo crítico, sem chave.
- **OpenAlex (opcional):** fica escondido em Configurações. Só use se o Semantic Scholar falhar no seu navegador.

**Primeira coisa a testar:** faça uma busca no Semantic Scholar pelo app publicado. Este ambiente de testes não alcança a API real, então ainda não confirmei que ela aceita chamadas vindas do navegador. Se aparecer erro de rede, me avise: a alternativa é a chave gratuita do OpenAlex.

## 4. Celular e sincronização

1. No celular, abra o endereço e use "Adicionar à tela inicial".
2. Para sincronizar com o PC, use o mesmo ID de cliente OAuth do LabResearch:
   - No Google Cloud Console, confirme que `https://hgribeirogeo.github.io` está nas origens JavaScript autorizadas e que a Google Drive API está ativa.
   - Cole o ID em Configurações e toque em "Conectar e sincronizar" nos dois aparelhos.
   - Os dados ficam na pasta oculta do aplicativo no seu Drive. As chaves de API não são sincronizadas.
3. Uma crítica pedida no celular sem IA entra na fila. Ela roda quando o PC abre o app e sincroniza.
4. Em conflito, vence a versão mais recente **de cada projeto**. Evite editar o mesmo projeto nos dois aparelhos sem sincronizar entre uma edição e outra.

## 5. Roteiro do primeiro teste (Programa A — MRV)

1. **Novo projeto.** Escreva o esboço sem consultar a pergunta-mãe da carteira de temas. Ela foi redigida pelo Claude e contaminaria o teste.
2. **Estágio 1.** Escreva, faça a autoavaliação e peça a crítica N1. Registre uma decisão para cada crítica com nota até 3.
3. **Literatura.**
   - Faça ao menos uma busca por termos (Semantic Scholar) e importe o CSV do Consensus.
   - Escreva os critérios de triagem e rode a triagem.
   - Defina dois ou três campos de extração e rode a extração.
   - Fixe o limiar, sorteie a amostra de 30 e codifique às cegas.
   - Escreva a afirmação de lacuna, classifique o corpus, leia a lista obrigatória e declare a lacuna.
4. **Volte ao estágio 1** e declare o portão.
5. **Anote** para cada etapa: tempo de máquina, tempo seu e o que travou. Esses números calibram a versão 0.2.

## 6. O que foi testado e o que não foi

Antes da entrega, 44 verificações automáticas rodaram no Chrome com um Qwen simulado e as APIs externas interceptadas. Elas cobriram:
- **Estágios e crítico:** fluxo dos estágios, bloqueios dos níveis de ajuda e decisões obrigatórias.
- **Verificações de evidência:** trechos e DOIs inventados são sinalizados.
- **Literatura:** importação do seu CSV do Consensus (11 registros), deduplicação por DOI, triagem, extração com trecho verificado, validação com kappa, mapa, grupos, núcleo e lacuna.
- **Matriz e DAG:** detecção de linhas idênticas na matriz e caminhos pela porta dos fundos no DAG.
- **Resto:** fila offline, persistência e layout no celular.

**Não testado aqui, e a testar por você:**
- a qualidade real das respostas do Qwen 14B;
- o tempo real por artigo na sua RTX 3060;
- a busca real no Semantic Scholar a partir do navegador;
- a permissão de rede local do Chrome com o Ollama;
- a sincronização com o Google Drive.

Para repetir os testes: `python testes/mock_ollama.py` em um terminal, `python -m http.server 8080` na pasta do app em outro, e `python testes/e2e.py`. Requer Playwright e Chrome.

## Limites conhecidos da v0.1

- A extração usa só título e resumo. A busca de texto completo de acesso aberto (Unpaywall) fica para a v0.2.
- O dossiê é gerado como página para imprimir em PDF pelo navegador.
- A nota do crítico não é verdade de referência. A calibração real vem dos pareceres dos periódicos.
