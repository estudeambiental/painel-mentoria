# Contexto do projeto — Páginas de vendas Estude Ambiental

Resumo de referência do trabalho feito na branch `claude/sales-page-design-9l9omo`, pra não depender de vasculhar o histórico de chat. Atualize esse arquivo conforme o projeto evoluir.

## Sites e onde cada pasta do repositório vai parar

A hospedagem real é o **cPanel da Hostgator** (não é GitHub Pages, mesmo o repo tendo uma pasta `docs/` — essa pasta não é o que serve o site).

| Pasta no repositório | URL ao vivo | Pasta no cPanel (`public_html/`) |
|---|---|---|
| `mentoria-estude-ambiental/` | `estudeambiental.com.br` (raiz) | `public_html/` (raiz) |
| `mentoria-estude-ambiental/institucional/` | `estudeambiental.com.br/institucional/` | **ainda não publicada** — pendente |
| `docs/discursiva-fcc-fundacao-florestal-sp/` | `estudeambiental.com.br/discursiva-fcc-fundacao-florestal-sp/` | `public_html/discursiva-fcc-fundacao-florestal-sp/` |
| `docs/discursiva-fepese-itajaisc/` | `estudeambiental.com.br/discursiva-fepese-itajai-sc/` | `public_html/discursiva-fepese-itajai-sc/` (nome da pasta no servidor tem hífen antes do "sc", diferente do nome da pasta no repo) |

`estudeambiental.com` (sem `.br`) é um **site novo, ainda em construção, separado deste repositório** — não confundir com o `.com.br`, que é o site atual no ar.

## Como publicar uma alteração (passo a passo do cPanel)

1. Faço a alteração no código e gero um `.zip` da pasta correspondente.
2. No Gerenciador de Arquivos do cPanel, **entrar dentro da pasta de destino exata** antes de qualquer coisa (ex.: `public_html/discursiva-fepese-itajai-sc/`, ou `public_html/` raiz pra home da mentoria).
3. Clicar em **Carregar** *estando dentro dessa pasta* e subir o zip ali.
4. Clicar com o botão direito no zip → **Extrair**, confirmando a sobrescrita do `index.html` e `assets/` existentes.
5. Conferir em aba anônima (evita cache do navegador mostrar a versão antiga).

**Cuidado**: extrair o zip no nível errado (ex. na raiz do `public_html/` quando era pra estar dentro de uma subpasta) já sobrescreveu por engano o `index.html` da home com o conteúdo de outra página. Sempre confirmar em qual pasta está antes de extrair.

## Decisões de conteúdo já tomadas

- **Ordem das seções da home** (pensada pra conversão, tráfego de anúncio, atenção curta): Hero → faixa curta de prova social → VSL → dor → mecanismo/reposicionamento → **depoimentos/resultados** (movido pra logo após o mecanismo, antes só aparecia no fim) → Método TEIA → como funciona → dia a dia → o que recebe → especialização → para quem → sobre Dominique → FAQ → CTA final.
- **A mentoria não entrega material de conteúdo** (apostilas/PDFs/videoaulas) — só organiza a preparação e indica fontes. Isso foi alinhado em vários pontos: seção "O que você recebe", contrato, e nas páginas de Discursiva.
- **Discursiva é bônus condicionado** a quem tem essa fase no edital, não item padrão da oferta.
- **Nome "Tutory"** (fornecedor da plataforma) removido de todo texto visível ao aluno — pra ele, é só "a área exclusiva"/"a plataforma da mentoria", nunca o nome do fornecedor.
- **Não mostrar número exato de aprovados** por enquanto (são 10 no total, considerado pouco) — a faixa de prova social fala "Aprovados nas 3 esferas: municipal, estadual e federal", sem número.
- Nas páginas de Discursiva (FCC/FEPESE): nome do concurso/banca em destaque (negrito mais forte) no badge do hero, e citado na headline (H1) — pedido do gestor de tráfego.
- Logo do header da home aumentada de 34px pra 42px de altura.

## Script de rastreamento de UTM

Adicionado **só na home da mentoria** (`mentoria-estude-ambiental/index.html`, antes do `</body>`) — propaga `utm_source`, `utm_medium`, `utm_campaign`, `utm_term`, `utm_content`, `fbclid` e `gclid` da URL da página pro link do formulário de aplicação (`estude-ambiental-form.vercel.app`, o formulário próprio que está substituindo o Respondi), antes do clique. Não muda nada visualmente. **Não foi adicionado** nas páginas de Discursiva nem nas institucionais — só onde tem o link do formulário.

## Referências e números

- **WhatsApp comercial**: 55 48 99818-8078 (Florianópolis/SC)
- **CNPJ**: 55.491.669/0001-15 (Dominique Martins Sala, MEI)
- **Meta Pixel ID**: 706004184857947
- **Preço Discursiva FCC/FEPESE**: R$ 67,00 à vista (Pix/boleto) ou 12x de R$ 6,93 no cartão
- **Prova FCC e FEPESE**: 30/08/2026
- **Checkout Tutory FCC**: `pay.plataformatutory.com.br/checkout/15bba2f8-43f7-4274-8c46-889e888bb8a3`
- **Checkout Tutory FEPESE**: `pay.plataformatutory.com.br/checkout/d0f2b420-9b60-473f-8a6e-6f4a45c63f56`
- **Formulário novo (substituindo Respondi)**: `estude-ambiental-form.vercel.app` (projeto Vercel confirmado, existe na conta)
- **Time Vercel**: "Dominique - Estude Ambiental's projects" (`team_HEObyZTRX4G302hhceXEalDf`), com os projetos: `estude-ambiental-app`, `estude-ambiental-form`, `estude-ambiental-carrossel`, `estude-ambiental-feedbacks`, `ea-deploy-schema-check`. Nenhum tinha o domínio `estudeambiental.com` conectado na última checagem.

## Pendências em aberto

1. **Publicar `institucional/`** (Termos, Política, Contrato) em `estudeambiental.com.br/institucional/` — os links do rodapé na home e nas páginas de Discursiva já apontam pra lá, mas ainda dão 404 até isso ser subido.
2. **`estudeambiental.com`** (site novo, sem `.br`) — ainda não identifiquei onde está sendo construído/hospedado; não está neste repositório nem nos projetos Vercel conectados aqui.
3. Confirmar se a decisão de ter a home da mentoria na raiz do domínio (em vez de subpasta) está definitiva — hoje já está assim na prática, depois do incidente de recuperação.
