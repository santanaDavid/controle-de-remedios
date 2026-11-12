# Arquitetura Web (Cloudflare)

## Decisão
Hospedar tudo no **Cloudflare Workers**, no plano gratuito: um único Worker serve o app, expõe a API e executa a rotina diária de alertas. Sem servidor para manter e sem cartão de crédito no plano gratuito.

## Componentes

| Camada | Escolha | Observação |
|--------|---------|------------|
| App + API | **Cloudflare Worker** (TypeScript) | Um projeto só; frontend servido como arquivos estáticos do próprio Worker |
| Frontend | **Preact** (TypeScript), Tailwind, mobile-first, **PWA** | Instalável no celular; pronto para push depois |
| Banco | **Cloudflare D1** (SQLite gerenciado) | Segue o modelo de dados de `03-modelo-de-dados.md` |
| Rotina diária | **Cron Trigger** (`scheduled()` do Worker), 1x/dia | Plano gratuito permite 5 por conta; usamos 1 |
| Login | **Cloudflare Access** (gratuito para poucos usuários) | Protege o app inteiro, sem escrever autenticação |
| E-mail | **Email Sending do Cloudflare** para endereço verificado (seu próprio e-mail); plano B: Resend | Ver "Ponto de atenção: e-mail" |
| Push (futuro) | Web Push (service worker do PWA) como novo `Notificador` | Mais trabalho manual que FCM |

## Telas
Descritas em `10-telas.md` (layout responsivo e componentes em `09-identidade-visual.md`).

## Estrutura de módulos
```
src/
  index.ts          entrada do Worker: fetch() (API) e scheduled() (cron)
  api/              rotas: remédios, configurações
  domain/           regras: status do remédio, cálculo de alertas
  notifiers/        interface Notificador + EmailNotificador (PushNotificador no futuro)
  db/               schema SQL e migrações do D1
  jobs/             verificarAlertas() chamado pelo scheduled()
  *.test.ts         testes ao lado do código (node:test, sem dependências)
web/                frontend Preact (PWA: manifest + service worker); build em dist/client
wrangler.jsonc      bindings (D1, cron, assets)
```

## Fluxo diário
1. O Cron Trigger dispara `scheduled()` no horário configurado (em UTC; ajustar para 08:00 America/Sao_Paulo = 11:00 UTC).
2. `verificarAlertas()` consulta o D1: remédios ativos cujo `dias_restantes` bate com uma antecedência configurada e que ainda não constam em `AlertaEnviado`.
3. Agrupa tudo em **um e-mail** resumido, envia pelo `Notificador` e grava em `AlertaEnviado`.

## Decisões-chave
- **Notificador como interface**: a regra de alertas não sabe se o envio é e-mail ou push (RNF04).
- **Idempotência**: rodar a rotina duas vezes no mesmo dia não duplica e-mails (RF10).
- **Datas sem fuso**: vencimento é uma data (sem hora), comparada no fuso America/Sao_Paulo.
- **Rotina leve**: o plano gratuito limita o tempo de CPU por execução (cerca de 10 ms); consultas simples e sem processamento pesado.
- **Backup**: exportar o D1 periodicamente (`wrangler d1 export`); o D1 também tem restauração dos últimos dias (Time Travel).

## Limites do plano gratuito (conferir na documentação antes de implantar)
| Recurso | Limite gratuito | Uso esperado |
|---------|-----------------|--------------|
| Requisições do Worker | 100 mil/dia | Poucas dezenas |
| Cron Triggers | 5 por conta | 1 |
| D1 leituras / escritas | 5 milhões / 100 mil por dia | Muito abaixo |
| CPU por invocação | ~10 ms | Rotina enxuta |

## Ponto de atenção: e-mail
- O envio para destinatários quaisquer exige plano pago; envio para **endereços verificados** é gratuito. Como o lembrete vai só para o próprio usuário, deve bastar.
- O serviço de e-mail do Cloudflare ainda está em beta: **conferir a página de preços e a cota diária** de contas novas antes de depender dele.
- **Plano B:** chamar um provedor externo (ex.: Resend) por `fetch` a partir do Worker, trocando apenas a implementação de `EmailNotificador`.

## Alternativas consideradas
- **Firebase (Blaze)**: tem FCM pronto para push, mas exige cartão de crédito e cobrança por uso.
- **n8n como agendador**: exigiria hospedar o n8n (VPS ou plano pago do n8n Cloud).
- **Next.js + SQLite em VPS**: mais controle, mas é preciso manter servidor.
