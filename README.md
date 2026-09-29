# CbOfertas 12.6.6

Nova automação de busca de link afiliado no aplicativo Mercado Livre.
Leia `AUTOMACAO-MERCADO-LIVRE.txt` para configuração, uso e limitações.

# CbOfertas V12.2

## Automação por rodadas

Configuração padrão:

- 3 mensagens por rodada;
- 20 segundos entre as mensagens da mesma rodada;
- 30 minutos entre as rodadas;
- limite diário de 80 mensagens.

Todos os valores são editáveis no celular.

## Calibração para aparelhos antigos

Há três estratégias:

1. somente elementos e textos da tela;
2. somente coordenadas calibradas;
3. texto primeiro e coordenadas como reserva.

Passos:

1. ativar Acessibilidade;
2. tocar em **Capturar clique do grupo** e selecionar o destino;
3. tocar em **Capturar clique de Enviar**;
4. usar uma conversa de teste, pois essa segunda calibração envia a mensagem;
5. executar **Enviar teste**;
6. ativar a fila.

A máquina de estados permite apenas dois cliques por mensagem:

1. destino;
2. Enviar.

## Transferência entre celulares

Na página Fila:

- Exportar fila;
- Importar fila;
- Somar à fila atual;
- Substituir fila atual;
- evitar duplicados pelo link ou identificador.

## Observação

A automação continua dependendo da interface do WhatsApp e do Serviço de
Acessibilidade. A calibração por aparelho reduz as diferenças entre celulares,
mas pode precisar ser refeita após atualizações do WhatsApp.


## Correção V12.2.1 — colagem em lote restaurada

- Campo grande para colar várias ofertas copiadas do WhatsApp.
- Uma oferta separada para cada link do Mercado Livre.
- O lote anterior é preservado.
- Links duplicados são ignorados.
- Título e preço são extraídos do texto.
- Seleção de todas ou algumas ofertas.
- Envio das selecionadas para a fila automática.
- Envio individual para o WhatsApp.
- Exclusão individual.
- Botão para apagar o lote completo.


## V12.3 — lote afiliado e clique reforçado

- Limpeza de cabeçalhos do WhatsApp, data, hora, remetente e linhas repetidas.
- Uma oferta separada por link.
- Link original preservado.
- Afiliado desmarcado por padrão, mesmo quando o texto colado contém meli.la.
- Checkbox afiliado somente após o botão `Usar link copiado`.
- Botão `Abrir produto no ML`.
- Botão `Copiar link original`.
- Envio individual para WhatsApp Business somente após confirmação do afiliado.
- Fila automática bloqueia ofertas de colagem sem afiliado confirmado.
- Clique do destino usa a linha inteira, não apenas as letras do nome.
- Duração e ajustes X/Y configuráveis.
- Perfil pronto para celular antigo.


## V12.4 — correção crítica dos gestos

O AccessibilityService agora declara `android:canPerformGestures="true"`.
Após instalar a V12.4, desligue e ligue novamente o serviço de Acessibilidade
para o Android carregar a nova capacidade.

O diagnóstico da Fila mostra:
- Acessibilidade;
- serviço conectado;
- Gestos LIBERADOS / NÃO LIBERADOS;
- coordenadas calibradas.

## Texto do lote

O lote reescreve cada oferta no estilo:

🔥 *CHAMADA DA OFERTA* 🔥

🛍️ Produto

💰 De ~R$ antigo~ por *R$ atual no PIX*

🎟️ Use o Cupom: *CUPOM*

🏪 Loja/Vendedor

🔗 link

O link afiliado continua sendo confirmado somente pelo botão `Usar link copiado`.


## V12.5 — foto automática no lote

- Ao separar ofertas em lote, o app tenta buscar automaticamente a imagem principal do produto.
- O link curto é resolvido até a página final do Mercado Livre.
- A imagem é obtida preferencialmente por `og:image` / `twitter:image`.
- Cada card mostra `foto pronta` ou `sem foto`.
- Botão `Buscar fotos faltantes`.
- Botão individual `Buscar foto`.
- O envio ao WhatsApp Business usa `ACTION_SEND` com imagem + legenda.
- A imagem é baixada para cache e compartilhada por `FileProvider`.
- A automação de Acessibilidade continua responsável por selecionar o grupo e tocar em Enviar.
- Se o download da imagem falhar, o envio automático não é iniciado pelo botão do Lote até a foto ser encontrada.


## V12.5.1
- Corrige erro de compilação Java em `jobId == null ? String(now) : jobId`.
- Usa `String.valueOf(now)`, que é válido em Java.


## V12.6
- Painel antigo trocado por Central operacional.
- Exportar/Importar Lote no Android.
- Embaralhar ofertas com ordem persistente.
- Importar diretamente backup JSON do CBF Ofertas / CBF Captura.
- A fila do CBF tem prioridade; Salvas completa produtos que não estavam na fila.
- Ofertas importadas continuam com afiliado NÃO confirmado no CbOfertas.
- Fotos faltantes são buscadas após a importação.
- WebView agora possui seletor de arquivos nativo.
- Envio automático ganhou confirmação da seleção do destino antes de clicar em Enviar.
- Trabalho travado em "sending" por mais de 90 segundos volta à fila.


## V12.6.1 — afiliado automático no Lote

- Qualquer link `meli.la` que entrar no Lote já fica com `Afiliado confirmado`.
- Ofertas importadas do CBF Captura priorizam `shortUrl` afiliado.
- Se o `shortUrl` não estiver no objeto, o CbOfertas procura `meli.la` na mensagem preparada.
- O importador reconhece:
  - exportação CBF V2.3 (`cbfToCbOfertas`);
  - backup CBF antigo (`cbfOfertasBackup`);
  - arquivos com `data.queue` / `data.savedOffers`;
  - histórico como reserva.
- O lote que já estava salvo é migrado: links `meli.la` existentes passam a ficar ativos automaticamente.
- Link comum do Mercado Livre não é marcado falsamente como afiliado.


## V12.6.2 — segunda mensagem presa em “Enviar para”

Correção específica para o caso em que:
1. a primeira oferta é enviada corretamente;
2. a segunda abre o WhatsApp;
3. fica parada na tela `Enviar para...`.

Mudanças:
- após uma seleção de grupo confirmada, salva automaticamente a coordenada que funcionou;
- a próxima mensagem tenta primeiro o último clique comprovado;
- o nome do grupo é comparado ignorando acentos, emoji, pontuação e diferenças de espaços;
- varredura da árvore completa de Acessibilidade quando `findAccessibilityNodeInfosByText` falha;
- fallback da calibração ganha prioridade após a primeira tentativa;
- memoriza também o último botão Enviar que funcionou;
- diagnóstico mostra `Último grupo OK` e `Último Enviar OK`.


## V12.6.3 — importação do CBF com fotos

- usa `imageData` quando o CBF exportar a foto incorporada;
- aceita compartilhar imagem em `data:image/...;base64,...`;
- mantém compatibilidade com `image`/`imageUrl` comuns.


## V12.6.4 — retorno automático ao CbOfertas

- Corrige aparelhos em que o WhatsApp envia a mensagem, mas permanece aberto.
- O serviço de acessibilidade usa Voltar e depois traz o CbOfertas para frente.
- Inclui uma tentativa extra apenas se o WhatsApp continuar em primeiro plano.


## V12.6.5 — seleção do grupo mais resistente

A automação procura o grupo pelo nome antes de qualquer coordenada, invalida posições antigas quando um toque não é confirmado e recupera automaticamente falhas temporárias da tela de compartilhamento do WhatsApp.
