# Alertas e Roadmap

## Fluxo de alertas (v1 — e-mail)
1. Uma rotina diária roda no horário configurado.
2. Para cada remédio ativo (não descartado), calcula `dias_restantes = data_vencimento - hoje`.
3. Se `dias_restantes` for igual a (ou menor que) uma antecedência configurada **e** esse alerta ainda não foi enviado, envia o e-mail e registra em AlertaEnviado.
4. Vários remédios no mesmo dia vão em **um único e-mail** resumido.

## Exemplo de e-mail
**Assunto:** Remédios vencendo — 2 remédios precisam de atenção

- Dipirona 500 mg — vence em 15 dias (25/11/2026)
- Amoxicilina — **vence hoje**

## Evolução futura
1. Push no celular (novo canal, mesma lógica de alertas)
2. Escanear código de barras para preencher o nome
3. Foto da caixa/data de vencimento no cadastro
4. Alertas por remédio (sobrescrevendo o padrão global)
5. Compartilhar com familiares

## Decisões tomadas
- Frontend: **Preact**
- E-mail: **Email Sending do Cloudflare** (plano B: Resend, trocando só a implementação do `EmailNotificador`)
- Volume e uso: até ~20 remédios, uso pessoal (um único usuário)
- Sem carga inicial: os remédios são cadastrados pela tela

## Perguntas em aberto
Nenhuma. A especificação está pronta para o plano de implementação.
