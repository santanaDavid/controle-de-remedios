# Modelo de Dados

## Remedio
| Campo | Tipo | Obrigatório | Observação |
|-------|------|-------------|------------|
| id | identificador | sim | |
| nome | texto | sim | |
| data_compra | data | sim | |
| data_vencimento | data | sim | |
| quantidade | número | não | |
| observacoes | texto | não | marca, dosagem, local |
| descartado | booleano | sim | padrão: falso |
| criado_em | data/hora | sim | |

## ConfiguracaoAlerta
| Campo | Tipo | Observação |
|-------|------|------------|
| id | identificador | |
| dias_antes | inteiro | 30, 15, 0... |
| ativo | booleano | |
| canal | texto | `email` na v1; `push` no futuro |

Padrão inicial: 30, 15 e 0 dias, canal e-mail.

## Usuario (configuração)
| Campo | Tipo |
|-------|------|
| email | texto |
| horario_envio | hora (padrão 08:00) |

## AlertaEnviado
Evita duplicidade (RF10).
| Campo | Tipo |
|-------|------|
| remedio_id | referência |
| dias_antes | inteiro |
| canal | texto |
| enviado_em | data/hora |
