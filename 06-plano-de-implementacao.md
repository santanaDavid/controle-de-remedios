# Plano de Implementação

Stack: Cloudflare Workers + D1 + Cron Trigger + Access, frontend em Preact (PWA).
Cada etapa termina com algo que dá para rodar e testar. Os IDs entre parênteses referem-se a `02-requisitos.md`.

## Visão geral

| # | Etapa | Entrega |
|---|-------|---------|
| 0 | Preparação e verificações | Projeto criado, ambiente local rodando, pré-requisitos confirmados |
| 1 | Banco e regras de domínio | Esquema do D1 e funções de status/alertas, com testes |
| 2 | API | CRUD de remédios e configurações |
| 3 | Frontend | Telas de lista, cadastro/edição e configurações |
| 4 | Rotina de alertas | `scheduled()` com idempotência, usando notificador de teste |
| 5 | E-mail real | Envio pelo Email Sending (plano B: Resend) |
| 6 | Segurança e deploy | App em produção protegido por login e cron ativo |
| 7 | PWA e validação final | App instalável no celular e teste ponta a ponta |

## Etapa 0 — Preparação e verificações
**Tarefas**
- Criar a conta Cloudflare, instalar Node e o `wrangler`, criar o repositório.
- Criar o projeto (Worker + Vite + Preact + TypeScript + Tailwind) com `wrangler.jsonc`.
- Criar o banco D1 (local e remoto) e configurar Vitest.
- **Verificar os pré-requisitos antes de seguir** (são os maiores riscos):
  - O Email Sending exige um domínio na sua conta Cloudflare? Como verificar o seu e-mail de destino?
  - O Cloudflare Access (Zero Trust) pede cartão de crédito para o plano gratuito?
  - Cota diária inicial de e-mails de contas novas.

**Pronto quando:** `wrangler dev` sobe o app com uma página de teste e os três pré-requisitos estão respondidos.
**Decisão se algo falhar:** usar Resend para e-mail e/ou autenticação simples própria no lugar do Access.

## Etapa 1 — Banco e regras de domínio
**Tarefas**
- Migrações do D1 conforme `03-modelo-de-dados.md`: `remedio`, `configuracao_alerta`, `usuario`, `alerta_enviado`.
- Configuração inicial: alertas 30, 15 e 0 dias, horário 08:00 (RF08).
- `domain/status.ts`: Válido / Vencendo / Vencido (RF05).
- `domain/alertas.ts`: dado hoje (fuso America/Sao_Paulo), remédios e configurações, retorna os alertas devidos e ainda não enviados (RF09, RF10, RN02–RN05).
- Testes unitários cobrindo: vencimento hoje, em 15 dias, já vencido no cadastro, data alterada reinicia alertas, descartado não alerta, sem duplicidade.

**Pronto quando:** testes de domínio passam sem depender de banco real.

## Etapa 2 — API
**Tarefas**
- Rotas: listar (com busca e filtro por status), criar, editar, excluir e descartar remédio (RF01–RF04, RF06, RF07).
- Rotas: ler e alterar antecedências de alerta e e-mail de destino (RF08, RF11).
- Validação dos dados (obrigatórios; advertência, sem bloquear, se vencimento < compra, RN01).
- Alterar `data_vencimento` apaga os envios anteriores daquele remédio (RN03).

**Pronto quando:** todas as rotas respondem corretamente em testes de integração locais.

## Etapa 3 — Frontend (Preact)
**Tarefas**
- **Lista**: ordenada por vencimento, cores por status, busca e filtro (RF04–RF06).
- **Cadastro/edição**: nome, datas, campos opcionais; ação de descartar.
- **Configurações**: e-mail de destino, antecedências e horário.
- Layout mobile-first, textos em português do Brasil.

**Pronto quando:** dá para cadastrar, editar, descartar e configurar tudo pelo navegador, no celular e no computador.

## Etapa 4 — Rotina de alertas
**Tarefas**
- Interface `Notificador` e `NotificadorDeRegistro` (só grava em log, para testes).
- `jobs/verificarAlertas()`: busca devidos, agrupa em **um e-mail** resumido, envia e registra em `alerta_enviado` (RF09, RF10).
- Ligar ao `scheduled()` do Worker e testar localmente com `wrangler dev --test-scheduled`.
- Cuidar do fuso: o cron roda em UTC (08:00 Brasília = 11:00 UTC).

**Pronto quando:** rodar a rotina duas vezes seguidas gera os alertas uma única vez.

## Etapa 5 — E-mail real
**Tarefas**
- Implementar `EmailNotificador` com o Email Sending e enviar um e-mail de teste ao seu endereço verificado.
- Montar o corpo conforme o exemplo de `04-alertas-e-roadmap.md` (assunto com quantidade de remédios, lista com dias restantes).
- Tratar falha de envio: não registrar `alerta_enviado` se o envio falhar, para tentar de novo no dia seguinte.
- Se houver bloqueio (domínio, cota ou beta): trocar para Resend, mudando só esta classe.

**Pronto quando:** você recebe o e-mail de lembrete de verdade com remédios de teste.

## Etapa 6 — Segurança e deploy
**Tarefas**
- Publicar o Worker e o D1 em produção (`wrangler deploy`, migrações remotas).
- Configurar o Cloudflare Access para proteger o app inteiro.
- Configurar o Cron Trigger de produção e os segredos (se houver).
- Rotina de backup: `wrangler d1 export` periódico, guardado fora da Cloudflare.

**Pronto quando:** o app abre apenas depois do login e o cron de produção dispara no horário esperado.

## Etapa 7 — PWA e validação final
**Tarefas**
- `manifest` e service worker; instalar o app na tela inicial do celular.
- Teste ponta a ponta com remédios reais de teste: um vencendo em 30 dias, um em 15, um hoje e um já vencido.
- Conferir o consumo nos limites gratuitos (painel da Cloudflare) e ajustar o que for preciso.
- Cadastrar os remédios reais da gaveta.

**Pronto quando:** você recebe os alertas corretos e o app está em uso no dia a dia.

## Riscos e pontos de atenção
| Risco | Mitigação |
|-------|-----------|
| Email Sending é beta e pode ter cota baixa ou exigir domínio | Verificar na etapa 0; Resend como plano B |
| Cloudflare Access pode exigir cartão para ativar | Verificar na etapa 0; alternativa: login próprio simples |
| Limite de ~10 ms de CPU por execução | Consultas simples; medir na etapa 4 |
| Diferença de fuso (cron em UTC) | Testes de domínio com datas em America/Sao_Paulo |
| Perda de dados | Backup do D1 na etapa 6 |

## Fora do plano (futuro)
Push no celular (novo `PushNotificador` + service worker), leitura de código de barras, alertas por remédio.
