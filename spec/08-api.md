# API

Todas as rotas ficam sob `/api` e usam JSON. Datas são `aaaa-mm-dd`. Termos conforme `07-glossario.md`.
Em produção o acesso é protegido pelo Cloudflare Access (login), antes de chegar ao Worker.

## Formato de erro
```json
{ "erro": "Dados inválidos.", "detalhes": { "nome": "Informe o nome do remédio." } }
```
`detalhes` só aparece em erros de validação (400). Códigos: 400 dados inválidos, 404 não encontrado, 405 método não permitido, 500 erro interno.

## Objeto remédio (resposta)
```json
{
  "id": 1,
  "nome": "Dipirona 500 mg",
  "dataCompra": "2026-01-10",
  "dataVencimento": "2027-06-30",
  "quantidade": null,
  "observacoes": null,
  "descartado": false,
  "diasRestantes": 231,
  "status": "valido"
}
```
`status` (`valido`, `vencendo`, `vencido`) e `diasRestantes` são calculados na hora, com "hoje" no fuso de Brasília e a maior antecedência configurada (RF05).

## Remédios

| Método e rota | O que faz | Requisitos |
|---------------|-----------|------------|
| `GET /api/remedios` | Lista ordenada por vencimento. Parâmetros: `busca` (nome, sem distinguir acentos/caixa), `status` (`valido`/`vencendo`/`vencido`), `descartados=1` (inclui descartados). | RF04, RF05, RF06, RF07 |
| `POST /api/remedios` | Cadastra. Corpo: `nome`, `dataCompra`, `dataVencimento` (obrigatórios); `quantidade`, `observacoes` (opcionais). Responde 201 com o remédio e `advertencias`. | RF01, RF02, RN01 |
| `GET /api/remedios/:id` | Busca um remédio. | |
| `PUT /api/remedios/:id` | Edita (corpo igual ao do cadastro). Mudar a data de vencimento reinicia os alertas daquele remédio. | RF03, RN03 |
| `POST /api/remedios/:id/descartar` | Marca como descartado: sai da lista ativa e para de gerar alertas. | RF07, RN04 |
| `DELETE /api/remedios/:id` | Exclui o cadastro definitivamente (204). | RF03 |

Regras de validação: nome de 1 a 120 caracteres; quantidade inteira de 0 a 9999; observações até 500 caracteres. Vencimento anterior à compra **não bloqueia**: volta em `advertencias`.

## Configurações

| Método e rota | O que faz | Requisitos |
|---------------|-----------|------------|
| `GET /api/configuracoes` | Retorna `{ email, horarioEnvio, antecedencias }`. Padrão: `null`, `"08:00"`, `[30, 15, 0]`. | RF08, RF11 |
| `PUT /api/configuracoes` | Altera só os campos enviados. `email`: e-mail válido ou `null`; `horarioEnvio`: `HH:MM`; `antecedencias`: de 1 a 10 inteiros entre 0 e 365 (duplicadas são removidas). | RF08, RF11 |

## Outras
`GET /api/saude`: verificação do Worker e do banco.

## Pendência de projeto
`horarioEnvio` é guardado, mas o Cron Trigger tem horário fixo no `wrangler.jsonc`. A Etapa 4 precisa decidir: cron de hora em hora com a rotina comparando com `horarioEnvio` (recomendado), ou horário fixo.
