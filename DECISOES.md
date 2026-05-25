# Notas do teste técnico

Documentação das escolhas feitas durante o desenvolvimento. O objetivo é deixar claro o raciocínio, não só o código final.

---

## Exercício 1 — Vídeo na section Imagem com texto

### O que foi feito

- Campo **Tipo de mídia** (imagem ou vídeo) no personalizador.
- Vídeo via picker nativo do Shopify (`type: "video"`).
- Autoplay com `muted: true` (sem isso o browser bloqueia autoplay).
- Lazy load com `preload: 'none'`, `loading: 'lazy'` e um custom element `<lazy-autoplay-video>` que só dá `play()` quando o bloco entra na viewport.

### Por que assim

Reaproveitei o padrão do Dawn: `video_tag` na section de vídeo do tema e a ideia de `DeferredMedia` no `global.js`, mas sem botão de play — aqui o vídeo substitui a imagem e precisa rodar sozinho.

O seletor de tipo de mídia deixa explícito no editor o que está ativo. Quando é vídeo, a imagem não aparece no front (evita confusão), mesmo que ainda exista no schema.

### Limitações

- Vídeo sem áudio por causa do autoplay.
- `LazyAutoplayVideo` foi para o `global.js` (carrega em todas as páginas). Em produção eu avaliaria colocar o script só na section quando `media_type == video`.
- Imagem e vídeo aparecem juntos no customizer; dá para melhorar com `visible_if` numa versão futura.

---

## Exercício 2 — Imagem mobile no Slideshow

### O que foi feito

- Campo **Imagem mobile** em cada slide.
- Desktop: classe `small-hide` (some abaixo de 750px — breakpoint do Dawn).
- Mobile: classe `slideshow__image--mobile` + CSS que esconde a partir de 750px.
- Se mobile estiver vazia, usa a imagem desktop em todos os tamanhos.
- Altura “adaptar à imagem” usa aspect ratio da mobile no mobile e da desktop no desktop.

### Por que assim

Segui o utilitário `small-hide` que o tema já usa em outros lugares, em vez de inventar breakpoint novo. Mobile carrega `width: 1500` (menor que os 3840 do desktop) — arquivo menor no celular.

### Limitações

- As duas `<img>` ainda vão pro HTML; o browser pode baixar as duas. Isso virou o tema do exercício 3 no `PERFORMANCE.md`.
- Só o primeiro slide usa `fetchpriority: high`; os outros já usam `loading: lazy` como no Dawn original.

---

## Exercício 3 — Performance

Escrito em `PERFORMANCE.md`, em tom direto: uma melhoria óbvia ligada ao que implementamos no slideshow (`<picture>` em vez de duas imagens escondidas com CSS).

---

## Template da home

`templates/index.json` ganhou as sections **Imagem com texto** e **Slideshow** para conseguir testar no editor sem procurar outra página.
