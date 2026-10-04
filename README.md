# Don Toro Steak & Beer

Site institucional de página única da Don Toro Steak & Beer — açougue premium e steakhouse em Joinville/SC.

**Demo:** abra `index.html` direto no navegador. Não há build, bundler nem dependências de instalação.

---

## Como rodar

```bash
git clone git@github.com:ghastly0301/don-toro.git
cd don-toro
open index.html          # macOS
# ou sirva localmente, se preferir:
python3 -m http.server 8000
```

## Estrutura

O site inteiro é um único arquivo `index.html` com CSS e JavaScript embutidos. Essa decisão é intencional: a página precisa abrir sem servidor, sem etapa de build e sem dependências externas além das fontes do Google.

```
index.html     # página completa: markup + <style> + <script>
README.md
```

Os únicos recursos externos são as famílias do Google Fonts (Cormorant Garamond, Archivo e Spline Sans Mono). Todos os gráficos — coroa, mascote, caneca, ícones, mapa e fumaça — são SVG inline. Não há imagens binárias no repositório.

## Design tokens

A paleta e as fontes estão declaradas no bloco `:root` no topo do `<style>`. Alterar a identidade visual começa por ali; nenhuma cor é escrita diretamente nas regras de componente.

| Token | Valor | Uso |
| --- | --- | --- |
| `--breu` | `#0B0908` | fundo principal |
| `--carvao` | `#16120E` | seções alternadas |
| `--couro` | `#221B14` | cards |
| `--linha` | `#3A2F23` | bordas e filetes em repouso |
| `--ouro` | `#C9A24B` | cor da casa: filetes, CTAs, ícones |
| `--ouro-claro` | `#E9D5A1` | estados de hover e destaque |
| `--marfim` | `#F3EBDC` | texto principal |
| `--osso` | `#A99F8C` | texto secundário |
| `--brasa` | `#9C3B22` | acento raro: tags "mais pedido", status fechado |

O site é deliberadamente de tema único (escuro), com `color-scheme: dark` em `:root`.

## Seções

`#hero` · `#casa` · `#cortes` · `#ritual` · `#chopp` · `#kits` · `#eventos` · `#avaliacoes` · `#visita`

## JavaScript

Todo o comportamento vive num IIFE no fim do arquivo, sem bibliotecas. São cinco responsabilidades:

- **Status aberto/fechado** — calcula pelo relógio local contra a tabela `HORAS` (índice = `getDay()`, domingo = 0) e revalida a cada minuto. Atualiza o chip do header, o badge da seção Visita e o destaque da linha de hoje na tabela de horários.
- **Menu mobile** — alterna o atributo `hidden` do painel, trava o scroll em `documentElement` e fecha com `Esc`, devolvendo o foco ao botão.
- **Carrossel de depoimentos** — troca a cada 7s, pausa em hover e foco, navegável por teclado.
- **Revelações por scroll** — um `IntersectionObserver` aplica a classe `.in` nos elementos `.reveal`; outro controla o header, e outros dois o botão flutuante de WhatsApp.
- **Copiar telefone** — usa `navigator.clipboard` com fallback de seleção de texto.

### Degradação

Os estados iniciais ocultos ficam todos atrás da classe `.js`, que só é adicionada quando `IntersectionObserver` existe. Sem JavaScript, a página renderiza completa e legível: as citações empilham, a caneca aparece cheia e nada fica invisível. Ao editar, mantenha essa regra — qualquer `opacity: 0` ou `transform` inicial precisa do prefixo `.js`.

`prefers-reduced-motion: reduce` desliga marquee, parallax, fumaça, brasa e o autoplay do carrossel, reduzindo as transições a 0,01ms sem esconder conteúdo.

## Regras de conteúdo

O conteúdo foi levantado das fontes oficiais da casa (Instagram, Linktree, site antigo, Google e TripAdvisor). Três regras valem para qualquer edição:

1. **Nenhum preço no site.** Preços e disponibilidade são direcionados ao WhatsApp, para não publicar valor desatualizado.
2. **Depoimentos apenas reais.** As quatro citações da seção de avaliações são transcrições de avaliações públicas do Google e do TripAdvisor. Não invente nem parafraseie a ponto de mudar o sentido.
3. **Sem promessas não verificadas.** Rendimento de kits, dias de maturação e composições exatas só entram no site depois de confirmados com a casa.

### Dados da casa

| | |
| --- | --- |
| Endereço | R. Max Colin, 1195 — América, Joinville/SC, 89204-041 |
| Telefone | (47) 3030-2021 |
| Horários | Seg 9h–19h · Ter a Sex 9h–23h · Sáb e Dom 9h–17h |
| Steakhouse | Qui e sex a partir das 19h · sáb a partir das 11h |
| Instagram | [@_dontoro](https://www.instagram.com/_dontoro/) |
| Links | [linktr.ee/dontorolinks](https://linktr.ee/dontorolinks) |

Alterar horários exige mudar dois lugares: a constante `HORAS` no JavaScript e a tabela em `#visita`.

## Pendências

- [ ] Substituir os slots marcados com `<!-- FOTO FUTURA -->` por fotos reais da casa (hero e seção de cortes).
- [ ] Confirmar a sétima bandeja do Kit Família — a fonte lista seis dos sete itens.
- [ ] Confirmar a grafia do rótulo da lager própria de 555 ml.
- [ ] Apontar o domínio `dontoro.com.br` para esta versão (o site atual está com certificado SSL expirado).
