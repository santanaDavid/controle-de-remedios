# Telas

Visual definido em `09-identidade-visual.md` (tema escuro, azul elétrico como único acento). Textos conforme `07-glossario.md`.

## Comportamento responsivo

| Largura | Layout |
|---------|--------|
| **Celular** (< 1024 px) | Uma tela por vez: lista -> detalhe, formulário ou alertas, com seta de voltar. Botão principal azul fixo no rodapé. |
| **Tablet** (≥ 768 px) | Mesma navegação do celular; formulários e cards em duas colunas. |
| **Computador** (≥ 1024 px) | Duas áreas lado a lado, cada uma com rolagem própria: **lista de remédios fixa à esquerda** (340 a 420 px) e **conteúdo à direita**. Barra superior com a marca e a navegação (Meus remédios, Alertas). O botão principal sai do rodapé fixo e fica no fluxo da página. |

Limite de largura de 1440 px, centralizado. Rotas por `#`: `#/` lista, `#/novo`, `#/remedio/:id`, `#/editar/:id`, `#/configuracoes`.

## Telas

| Tela | Conteúdo |
|------|----------|
| **Meus remédios** (lista) | Busca por nome; chips `TODOS`, `VENCENDO`, `VENCIDOS`, `VÁLIDOS`, `DESCARTADOS`; itens com nome, "Vence em dd/mm/aaaa" e status à direita ("Válido", "Vence em 12 dias", "Vencido há 30 dias"); no celular, contagem por status no topo; botão `ADICIONAR REMÉDIO`. O item aberto aparece destacado em azul (computador). |
| **Visão geral** (só computador, nada aberto) | Cards grandes com a contagem de vencidos, vencendo e válidos; lista "Precisam de atenção"; resumo dos lembretes (antecedências, e-mail e horário) com atalho para ajustar. |
| **Detalhe do remédio** | Nome, selo de status (texto + ícone), número grande de dias, frase de apoio ("Vencido há 30 dias. Descarte com segurança."), card com datas, quantidade e observações. Ações: `EDITAR REMÉDIO` (principal), `DESCARTAR`, `EXCLUIR`. |
| **Novo / Editar remédio** | Nome, dois calendários (data da compra e data de vencimento, com seleção de mês e ano), contador de quantidade, observações com contador (0/500) e card de **Resumo** (inclui os alertas configurados) antes de `SALVAR REMÉDIO`. Vencimento anterior à compra pede confirmação, sem bloquear. |
| **Alertas** | E-mail de destino, horário de envio e chips de antecedência com seleção múltipla (`60`, `30`, `15`, `7`, `3`, `1 DIA`, `NO DIA`) mais campo para outra antecedência; frase de conferência ("Você será avisado 30 dias, 15 dias e no dia antes do vencimento..."). `SALVAR ALERTAS`. |

## Decisões

- A etapa "Revisão" da identidade visual virou o **card de Resumo** dentro do formulário, em vez de uma tela separada.
- A tela "Alertas" da identidade visual é a **configuração global** de alertas (RF08), não uma configuração por remédio (item de roadmap).
- Ícones de linha embutidos no código (estilo Lucide), sem dependência de pacote.
- Fonte Inter carregada do Google Fonts, com fallback para a fonte do sistema.
- **Fora desta versão:** a opção "Sem data de validade" da identidade visual. Hoje a data de vencimento é obrigatória (RF01). Incluí-la exige tornar o vencimento opcional na API, no banco, no status e nos alertas.

## Acessibilidade

Status sempre com texto e ícone; alvos de toque de 44 px; foco visível (anel azul de 2 px); calendário com botões rotulados por extenso ("12 de novembro de 2026"); animações desligadas com `prefers-reduced-motion`.
