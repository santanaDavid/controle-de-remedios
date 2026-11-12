# Requisitos

## Requisitos funcionais

| ID | Requisito |
|----|-----------|
| RF01 | Cadastrar remédio com: nome, data da compra, data de vencimento (obrigatórios) |
| RF02 | Campos opcionais: quantidade, local/observações, marca/dosagem |
| RF03 | Editar e excluir remédios |
| RF04 | Listar remédios ordenados por data de vencimento |
| RF05 | Mostrar status: **Válido**, **Vencendo** (dentro do maior alerta configurado) e **Vencido** |
| RF06 | Filtrar por status e buscar por nome |
| RF07 | Marcar remédio como **descartado** (sai da lista ativa e para de gerar alertas) |
| RF08 | Configurar alertas globais: lista de antecedências (ex.: 30, 15, 0 dias) |
| RF09 | Enviar e-mail de lembrete conforme os alertas configurados |
| RF10 | Não enviar o mesmo alerta duas vezes para o mesmo remédio |
| RF11 | Cadastrar/alterar o e-mail de destino dos lembretes |

## Regras de negócio
- RN01: A data de vencimento deve ser posterior ou igual à data da compra (advertência na tela, sem bloquear, caso o usuário cadastre um remédio já vencido).
- RN02: Remédios já vencidos no momento do cadastro geram um único alerta imediato.
- RN03: Alteração da data de vencimento reinicia os alertas daquele remédio.
- RN04: Remédios descartados não geram alertas.
- RN05: Antecedência 0 significa "no dia do vencimento".

## Requisitos não funcionais
- RNF01: Simples de usar, em português do Brasil.
- RNF02: Verificação de alertas executada ao menos uma vez por dia (ex.: 08:00).
- RNF03: Os dados ficam persistidos (não podem se perder ao fechar o app).
- RNF04: Arquitetura preparada para adicionar novos canais (push) sem reescrever o resto.
- RNF05: Dados de saúde são pessoais: sem compartilhamento com terceiros.
