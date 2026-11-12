# Identidade visual — Cadastro de remédios vencidos

Baseada na imagem de referência (fluxo de reserva em app mobile, tema escuro com azul elétrico). Os valores de cor são aproximações visuais da imagem; ajuste finos podem ser feitos depois sem mudar a estrutura.

> Convenção do projeto: usar sempre **"remédio"** nos textos da interface (nunca "medicamento" ou "substância").

---

## 1. Princípios

1. **Escuro e sóbrio.** Fundo quase preto, superfícies em cinza muito escuro, bordas finas. O conteúdo é o destaque.
2. **Um único acento.** O azul elétrico é a cor de ação e de seleção. Todo o resto é neutro.
3. **Status por cor, nunca só por cor.** Verde, âmbar e vermelho sempre vêm acompanhados de texto ("Vence em 12 dias").
4. **Uma ação principal por tela.** Botão azul largo, fixo na parte de baixo.
5. **Rótulos em caixa-alta, pequenos e espaçados.** Títulos de cabeçalho, seções e botões seguem esse estilo.

---

## 2. Cores

### 2.1 Neutros (tema escuro)

| Token | Hex | Uso |
|---|---|---|
| `--bg` | `#08090A` | Fundo da aplicação |
| `--surface` | `#101214` | Cards, listas, campos |
| `--surface-2` | `#16191D` | Card elevado, item selecionado neutro |
| `--surface-3` | `#222A3D` | Círculos de dia no calendário (dias disponíveis) |
| `--border` | `#23272D` | Bordas de 1px de cards e campos |
| `--border-strong` | `#323843` | Borda de chips e botões secundários |
| `--text` | `#F5F7FA` | Texto principal |
| `--text-muted` | `#8B919A` | Rótulos, textos de apoio |
| `--text-faint` | `#555B64` | Placeholder, itens desabilitados |

### 2.2 Acento (azul)

| Token | Hex | Uso |
|---|---|---|
| `--primary` | `#1766FF` | Botão principal, chip selecionado, dia selecionado |
| `--primary-hover` | `#2F78FF` | Hover/foco |
| `--primary-pressed` | `#0F52D6` | Pressionado |
| `--primary-soft` | `rgba(23,102,255,0.16)` | Fundo de destaque suave, ícone em círculo |
| `--on-primary` | `#FFFFFF` | Texto sobre azul |

### 2.3 Status de validade

Verde e âmbar aparecem na imagem ("Tables Left" e "Limited"). O vermelho é uma extensão necessária para "vencido".

| Status | Token | Hex | Texto na interface |
|---|---|---|---|
| Dentro da validade | `--status-ok` | `#3FB36B` | "Válido" / "Vence em N dias" |
| Vencendo (alerta) | `--status-warn` | `#E3B341` | "Vence em N dias" |
| Vencido | `--status-expired` | `#F0545B` | "Vencido há N dias" |
| Sem data de validade | `--status-unknown` | `#8B919A` | "Sem validade" |

Fundos suaves dos status: mesma cor com 14% de opacidade.

---

## 3. Tipografia

- **Família:** `Inter`, com fallback `system-ui, -apple-system, "Segoe UI", sans-serif`.
- **Números:** usar `font-variant-numeric: tabular-nums` em datas e contagens.

| Estilo | Tamanho / peso | Observação |
|---|---|---|
| `display` | 48 / 600 | Número grande (ex.: contagem de dias, como o "4" da imagem) |
| `title` | 20 / 600 | Nome do remédio na tela de detalhe |
| `body` | 15 / 500 | Itens de lista e valores |
| `body-muted` | 14 / 400 | Textos de apoio |
| `label` | 11 / 600, caixa-alta, `letter-spacing: 0.08em` | Títulos de cabeçalho, rótulos de seção, textos de botão |
| `caption` | 12 / 500 | Status à direita nas listas, contadores (ex.: "0/200") |

---

## 4. Forma, espaço e elevação

- **Raio:** `--radius-sm: 12px` (campos), `--radius-md: 16px` (cards), `--radius-lg: 24px` (painéis grandes), `--radius-pill: 999px` (botões, chips, itens de lista selecionáveis).
- **Espaçamento:** escala de 4px (`4, 8, 12, 16, 24, 32`). Margem lateral de tela: `20px`.
- **Bordas:** 1px sólidas em `--border`. Nada de sombras pesadas; separação vem de borda e contraste de superfície.
- **Foco/seleção:** borda de 1.5px em `--primary` mais `--primary-soft` de fundo.
- **Fundo decorativo (opcional):** degradê radial azul bem suave atrás do conteúdo, só em telas de abertura. Telas de uso diário ficam em `--bg` liso.

---

## 5. Ícones

- Estilo **linha** (outline), traço de 1.5px, cantos arredondados, tamanho 20px (24px em destaque).
- Cor padrão `--text-muted`; ativos em `--text` ou `--primary`.
- Sugestão de biblioteca: Lucide (tem `pill`, `calendar`, `bell`, `mail`, `arrow-left`, `arrow-right`, `plus`, `minus`, `trash`).
- Ícone de destaque dentro de círculo com `--primary-soft` (como o avatar azul da tela de convidados).

---

## 6. Componentes

### 6.1 Cabeçalho de tela
Seta de voltar à esquerda e título em `label` (caixa-alta, `--text-muted`) ao lado. Hora/status do sistema não fazem parte do app web.

### 6.2 Botão principal
- Largura total, altura 56px, `--radius-pill`, fundo `--primary`.
- Texto em `label` (caixa-alta) à esquerda, seta `→` à direita.
- Fixo no rodapé da tela, com 20px de margem.
- Exemplos de texto: `SALVAR REMÉDIO`, `CONTINUAR`, `CONFIRMAR`.
- Desabilitado: fundo `--surface-2`, texto `--text-faint`.

### 6.3 Botão secundário
Mesma forma, fundo transparente, borda `--border-strong`, texto `--text`.

### 6.4 Chips (opções)
- Pílula com borda `--border-strong`, texto `label` em `--text`.
- Selecionado: fundo `--primary`, sem borda, texto branco.
- Uso: escolher antecedência dos alertas (`30 DIAS`, `15 DIAS`, `NO DIA`), filtros da lista (`TODOS`, `VENCENDO`, `VENCIDOS`).

### 6.5 Lista de itens
- Linha em pílula/retângulo arredondado com borda `--border`, altura 52–60px.
- Esquerda: nome do remédio (`body`). Direita: status em `caption` com a cor do status.
- Item em destaque/selecionado: fundo `--primary`, texto branco.
- Sob o nome, em `body-muted`: "Vence em 15/03/2027".

### 6.6 Calendário (data de compra e de vencimento)
- Cabeçalho `MÊS ANO` em `label`; dias da semana em `caption` `--text-faint`.
- Dias disponíveis: círculo `--surface-3`. Dia selecionado: círculo `--primary`. Dias fora do mês ou indisponíveis: apenas número em `--text-faint`.
- Navegação entre meses por botão circular com seta.

### 6.7 Contador (stepper)
Número grande (`display`) ao centro com botões circulares `−` e `+` de borda fina. Texto de apoio abaixo em `body-muted`. Uso possível: quantidade de unidades na caixa.

### 6.8 Campos de texto
- Fundo `--surface`, borda `--border`, raio 12px, altura 52px.
- Rótulo acima em `label` `--text-muted`; placeholder em `--text-faint`.
- Área de texto com contador no canto inferior direito (ex.: `0/200`) para observações.
- Foco: borda `--primary`.

### 6.9 Card de resumo
Card grande com linhas `ícone + rótulo` à esquerda e valor em negrito à direita (Data da compra, Vencimento, Alertas). Usado na tela de revisão antes de salvar.

### 6.10 Selo de status
Pílula pequena com texto do status e fundo suave da cor correspondente. Sempre com texto, nunca só um ponto colorido.

---

## 7. Aplicação nas telas do app

| Tela | Composição |
|---|---|
| **Lista de remédios** | Cabeçalho `MEUS REMÉDIOS`, chips de filtro, lista com status à direita, botão principal `ADICIONAR REMÉDIO` fixo no rodapé. Ordenar por vencimento mais próximo. |
| **Cadastro** | Campos de nome e observação, calendário para data da compra e para data de vencimento, opção "Sem data de validade". Botão `CONTINUAR`. |
| **Alertas** | Chips de antecedência (`30 DIAS`, `15 DIAS`, `NO DIA`), múltipla seleção. Mostra o e-mail de destino. |
| **Revisão** | Card de resumo e botão `SALVAR REMÉDIO`. |
| **Detalhe** | Nome em `title`, selo de status, contagem de dias em `display`, ações `EDITAR` e `DESCARTAR`. |

---

## 8. Tokens prontos (CSS)

```css
:root {
  color-scheme: dark;

  /* neutros */
  --bg: #08090A;
  --surface: #101214;
  --surface-2: #16191D;
  --surface-3: #222A3D;
  --border: #23272D;
  --border-strong: #323843;
  --text: #F5F7FA;
  --text-muted: #8B919A;
  --text-faint: #555B64;

  /* acento */
  --primary: #1766FF;
  --primary-hover: #2F78FF;
  --primary-pressed: #0F52D6;
  --primary-soft: rgba(23, 102, 255, 0.16);
  --on-primary: #FFFFFF;

  /* status */
  --status-ok: #3FB36B;
  --status-warn: #E3B341;
  --status-expired: #F0545B;
  --status-unknown: #8B919A;

  /* forma */
  --radius-sm: 12px;
  --radius-md: 16px;
  --radius-lg: 24px;
  --radius-pill: 999px;

  /* espaço */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 24px;
  --space-6: 32px;
  --screen-margin: 20px;

  /* tipografia */
  --font: "Inter", system-ui, -apple-system, "Segoe UI", sans-serif;
}

body {
  margin: 0;
  background: var(--bg);
  color: var(--text);
  font-family: var(--font);
  font-size: 15px;
  font-weight: 500;
}

.label {
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--text-muted);
}

.btn-primary {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  height: 56px;
  padding: 0 24px;
  border: 0;
  border-radius: var(--radius-pill);
  background: var(--primary);
  color: var(--on-primary);
  font: 600 12px/1 var(--font);
  letter-spacing: 0.08em;
  text-transform: uppercase;
}
.btn-primary:hover { background: var(--primary-hover); }
.btn-primary:active { background: var(--primary-pressed); }
.btn-primary:disabled { background: var(--surface-2); color: var(--text-faint); }

.chip {
  padding: 10px 18px;
  border: 1px solid var(--border-strong);
  border-radius: var(--radius-pill);
  background: transparent;
  color: var(--text);
  font: 600 11px/1 var(--font);
  letter-spacing: 0.08em;
  text-transform: uppercase;
}
.chip[aria-pressed="true"] { background: var(--primary); border-color: var(--primary); color: var(--on-primary); }

.card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius-md);
  padding: var(--space-4);
}
```

---

## 9. Acessibilidade

- Contraste mínimo de 4.5:1 para texto. `--text-faint` serve só para placeholder e itens desabilitados.
- Alvos de toque de pelo menos 44px.
- Status sempre com texto e ícone, nunca só cor.
- Respeitar `prefers-reduced-motion`; animações curtas (150–200ms) apenas em seleção e transição de tela.
- Foco visível em todos os elementos interativos (anel de 2px em `--primary`).

---

## 10. Tom de voz dos textos

- Direto e curto, em português do Brasil, segunda pessoa informal ("Seu remédio vence em 15 dias").
- Sem alarmismo. Para vencido: "Vencido há 30 dias. Descarte com segurança."
- Sempre "remédio", conforme o glossário do projeto.
