# Contexto do projeto — Estude Ambiental

Documento de referência do trabalho feito na branch `claude/sales-page-design-9l9omo`,
para não depender de vasculhar histórico de conversa. **Atualize conforme o projeto evoluir.**

Última atualização: **24 de setembro de 2026**

---

## 1. Links que não podem ser perdidos

Os vídeos do YouTube são **não listados**: não aparecem em busca nem no canal.
Se estes links se perderem, o acesso se perde junto.

| O que | Link |
|---|---|
| VSL (8:27) | `https://youtu.be/POIGtgzNSs8` |
| Depoimento Lucemir | `https://youtu.be/xNrKYavJyS0` |
| Depoimento Daniel | `https://youtu.be/VkXaHq601i4` |
| Depoimento Luanna | `https://youtu.be/CjDCH91p6cA` |
| Depoimento Arianne | `https://youtu.be/FHLAsuT8Twk` |
| Playlist das entrevistas | `PLKaRyxtns2Mnhpjm1hrJMNJKRzedLTnka` |
| Formulário de aplicação | `https://form.respondi.app/USGGUf9h` |

Outros dados fixos:

- **Domínio definitivo**: `estudeambiental.com`
- **`estudeambiental.com.br`**: o domínio está registrado na HostGator, mas o site **está no
  Framer** (A `31.43.160.6`/`31.43.161.6`, `www` → CNAME `sites.framer.app`), verificado em
  24/09/2026. A pasta `estudeambiental.com.br` do cPanel **não é servida**: subir arquivo lá
  não muda o site. Recomendação: redirecionar o `.com.br` para o `.com` (301), em vez de
  manter duas cópias.
- **Hospedagem**: HostGator, Plano M, ativo até 20/03/2027
- **Nameservers**: `ns1090.hostgator.com.br` / `ns1091.hostgator.com.br` → IP `69.49.241.120`
- **CNPJ**: 55.491.669/0001-15 — 55.491.669 Dominique Martins Sala (MEI)
- **Endereço**: Rua Dario Manoel Cardoso, 132, Ingleses do Rio Vermelho, Florianópolis/SC, CEP 88058-400
- **E-mail**: estudeambiental@gmail.com
- **WhatsApp comercial**: 55 48 99818-8078
- **Meta Pixel ID**: 706004184857947
- **Google Analytics**: ainda não instalado (os eventos já estão escritos, esperando a tag)

---

## 2. Onde cada pasta do repositório vai parar

A hospedagem é o **cPanel da HostGator**. A pasta `docs/` do repositório **não** serve o site
(não é GitHub Pages).

| Pasta no repositório | URL ao vivo | Pasta no cPanel |
|---|---|---|
| `mentoria-estude-ambiental/` | `estudeambiental.com` (raiz) | `public_html/` |
| `mentoria-estude-ambiental/institucional/` | `estudeambiental.com/institucional/` | `public_html/institucional/` |
| `docs/discursiva-fcc-fundacao-florestal-sp/` | `.../discursiva-fcc-fundacao-florestal-sp/` | mesma pasta |
| `docs/discursiva-fepese-itajaisc/` | `.../discursiva-fepese-itajai-sc/` | **atenção**: no servidor o nome tem hífen antes do "sc" |

### Como publicar

1. Gerar um `.zip` da pasta correspondente.
2. No Gerenciador de Arquivos, **entrar dentro da pasta de destino** antes de qualquer coisa.
3. **Carregar** o zip *estando dentro dessa pasta*.
4. Botão direito no zip → **Extrair**, confirmando a sobrescrita.
5. Conferir em aba anônima, para o cache não mostrar a versão antiga.

> **Cuidado que já custou caro:** extrair na raiz quando era para ser numa subpasta
> sobrescreveu o `index.html` da home com o conteúdo de outra página. Confirme a pasta
> antes de extrair.

---

## 3. A home hoje (`mentoria-estude-ambiental/index.html`)

Arquivo único, sem build. ~21.500px no desktop, **628 KB no primeiro carregamento**.

### Ordem das seções

Hero → barra de prova social → **VSL** → dor → "não precisa de mais conteúdo" (comparativo) →
"do outro lado" (o que muda com a aprovação) → **Depoimentos** (cards + 4 vídeos + quadro de
aprovações + prints) → frase de transição → Método TEIA → como funciona → dia a dia (6 clipes) →
o que você recebe → para quem é → Sobre Dominique → **Por que agora** → FAQ (11) →
Recapitulando → Garantia 7 dias → CTA final.

São 11 CTAs distribuídas ao longo da página.

### VSL — como funciona

- Embed pela **IFrame API** do YouTube em `youtube-nocookie.com`, 16:9 fluido.
- Começa com **autoplay mudo + playsinline** — única combinação que iPhone e Android
  deixam tocar sozinho.
- Camada **"Clique para ouvir"** por cima: no clique tira o mudo, volta para o segundo 0,
  dá play e some. Funciona no iOS porque acontece dentro de um gesto do usuário.
- O CTA **"Quero uma preparação com direção" só aparece aos 445s (7:25)**, lido do
  `getCurrentTime`. É o ponto em que a Domi fala "logo abaixo desse vídeo está a aplicação".
  Para mudar esse momento, altere `data-cta-em` no elemento `#vslEmbed`.
- Esse CTA leva **direto ao formulário do Respondi**, em nova aba, com as UTMs do anúncio
  repassadas automaticamente.
- A API do YouTube só é baixada quando a pessoa chega perto do vídeo.

### Eventos de medição

Disparam no **Meta Pixel** e no **Google Analytics** (o GA só passa a receber quando a tag
for instalada; o código já está pronto e não quebra nada enquanto isso):

`CliqueHeroParaVSL` · `VSLPlay` · `VSLSom` · `VSLProgresso` (25/50/75/100%) · `VSLCtaExibido` ·
`VSLCtaClique` · `DepoimentoPlay` · `CliqueChamadaAplicar` · `Lead`

`Lead` (evento padrão do Pixel, o que o Meta usa para otimizar) dispara em **todo clique que
sai para o formulário**, com o parâmetro `origem` (`vsl` ou `final`). Até 24/09 ele também
disparava no envio do popup de saída, o que misturava contatos de popup com aplicações.

### Performance — decisões que não devem ser desfeitas

O primeiro carregamento já foi **2.034 KB** e hoje é **628 KB**. O que segura isso:

- Os 6 clipes da plataforma e seus posters **só baixam quando o card entra na tela**.
  Antes o `autoplay` forçava ~740 KB de vídeo mesmo para quem nunca rolava até lá.
  Se alguém remover o carregamento sob demanda, a página volta a pesar 2 MB.
- Fotos da Dominique reduzidas para 1000px de largura (343 KB → 126 KB e 303 KB → 85 KB).
- Preload das duas fontes e da imagem da primeira tela.
- Os players do YouTube (VSL e depoimentos) só carregam sob demanda.

---

## 4. Provas sociais — o que está publicado e de onde veio

### Cards de resultado (6)

Lucemir, Arianne, Daniel, Luanna, André e Sandra. Todos com foto, menos **André**, que não
tem foto disponível e usa um monograma "A".

### Quadro de aprovações oficiais (11 linhas)

Cada linha foi conferida no resultado publicado pela banca:

| Órgão | Cargo | Aluno | Resultado |
|---|---|---|---|
| CPRH/PE | Analista em Gestão Ambiental (Biologia) | Isadora | 1º |
| IDEMA/RN | Fiscal Ambiental | Arianne | 2º |
| Pref. Planaltina/GO | Fiscal Ambiental | Thalyssa | 2º, já nomeada |
| IPAAM/AM | Analista Ambiental (Eng. Florestal) | André | 2º |
| IDEMA/RN | Analista Ambiental (Arquitetura) | Arianne | 4º |
| IDEMA/RN | Analista Ambiental (Geografia) | Daniel | 4º |
| IDEMA/RN | Analista Ambiental (Eng. Florestal) | Luanna | 4º |
| UFAC | Técnico em Agropecuária | Sandra | 5º |
| IDEMA/RN | Fiscal Ambiental | Lucemir | 5º (cotas) / 30º (ampla) |
| SEMMAS Manaus | Analista Municipal II (Eng. Florestal) | Jhonatan | 8º |
| SEMMAS Manaus | Analista Municipal II (Eng. Florestal) | André | 23º |

**Fora da página de propósito:** Hebert, Prefeitura de Itabaiana/PB, 2º lugar em Engenheiro
Ambiental. Ele está **classificado fora das vagas**, sem convocação até agora, e a Domi ainda
não divulgou. Entra quando houver convocação.

**Por que o quadro é em HTML e não são os prints das listas:** os prints oficiais são listas
completas, com dezenas de nomes, números de inscrição e datas de nascimento de candidatos que
não são alunos. Publicar aquilo vazaria dado pessoal de terceiros. O quadro mostra só a linha
da aluna, primeiro nome apenas. Os prints originais continuam com a Domi como comprovação.

### Prints de WhatsApp (12)

Em `assets/img/depoimentos/`. Todos autorizados pelos alunos. Nomes borrados, e no print da
nomeação (dep-1824) também foram borrados o nome completo, o nome de um terceiro e os números
de inscrição que apareciam no documento oficial — preservando só o cabeçalho que prova o cargo.

Mosaico de 3 colunas no desktop; no celular vira faixa deslizante, para 12 prints não
esticarem a página. Clique amplia (lightbox com Esc, clique fora e teclado).

---

## 5. Funil da página (sugestões do Ítalo, gestor de tráfego — 24/09/2026)

A página é **carta de apresentação e funil de qualificação**: a pessoa deve ler e assistir
antes de aplicar. Por isso:

- **Botão do topo → VSL**, e não mais para o fim da página.
- **Botão da VSL (7:25) → direto no formulário do Respondi**, com o texto "Quero aplicar para
  a Mentoria" e uma linha embaixo avisando que são perguntas, que cada aplicação é analisada
  e que aplicar não garante vaga. Os dois botões que abrem o formulário (VSL e final) têm o
  mesmo texto; os do meio da página levam à leitura. O preço fica fora dessa linha de
  propósito, para o caçador de preço não preencher o formulário só para receber o número.
- **Sem botão fixo de "Quero aplicar"** em lugar nenhum: saiu a barra do rodapé do celular
  e o botão do menu no computador.
- **Sem o link "Prefere não assistir agora?"** abaixo da VSL.
- **Sem popup de saída** pedindo nome, e-mail e WhatsApp. Motivo do Ítalo: capta dado
  incompleto e pula a qualificação do Respondi; funciona para infoproduto, não para
  mentoria. Além disso, o popup abria após 45s sem mexer no mouse — ou seja, **em cima
  da VSL** para quem estava assistindo parado.

A aplicação continua acessível pelos botões ao longo da leitura (que levam à seção final)
e pelo CTA da VSL.

## 5b. Decisões de conteúdo já tomadas

- **Preço não aparece na página.** O interessado vai para a aplicação e o preço é apresentado
  na conversa no WhatsApp, conforme o perfil.
- **A entrega é a mesma nos três planos** (3, 6 e 12 meses). Muda só a duração e o valor.
  Isso está dito na página, no FAQ e no Parágrafo Único do Inciso II da Cláusula Segunda.
- **A mentoria não entrega material de conteúdo** (apostilas/PDFs/videoaulas) — organiza a
  preparação e indica fontes.
- **Discursiva é bônus condicionado** a quem tem essa fase no edital.
- **Nome "Tutory" não aparece** para o aluno — é "a plataforma da mentoria".
- **Não mostrar número de aprovados.** O quadro de aprovações fala por si.
- **Escassez**: "vagas limitadas", sem número.
- **Garantia de 7 dias** (art. 49 do CDC), de satisfação e não de aprovação.
- **Depoimentos em vídeo são autorizados só para site e feed** — não usar em anúncio pago.
- Cada depoimento responde a uma objeção: Lucemir (não sou da área), Daniel (já tentei e não
  passei), Luanna (já tenho material), Arianne (não tenho tempo). Ficam **os quatro juntos**
  na seção Depoimentos — foram testados espalhados pela página e a Domi preferiu agrupados.

---

## 6. Documentos institucionais

Os três estão em `mentoria-estude-ambiental/institucional/`, datados de **21/09/2026**,
sem lacunas, e linkados no rodapé do site.

- `contrato-mentoria/` — Contrato de Prestação de Serviços Educacionais (~2.700 palavras)
- `termos-e-condicoes/`
- `politica-de-privacidade/`

**Pendência que não é técnica:** o contrato precisa de **revisão por advogado** antes de ser
tratado como definitivo, em especial a **multa rescisória de 80%** (Cláusula Nona, Inciso II).
Ela veio do contrato real já usado pela Domi, mas multa nesse patamar em relação de consumo é
o tipo de cláusula que costuma ser contestada.

---

## 7. Regras de marca

- Verde `#68995A`, grafite `#443C42`, verde claro `#C5FFB5`, fonte **Gilroy**.
- **Não trocar essas cores.** Decisão da Domi.
- Consequência conhecida: texto branco sobre `#68995A` dá **3,3:1** de contraste, abaixo do
  mínimo de 4,5:1. Foi resolvido no botão grande subindo o texto para 18,9px (entra no
  critério de "texto grande", que exige 3:1). Os botões menores — topo e barra fixa —
  seguem abaixo do ideal. Para resolver sem mexer na marca, bastaria escurecer só o fundo
  do botão.

---

## 8. Ambiente e limitações conhecidas

- **Egress bloqueado** no ambiente de trabalho do Claude: `estudeambiental.com`, YouTube,
  Google Drive, HostGator. Por isso o site publicado e os vídeos do YouTube **não podem ser
  testados daqui** — o teste real é sempre no domínio, depois de subir.
- O Chromium usado para verificação **não tem codec H.264**, então a reprodução dos MP4 da
  plataforma não pode ser confirmada localmente (o carregamento e o poster, sim).
- Arquivos anexados no chat têm **limite de 30 MB**.

### Incidente de DNS (21/09/2026)

`estudeambiental.com` saiu do ar com `DNS_PROBE_FINISHED_NXDOMAIN`. Não era o site nem a
hospedagem: os nameservers da HostGator estavam retornando **SERVFAIL** para a zona. Voltou
sozinho. Se repetir, a frase que resolve rápido no suporte é:

> "Os nameservers ns1090 e ns1091 estão retornando SERVFAIL para a zona do meu domínio.
> A delegação no registro está correta e a hospedagem está ativa. O problema é na zona DNS
> do servidor de vocês."

Depois, limpar o cache local: `ipconfig /flushdns` e `chrome://net-internals/#dns`.

---

## 9. Pendências em aberto

1. **Instalar a tag do Google Analytics.** Os eventos já estão programados e passam a
   funcionar sozinhos assim que o `gtag` existir na página.
2. **Revisão do contrato por advogado** (ver seção 6).
3. **Foto do André** para substituir o monograma nos cards de resultado.
4. **Hebert / Itabaiana** entra no quadro de aprovações quando houver convocação.
5. **Página de link na bio** (estilo `luanacarolinas.com.br/bio`), reunindo site, aplicação,
   Discursiva SEMA MT, Discursiva MMA/IBAMA/ICMBio, guia "do zero ao concursado ambiental" e
   comunidade gratuita no WhatsApp. Combinada, ainda não começada.
6. **Escurecer o fundo dos botões pequenos** se um dia a acessibilidade virar prioridade
   (ver seção 7).

---

## 10. Backup — o que está e o que não está aqui

**Está versionado no GitHub:** todo o código do site, os documentos institucionais, os 12
prints já tratados, as fotos recortadas dos alunos, os 6 clipes da plataforma já cortados e
comprimidos, e este documento.

**Não está, e só existe na máquina da Domi:**

- os dois `.rar` com os 68 prints originais e os 11 prints de resultado oficial;
- os vídeos brutos de tela da plataforma;
- os arquivos originais das entrevistas (as versões publicadas estão no YouTube).

Vale manter uma cópia desses originais em outro lugar (HD externo ou nuvem), porque são a
comprovação documental das aprovações e o material-fonte de tudo que está publicado.
