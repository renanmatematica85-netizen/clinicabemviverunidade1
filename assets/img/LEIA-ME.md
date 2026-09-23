# Lar de Idosos Bem Viver — site institucional

Site de página única, responsivo, sem dependências de build. Basta abrir `index.html`
no navegador ou subir a pasta inteira para qualquer hospedagem.

---

## 1. O que trocar antes de publicar

Abra o `index.html`, role até o final e localize o bloco **`const CONFIG = {`**.
Todos os dados de contato do site saem daí — é o único lugar que você precisa editar.

| Campo | O que colocar |
|---|---|
| `whatsapp` | Só números, com país e DDD: `5551999999999` |
| `telefoneExibicao` | Como o telefone aparece na tela: `(51) 3333-3333` |
| `email` | E-mail que recebe os contatos |
| `endereco` | Endereço completo com CEP |
| `instagram` / `facebook` | URL do perfil. Deixe `""` para esconder o botão |
| `msgWhatsapp` | Texto que já vem digitado quando alguém clica no WhatsApp |
| `formEndpoint` | Endpoint do formulário (veja o item 2) |

Além do `CONFIG`, ainda vale revisar manualmente:

- **Depoimentos** — a seção está com textos de exemplo. Procure o comentário
  `<!-- SUBSTITUA pelos depoimentos reais -->` e troque por depoimentos autorizados de familiares.
- **Mapa** — na seção de contato, troque o `src` do `<iframe>`.
  No Google Maps: encontre o local → *Compartilhar* → *Incorporar um mapa* → copie o `src`.
- **Bloco `application/ld+json`** (SEO) — atualize nome, telefone, e-mail, endereço e o domínio.
- **Meta tags no `<head>`** — troque `https://www.larbemviver.com.br/` pelo domínio real
  em `canonical`, `og:url` e `og:image`.
- **Números da faixa de confiança** (+10 anos, 5 refeições etc.) — confirme se batem com a realidade.

---

## 2. Fazer o formulário chegar no seu e-mail

O formulário usa o **FormSubmit** (gratuito, sem cadastro, sem back-end).

1. No `CONFIG`, troque o e-mail dentro de `formEndpoint`:
   `https://formsubmit.co/ajax/SEUEMAIL@dominio.com.br`
2. Publique o site.
3. Envie **um** formulário de teste. Você vai receber um e-mail do FormSubmit com um
   link de confirmação — **clique nele**. Isso só acontece uma vez.
4. Pronto: a partir daí todos os envios caem na sua caixa de entrada.

Se o envio falhar por qualquer motivo, o site mostra automaticamente um link para o
visitante mandar os mesmos dados pelo WhatsApp — o contato não se perde.

> Alternativas equivalentes, se preferir: Formspree, Web3Forms ou Getform.
> Basta trocar a URL do `formEndpoint`.

---

## 3. Publicar

**Opção mais simples (grátis):**
- [Netlify Drop](https://app.netlify.com/drop) ou [Vercel](https://vercel.com) — arraste a pasta `bem-viver` inteira. Sai no ar em segundos, com HTTPS.

**Hospedagem tradicional (Hostinger, HostGator, Locaweb):**
- Envie o conteúdo da pasta para `public_html/` via FTP ou gerenciador de arquivos.

Depois, aponte o domínio e ative o SSL (todas as opções acima oferecem certificado gratuito).

---

## 4. Estrutura de arquivos

```
bem-viver/
├── index.html          ← site inteiro (HTML + CSS + JS)
├── LEIA-ME.md          ← este arquivo
└── assets/img/
    ├── logo.png              (logotipo com fundo transparente)
    ├── fachada.jpg/.webp     (hero)
    ├── convivio-jardim.*     (seção Sobre + galeria)
    ├── sala-convivencia.*    (faixa de CTA + galeria)
    ├── corredor.*            (seção Estrutura + galeria)
    ├── festa-junina.*        (galeria)
    └── carnaval.*            (galeria)
```

Cada foto tem versão `.webp` (mais leve, usada por padrão) e `.jpg` (fallback para
navegadores antigos). As imagens originais eram de baixa resolução e foram ampliadas
e tratadas — **se você tiver os arquivos originais em alta, substitua-os** mantendo os
mesmos nomes. O site melhora bastante.

---

## 5. Recursos já implementados

- Layout responsivo (desktop, tablet e celular)
- Barra lateral flutuante de WhatsApp — vira barra inferior fixa no celular
- Formulário de contato com validação, máscara de telefone, anti-spam (honeypot) e
  consentimento LGPD
- Galeria com lightbox (setas do teclado e ESC funcionam)
- FAQ em acordeão nativo (`<details>`), indexável pelo Google
- Menu mobile em drawer lateral
- SEO: meta tags, Open Graph, dados estruturados Schema.org `MedicalBusiness`
- Acessibilidade: link "pular para o conteúdo", foco visível, textos alternativos em
  todas as imagens, respeito a `prefers-reduced-motion`
- Zero dependências de JavaScript externo (só as fontes do Google)

---

## 6. Identidade visual

Paleta extraída diretamente do logotipo:

| Uso | Cor |
|---|---|
| Verde principal | `#4A6B22` |
| Verde folha (logo) | `#6E9334` |
| Verde escuro (fundos) | `#26380F` |
| Coral / vermelho (logo) | `#BE3235` |
| Creme (fundo do site) | `#FBF8F3` |

Tipografia: **Fraunces** (serifada, títulos — transmite acolhimento) +
**Inter** (textos — alta legibilidade, importante para o público sênior).
O corpo do texto usa 17px, acima do padrão, justamente por acessibilidade.

---

## 7. Segurança (publicando na Vercel)

O arquivo `vercel.json` já vem pronto na pasta com cabeçalhos de segurança
recomendados. Ele é lido automaticamente pela Vercel — não precisa fazer nada,
só subir a pasta `bem-viver` como projeto (arraste no vercel.com ou use o
comando `vercel` na pasta, se tiver a CLI instalada).

O que esses cabeçalhos fazem:

- **Content-Security-Policy** — só deixa o site carregar scripts, estilos,
  fontes e conexões dos domínios que ele realmente usa (Google Fonts,
  FormSubmit, o contador de visitas e o mapa do Google). Bloqueia qualquer
  script estranho que tente rodar se o site for comprometido por outra via.
- **Strict-Transport-Security** — obriga o navegador a sempre usar HTTPS
  neste domínio depois da primeira visita.
- **X-Frame-Options** — impede que outro site coloque o seu dentro de um
  `<iframe>` (proteção contra clickjacking).
- **X-Content-Type-Options** — impede que o navegador "adivinhe" o tipo de um
  arquivo de forma perigosa.
- **Referrer-Policy** e **Permissions-Policy** — reduzem informação vazada
  para outros sites e desligam câmera/microfone/localização, que o site não usa.

Se um dia adicionar um novo serviço externo (outro formulário, um chat, etc.),
lembre de incluir o domínio dele na lista `connect-src` (ou `frame-src`/`script-src`,
dependendo do caso) dentro do `vercel.json` — senão o navegador vai bloquear
silenciosamente a conexão.

**Formulário de contato** — agora exige que o visitante marque a caixa de
consentimento LGPD antes de enviar (o envio é bloqueado até marcar). Os dados
digitados vão direto para o FormSubmit e depois para o seu e-mail; nada fica
armazenado no site.

**Boas práticas fora do código** — ative autenticação em duas etapas na conta
da Vercel e no registrador do domínio, e guarde uma cópia de backup do projeto
fora do computador (Google Drive, por exemplo).
