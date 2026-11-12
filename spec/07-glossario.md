# Glossário

Termos oficiais do projeto. Valem para **especificação, telas, e-mails, código e banco de dados**.
Regra de ouro: um conceito, um nome. Se aparecer um termo da coluna "Evitar", troque pelo termo oficial.

## Regras de linguagem
- Texto para o usuário (telas, e-mails): português do Brasil.
- Nomes no código e no banco: português, sem acentos, em `snake_case` para campos e tabelas e `camelCase` para funções (ex.: `data_vencimento`, `verificarAlertas`). Palavras técnicas sem tradução comum (Worker, cron, PWA, push) ficam como estão.
- Datas exibidas ao usuário: `dd/mm/aaaa`. Armazenadas como `aaaa-mm-dd`.

## Termos de negócio

| Termo oficial | Definição | No código / banco | Evitar |
|---------------|-----------|-------------------|--------|
| **Remédio** | Qualquer produto de saúde cadastrado no app (comprimido, xarope, pomada, gotas etc.). É a entidade principal. | `Remedio`, tabela `remedio` | medicamento, medicação, fármaco, droga, substância, produto, item |
| **Cadastro** | Ação e conjunto de dados de registrar um remédio no app. Verbo: *cadastrar*. | `criarRemedio` | registro, inclusão, entrada |
| **Data da compra** | Dia em que o remédio foi comprado. | `data_compra` | data de aquisição, data de entrada |
| **Data de vencimento** | Dia em que o remédio passa a ser impróprio para uso (a data impressa na caixa). | `data_vencimento` | validade, data de expiração, prazo, data limite |
| **Dias restantes** | Dias entre hoje e a data de vencimento. Negativo quando já vencido. Zero no próprio dia. | `diasRestantes` | prazo restante, dias para vencer |
| **Status** | Situação do remédio: **Válido**, **Vencendo** ou **Vencido**. Calculado, nunca digitado. | `status` (`valido`, `vencendo`, `vencido`) | situação, estado |
| **Válido** | Dias restantes maior que a maior antecedência configurada. | `valido` | ok, em dia, bom |
| **Vencendo** | Dias restantes entre 0 e a maior antecedência configurada. | `vencendo` | próximo do vencimento, a vencer |
| **Vencido** | Dias restantes menor que zero. | `vencido` | expirado, fora da validade |
| **Descartar** | Marcar o remédio como jogado fora. Ele sai da lista ativa e para de gerar alertas, mas o registro permanece. | `descartado` (booleano) | baixar, retirar, arquivar |
| **Excluir** | Apagar o cadastro definitivamente. Diferente de descartar. | `excluirRemedio` | deletar, remover, apagar |
| **Remédio ativo** | Remédio não descartado. | `descartado = falso` | em estoque, vigente |
| **Antecedência** | Quantos dias antes do vencimento o usuário quer ser avisado. `0` significa no dia. | `dias_antes` | prazo de aviso, offset, janela |
| **Alerta** | Ocorrência devida de uma antecedência para um remédio (ex.: "Dipirona vence em 15 dias"). É o que o sistema calcula. | `Alerta` | aviso, gatilho |
| **Configuração de alerta** | Lista de antecedências que o usuário definiu (padrão: 30, 15 e 0). | `configuracao_alerta` | regra, preferência |
| **Lembrete** | Mensagem enviada ao usuário contendo um ou mais alertas (e-mail agora, push no futuro). | `Lembrete` | notificação, aviso, mensagem |
| **Alerta enviado** | Registro de que um alerta já foi entregue, para não repetir. | `alerta_enviado` | histórico de envio, log |
| **Canal** | Meio de entrega do lembrete: `email` ou `push`. | `canal` | meio, tipo de envio |
| **Notificador** | Componente de código que entrega um lembrete por um canal. | `Notificador`, `EmailNotificador`, `PushNotificador` | enviador, sender, mailer |
| **Rotina diária** | Execução automática, uma vez por dia, que calcula e envia os alertas. | `verificarAlertas` | job, tarefa agendada, cron (exceto ao falar da tecnologia) |
| **Usuário** | A pessoa que usa o app (única). | `usuario` | cliente, dono, pessoa |
| **Horário de envio** | Hora do dia em que a rotina diária roda (padrão 08:00, horário de Brasília). | `horario_envio` | hora do cron, agendamento |

## Campos do remédio

| Termo oficial | Definição | No código | Evitar |
|---------------|-----------|-----------|--------|
| **Nome** | Nome do remédio (ex.: Dipirona 500 mg). | `nome` | título, descrição |
| **Quantidade** | Quantas unidades há (opcional). | `quantidade` | estoque, qtd |
| **Observações** | Texto livre: marca, dosagem, onde está guardado (opcional). | `observacoes` | notas, comentários, detalhes |

## Termos técnicos

| Termo | Significado no projeto |
|-------|------------------------|
| **App** | O aplicativo web completo (telas + API + rotina diária). |
| **Worker** | O programa na Cloudflare que serve o app, a API e a rotina diária. |
| **D1** | Banco de dados SQLite gerenciado da Cloudflare. |
| **Cron Trigger** | Recurso da Cloudflare que dispara a rotina diária no horário definido (em UTC). |
| **Access** | Cloudflare Access: o login que protege o app. |
| **PWA** | O app web instalável na tela inicial do celular. |
| **Push** | Notificação no celular (versão futura); sempre escrito "push", nunca "notificação". |

## Textos padrão da interface
Tom de voz e estilo em `09-identidade-visual.md`. Rótulos de botões e chips aparecem em caixa-alta.
- Botões principais: **ADICIONAR REMÉDIO**, **SALVAR REMÉDIO**, **EDITAR REMÉDIO**, **SALVAR ALERTAS**.
- Botões secundários: **DESCARTAR**, **EXCLUIR**, **CANCELAR**, **ADICIONAR**.
- Rótulos de campo: **Nome do remédio**, **Data da compra**, **Data de vencimento**, **Quantidade**, **Observações**.
- Status: **Válido**, **Vencendo**, **Vencido** (e **Descartado** para remédios descartados).
- Texto curto do status nas listas: "Válido", "Vence em N dias", "Vence hoje", "Vencido há N dias".
- Linha sob o nome: "Vence em dd/mm/aaaa" ou "Venceu em dd/mm/aaaa".
- Frase do detalhe: "Seu remédio vence em N dias." ou "Vencido há N dias. Descarte com segurança."
- Antecedências: "30 dias", "1 dia", "No dia".
- **Alertas** é o nome da tela de configuração de alertas.
- Mensagem de vencimento no lembrete por e-mail: "vence em N dias (dd/mm/aaaa)", "vence hoje", "venceu em dd/mm/aaaa".

## Como manter este glossário
- Termo novo? Adicione aqui **antes** de usar no código ou na especificação.
- Em revisão de código ou de texto, procure os termos da coluna "Evitar".
