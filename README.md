# Portfólio | Leônidas Leandro

Portfólio pessoal de Leônidas Leandro, instrutor estrategista com duas décadas de experiência na intersecção entre Tecnologia e Educação. A página reúne trajetória, competências, projetos e certificações, com uma identidade visual imersiva em roxo, rosa e amarelo.

**Acesse o site:** [www.portifolioleonidas.com](https://www.portifolioleonidas.com/)

Todo o site fica em um único arquivo, o `index.html`, feito com HTML, CSS e JavaScript puros. Não há build, framework nem dependências para instalar.

## Conteúdo da página

| Seção | O que mostra |
|---|---|
| **Hero** | Título "PORTFÓLIO", foto e área de atuação |
| **About** (`#about`) | Apresentação pessoal e foto |
| **Trajetória** (`#experiencia`) | Contatos, QR Code do currículo, mural Memories, experiência profissional e hard skills |
| **Projects** (`#cases`) | Cards dos projetos, com links para o produto e para o GitHub |
| **Certifications** | Mosaico de certificados, ampliáveis ao clicar |
| **Rodapé** | Redes sociais e direitos autorais |

## Como executar

Basta abrir o `index.html` no navegador. Para testar como em um site publicado, sirva a pasta localmente:

```bash
python -m http.server 8765
```

Depois, acesse `http://localhost:8765`. É preciso estar conectado à internet, porque fontes, ícones e imagens são carregados de servidores externos.

Para publicar, envie o `index.html` e a pasta `certificados/` para qualquer hospedagem de arquivos estáticos, como Vercel, Netlify ou GitHub Pages. Não há etapa de build.

## Estrutura do `index.html`

```text
<head>
├── Fontes e ícones, carregados sem bloquear a página
├── Script que adiciona as classes .js e .is-loading ao <html>
└── <style>
    ├── 1. Design system (variáveis CSS)
    ├── Estilos de cada seção da página
    ├── 2. Splash screen
    ├── 3. Motion: scroll reveal
    ├── 4. Fotos: duotone + glitch
    └── Acessibilidade, impressão e responsividade

<body>
├── Splash screen, com script próprio
├── Filtros SVG (#fx-duotone, #fx-rgb-lg, #fx-rgb-sm)
├── <header>, <main> e <footer>
├── Lightbox para ampliar imagens
└── <script> principal
    ├── Lightbox
    ├── Camadas de efeito das fotos
    ├── Scroll reveal
    └── Botões magnéticos e gradiente dos cards
```

## Design system

As cores ficam em variáveis CSS no `:root`. Para alterar uma cor, mude a variável em vez de editar cada regra.

| Variável | Cor | Uso |
|---|---|---|
| `--accent-yellow` | `#ffcc00` | Fundo da página (`--page-bg`) e destaques sobre superfícies escuras |
| `--bg-base` | `#11012e` | Fundo da splash e texto sobre o rosa |
| `--surface` | `#2a0c62` | Cards, containers e sombras do duotone |
| `--accent-pink` | `#ff018f` | Chamadas para ação, bordas ativas e detalhes neon |
| `--text-main` | muda com o contexto | Texto principal |
| `--text-muted` | muda com o contexto | Texto secundário |

O texto muda de cor conforme o fundo, porque `--text-main`, `--text-muted`, `--line-color` e `--highlight` são redefinidas por contexto:

| Contexto | Onde se aplica | `--text-main` | `--text-muted` | `--highlight` |
|---|---|---|---|---|
| **Página** | `body` | `#11012e` | `#2a0c62` | `#2a0c62` |
| **Superfície escura** | Splash, lightbox, cards de projetos, contatos, Memories e pills | `#ffffff` | `#b8b0d8` | `#ffcc00` |

Para deixar um novo elemento escuro com texto claro, adicione o seletor dele ao bloco "Contexto das superfícies escuras", logo após o `:root`.

Alguns valores derivam das cores da paleta, como `--glow-pink` e `--glow-yellow`. Os contornos dos avatares (`--sticker-*`) e os filtros SVG repetem os valores das cores, portanto precisam ser atualizados à mão se a paleta mudar.

**Tipografia:** Fredoka nos títulos e Outfit no corpo do texto, ambas do Google Fonts. Os ícones são do Font Awesome 6.4 Free.

## Efeitos visuais

### Splash screen

O nome "LEÔNIDAS" é preenchido de rosa a amarelo, com fatias de glitch por cima. A saída combina cortina, fade e scale-up em 1,2s, com a curva `cubic-bezier(0.77, 0, 0.175, 1)`.

A splash fica na tela por no mínimo 1,5s e sai quando a página termina de carregar. Em redes lentas, ela sai após no máximo 4s. Esses tempos são as constantes `MIN_VISIBLE` e `MAX_VISIBLE`, no script logo após a splash.

### Scroll reveal

Os elementos surgem com fade e deslocamento vertical ao entrar na tela, detectados por `IntersectionObserver`. A lista de elementos animados fica em `REVEAL_SELECTOR`.

### Micro-interações

- **Botões magnéticos:** os links dos projetos e os ícones do rodapé seguem levemente o cursor. A intensidade é definida em `MAGNET_STRENGTH`.
- **Cards de projetos:** um gradiente acompanha o cursor, com borda rosa e brilho neon no hover.
- **Links e títulos:** a cor muda com transição suave.

### Efeito nas fotos

No estado normal, as fotos ficam em duotone, com roxo nas sombras e rosa nas luzes. Ao passar o mouse:

1. O duotone se desfaz e revela as cores originais, com saturação aumentada.
2. Um deslocamento real dos canais de cor (RGB split) é ativado por 0,4s.
3. Uma borda fina em gradiente animado de rosa e amarelo aparece ao redor da imagem.

A técnica muda conforme o tipo de imagem:

| Tipo | Onde aparece | Técnica |
|---|---|---|
| **Fotos retangulares** | Projetos, Memories e certificados | Camadas com `mix-blend-mode` (`multiply` e `lighten`) sobre a imagem em preto e branco |
| **Avatares recortados** | Hero e About | Cópia da imagem com filtro SVG, porque uma camada de blend pintaria um retângulo sobre o fundo transparente |

Nos avatares, a borda é um contorno que acompanha a silhueta. O QR Code e os logos ficam de fora do efeito de propósito, para continuarem escaneáveis e legíveis.

## Como atualizar o conteúdo

As imagens de projetos e do mural Memories são carregadas por URL externa, a maioria de `clareia1.vercel.app`. Hospede a imagem nova e use a URL dela no HTML. Os certificados ficam na pasta `certificados/`.

**Novo projeto:** copie um bloco `.case-card-ed` dentro de `.cases-editorial-grid`:

```html
<div class="case-card-ed">
    <a href="URL_DO_PRODUTO" target="_blank" class="case-img-wrapper">
        <img loading="lazy" decoding="async" src="URL_DA_IMAGEM" alt="Prévia do projeto">
    </a>
    <div class="case-content">
        <h4>Nome do projeto</h4>
        <div class="case-techs">
            <span>Tecnologia</span>
        </div>
        <p>Descrição do projeto.</p>
        <div class="case-links-sm">
            <a href="URL_DO_PRODUTO" target="_blank"><i class="fa-solid fa-globe"></i> Produto</a>
            <a href="URL_DO_GITHUB" target="_blank"><i class="fa-brands fa-github"></i> GitHub</a>
        </div>
    </div>
</div>
```

**Nova foto no mural Memories:** dentro de `.memory-grid`:

```html
<div class="memory-pin-wrapper">
    <div class="pin"></div>
    <img loading="lazy" decoding="async" src="URL_DA_FOTO" class="memory-img" alt="Descrição da foto">
    <img loading="lazy" decoding="async" src="URL_DO_LOGO" class="memory-logo" alt="Logo da instituição">
</div>
```

**Novo certificado:** converta o arquivo para imagem (WebP com até 1400px de largura), salve em `certificados/` e adicione o bloco dentro de `.cert-grid`, informando as dimensões reais da imagem. Os certificados estão ordenados do mais recente para o mais antigo.

```html
<div class="cert-card">
    <img loading="lazy" decoding="async" src="certificados/nome-do-certificado.webp" width="1400" height="990" alt="Certificado: Nome do certificado">
    <span>Nome do certificado</span>
</div>
```

> **Atenção aos dados pessoais:** antes de publicar, cubra CPF, data de nascimento e dados parecidos na imagem. O site e o repositório são públicos. O `.gitignore` impede o envio de PDFs e PNGs da pasta principal, onde ficam os originais.

Imagens adicionadas nesses três lugares recebem o efeito de duotone automaticamente. As do mural e dos certificados também abrem no lightbox ao clicar.

Para tirar um grupo de imagens do duotone, como os certificados, remova o seletor correspondente da lista `.case-img-wrapper img, .memory-img, .cert-card img` no script principal.

## Desempenho e acessibilidade

- **Carregamento:** fontes e ícones não bloqueiam a primeira pintura, e as imagens abaixo da dobra usam `loading="lazy"`.
- **Toque:** sem hover, a foto se revela ao cruzar o centro da tela.
- **Teclado:** os links têm indicador de foco visível, e as imagens dos projetos também se revelam ao receber foco.
- **Movimento reduzido:** com `prefers-reduced-motion`, o scroll reveal e os efeitos de ponteiro são desativados, e as demais animações terminam imediatamente.
- **Sem JavaScript:** a página continua visível, sem a splash e sem os efeitos das fotos.

## Compatibilidade

O layout e os efeitos foram testados no Chromium, em desktop e em 375px de largura. Safari e Firefox ainda não foram testados. Em navegadores sem suporte a `@property`, a borda das fotos aparece, mas sem girar.

## Contato

- **LinkedIn:** [in/leonidasprof](https://www.linkedin.com/in/leonidasprof/)
- **GitHub:** [leonidasprof](https://github.com/leonidasprof)
- **E-mail:** [inscrofat@gmail.com](mailto:inscrofat@gmail.com)

---

© 2026 Leônidas Leandro. Todos os direitos reservados.
