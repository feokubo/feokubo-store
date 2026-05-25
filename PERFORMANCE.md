# Exercício 3 — Melhoria de desempenho

## O que eu mudaria

No slideshow, quando o slide tem **imagem desktop + imagem mobile**, as duas `<img>` vão pro HTML. No celular a desktop some com a classe `small-hide`, mas o navegador ainda pode baixar ela — e no desktop acontece o contrário com a mobile.

Isso ficou evidente no `sections/slideshow.liquid`, onde renderizamos as duas tags:

```liquid
{{ block.settings.image | image_url: width: 3840 | image_tag: class: '... small-hide' }}
{{ block.settings.image_mobile | image_url: width: 1500 | image_tag: class: 'slideshow__image--mobile' }}
```

O Dawn já faz algo parecido no carrossel: o **primeiro slide** carrega com prioridade e os demais usam `loading: 'lazy'`. Dá pra seguir a mesma linha aqui — só colocar no markup a imagem que de fato vai aparecer naquele breakpoint, em vez de esconder com CSS.

A forma mais simples seria usar `<picture>` com `<source media="(min-width: 750px)">` apontando pra desktop e um `<img>` fallback pra mobile. Assim o browser escolhe um arquivo só, sem depender de `display: none`.

Outra opção, mais trabalhosa: duplicar o bloco de imagem no Liquid com `{% if %}` por breakpoint — funciona, mas é mais chato de manter.

## Por que vale a pena

É lazy loading / economia de banda na prática: em mobile o cliente não precisa puxar uma imagem de 3840px que ele nunca vai ver. Em loja com slideshow pesado isso pesa, principalmente em 4G.

Não implementei isso no teste pra não estourar o escopo, mas é a primeira coisa que eu olharia depois de subir a feature de imagem mobile.
