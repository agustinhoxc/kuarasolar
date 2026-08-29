# Kuara Solar — Landing Page

Landing page estática, mobile-first, focada em SEO técnico, performance e
conversão via WhatsApp. Sem build, sem framework, sem dependências externas —
funciona publicando a pasta como está em qualquer hospedagem estática
(GitHub Pages, Netlify, Vercel, cPanel etc.).

## O que foi melhorado em relação à versão anterior

- **Vídeo do hero reprocessado**: recomprimido (H.264, `faststart`, sem
  áudio) para ~1,3 MB mantendo qualidade, pronto para autoplay mobile.
- **Imagens reais extraídas do próprio vídeo** (`assets/still-2.webp`,
  `still-5.webp`, `still-8.webp`, `assets/kuara-poster.webp`) em vez de
  imagens genéricas — mostram o sistema aberto, a energia em uso no
  porta-malas e a configuração compacta no teto.
- **Marca Kuara em SVG inline** (sol + folha, um único desenho reaproveitado
  via `<use>` no header, hero e rodapé) em vez de um arquivo de imagem —
  fica nítida em qualquer tela, não pesa nada extra e não depende de
  download de imagem. Veja a seção **Sobre a logo** abaixo: o arquivo de
  logo que você colou na conversa não chegou até mim como arquivo (só
  anexos enviados via upload chegam), então esta é uma marca própria nas
  cores da Kuara — troque pela arte oficial quando quiser.
- **Favicon completo**: `favicon.svg`, `favicon.png`, `apple-touch-icon.png`,
  ícones 192/512 e `manifest.webmanifest` (o site agora pode ser "instalado"
  como app no celular).
- **Imagem de Open Graph dedicada** (`assets/og-image.jpg`, 1200×630, JPG)
  para compartilhamento no WhatsApp/Facebook/LinkedIn — esses serviços
  historicamente falham com imagens só em WebP.
- **SEO técnico**: `<meta name="keywords">`, Twitter Card completo,
  `sitemap.xml`, `robots.txt` com linha de sitemap pronta para descomentar,
  JSON-LD com FAQ sincronizado 1:1 com as 5 perguntas visíveis na página
  (antes só 3 das 5 apareciam no schema).
- **Segurança possível em hospedagem 100% estática**: `Content-Security-
  Policy` via `<meta>` e `referrer-policy`. GitHub Pages não permite
  cabeçalhos HTTP customizados, então isto é o máximo alcançável sem trocar
  de hospedagem — detalhes na seção **HTTPS e segurança** abaixo.
- **Google Analytics 4** pronto para ligar (placeholder `G-XXXXXXXXXX`) com
  eventos de clique em todos os CTAs e no envio do formulário.
- Pequenos ajustes de acessibilidade (`aria-hidden` nos números decorativos
  dos cards) e `decoding="async"` nas imagens da seção de showcase.

Todo o resto da estrutura, copy, layout, animações e comportamento mobile
(menu, CTA flutuante, vídeo pausando fora da viewport, `prefers-reduced-
motion`, FAQ em `<details>`) foi mantido porque já seguia bem o briefing.

## Publicar no GitHub Pages

1. Suba **todo o conteúdo desta pasta** para a raiz do repositório
   (o `index.html` precisa ficar na raiz, não dentro de uma subpasta).
2. Em **Settings → Pages**, selecione **Deploy from a branch**.
3. Escolha a branch `main` e a pasta `/ (root)`.
4. Aguarde o endereço do GitHub Pages ficar disponível.
5. Se for usar domínio próprio, adicione o domínio em Settings → Pages e
   marque **Enforce HTTPS** assim que o certificado for emitido (GitHub gera
   e renova o certificado TLS automaticamente — é assim que o site passa a
   servir por HTTPS).

## Configurações obrigatórias antes de publicar de verdade

### 1. WhatsApp (única obrigatória para o formulário funcionar)

No `index.html`, procure:

```html
<body data-whatsapp="">
```

E coloque o número oficial em formato internacional, só números:

```html
<body data-whatsapp="5531999999999">
```

O formulário não precisa de servidor: ele monta a mensagem e abre direto no
WhatsApp. Sem o número configurado, o site mostra um aviso amigável em vez
de quebrar.

### 2. Domínio oficial (SEO)

Quando o domínio estiver definido, no `<head>` do `index.html`:

```html
<link rel="canonical" href="https://SEU-DOMINIO.com.br/">
<meta property="og:url" content="https://SEU-DOMINIO.com.br/">
```

(as linhas já existem comentadas logo no início do `<head>`, é só
descomentar e trocar a URL). Troque também a URL em `sitemap.xml` e a linha
`Sitemap:` em `robots.txt`, e depois envie
`https://SEU-DOMINIO.com.br/sitemap.xml` ao Google Search Console.

### 3. Google Analytics 4 (opcional)

No `<head>`, próximo ao final, há um bloco comentado com
`G-XXXXXXXXXX`. Crie uma propriedade GA4 em analytics.google.com e troque
todas as ocorrências desse ID. Se não quiser analytics agora, apague o
bloco inteiro (duas tags `<script>`) e pode remover
`googletagmanager.com` / `google-analytics.com` da linha de
`Content-Security-Policy` também.

Eventos já disparados automaticamente (só valem quando o GA estiver
configurado; sem isso o código simplesmente não faz nada):
- `cta_click` — clique em qualquer botão com `data-cta` (identifica qual:
  `hero-primary`, `hero-secondary`, `header-cta`, `floating`).
- `generate_lead` — envio do formulário de contato (inclui o perfil
  selecionado).

## Sobre a logo

O arquivo de imagem da logo colado na conversa não ficou disponível como
arquivo para processamento (apenas os arquivos enviados como anexo —
`index.html`, `README.md`, `robots.txt` e o vídeo — chegam como arquivo;
imagens coladas diretamente na mensagem, não). Para o site funcionar
imediatamente, foi criada uma marca própria (sol estilizado + folha) nas
cores oficiais da Kuara, como SVG embutido no próprio `index.html` — sem
depender de nenhum arquivo de imagem externo.

Para usar a arte oficial:
1. Exporte a logo original como PNG ou WebP com fundo transparente.
2. Substitua o `<symbol id="kuara-mark">` no início do `<body>` por uma tag
   `<img>` apontando para o arquivo (ou converta a arte para SVG e troque o
   conteúdo do `<symbol>` diretamente, mantendo a nitidez em qualquer
   tamanho de tela).
3. Regenere `assets/favicon.png`, `assets/favicon.svg`,
   `assets/apple-touch-icon.png`, `assets/icon-192.png` e
   `assets/icon-512.png` a partir da arte oficial (qualquer conversor de
   imagem para favicon resolve).

## HTTPS e segurança

- **HTTPS**: automático no GitHub Pages, tanto no domínio `github.io`
  quanto em domínio próprio com **Enforce HTTPS** ativado (Settings →
  Pages). Não existe nenhuma configuração extra de código necessária.
- **Cabeçalhos de segurança** (`Strict-Transport-Security`,
  `X-Content-Type-Options`, `X-Frame-Options`): GitHub Pages **não permite**
  configurar cabeçalhos HTTP customizados em hospedagem 100% estática — por
  isso essas tags não aparecem no `<head>` (colocá-las como `<meta
  http-equiv="X-Frame-Options">`, por exemplo, não tem efeito nenhum nos
  navegadores, apesar de aparecer em muitos templates prontos por aí). O que
  **foi** possível adicionar via `<meta>` é a `Content-Security-Policy` e a
  `referrer-policy`, já incluídas no `<head>`.
- Se no futuro você quiser os cabeçalhos completos (HSTS, X-Frame-Options
  etc.), a forma mais simples é colocar o mesmo site atrás da Cloudflare
  (plano gratuito) ou trocar a hospedagem para Netlify/Vercel, que suportam
  um arquivo `_headers`.

## Google Search Console

1. Adicione a propriedade com o domínio definitivo.
2. Verifique a propriedade (meta tag, arquivo HTML ou DNS).
3. Envie `https://SEU-DOMINIO.com.br/sitemap.xml` em "Sitemaps".
4. Use "Inspeção de URL" na home para solicitar indexação.

## Estrutura de arquivos

```
index.html                  — página inteira: HTML + CSS + JS (sem build)
manifest.webmanifest        — ícones/nome para "instalar" o site como app
robots.txt
sitemap.xml
.nojekyll                   — evita processamento Jekyll no GitHub Pages
assets/
  kuara-hero.mp4             — vídeo do hero e da seção "Tecnologia"
  kuara-poster.webp          — poster do vídeo (primeiro frame visível)
  still-2.webp               — sistema fotovoltaico aberto (showcase)
  still-5.webp               — energia em uso no porta-malas (showcase)
  still-8.webp               — configuração compacta no teto (showcase)
  og-image.jpg                — imagem 1200×630 para compartilhamento social
  favicon.svg / favicon.png   — ícone do site (aba do navegador)
  apple-touch-icon.png        — ícone para tela inicial no iOS
  icon-192.png / icon-512.png — ícones do manifest (Android/PWA)
```

## Performance

Sem frameworks, sem fontes externas, sem bibliotecas JavaScript. O único
recurso externo é o Google Analytics — e só se você optar por ativá-lo. O
vídeo pausa automaticamente quando sai da viewport (economiza bateria e
dados no celular) e todas as imagens abaixo da dobra usam `loading="lazy"`.

## Testes recomendados antes de publicar

- Larguras: 360, 390, 768, 1024, 1440px.
- `prefers-reduced-motion: reduce` ativado no sistema operacional (o vídeo
  do hero deve ser substituído pelo poster estático).
- Navegação por teclado (Tab) em todos os CTAs e no FAQ.
- Lighthouse mobile (Performance, Accessibility, Best Practices, SEO).
- Envio do formulário abrindo o WhatsApp corretamente em iOS e Android
  (depois de configurar o número).
