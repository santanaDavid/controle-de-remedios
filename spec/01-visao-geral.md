# Controle de Vencimento de Remédios — Visão Geral

## Problema
A gaveta de remédios acumula remédios vencidos. Muitas vezes a data de vencimento
some da caixa (a embalagem rasga, a tinta apaga, a caixa é descartada), e não há como
saber se o remédio ainda pode ser usado.

## Objetivo
Cadastrar os remédios comprados, com data de compra e data de vencimento, e receber
lembretes antes (e no dia) do vencimento.

## Escopo da versão 1 (MVP)
- Cadastro, edição e exclusão de remédios
- Lista de remédios ordenada por vencimento, com destaque visual (ok / vencendo / vencido)
- Lembretes por **e-mail**
- Alertas configuráveis (ex.: 30 dias antes, 15 dias antes, no dia)

## Fora do escopo da v1 (futuro)
- Push no celular
- Leitura de código de barras / foto da caixa
- Múltiplos usuários / compartilhamento com a família
- Controle de estoque e dose

## Plataforma
Aplicação **web**, acessível pelo navegador (celular e computador). Precisa de um servidor (backend) para guardar os dados e enviar os e-mails.

## Usuário
Uma única pessoa (o dono da casa), uso pessoal, até cerca de 20 remédios. Sem múltiplos usuários e sem carga inicial: o cadastro é feito pela tela.

## Premissas
- A data de vencimento é informada no momento do cadastro, enquanto a caixa ainda está legível.
- O e-mail do usuário é cadastrado uma única vez.

## Termos
Os nomes usados em toda a especificação, nas telas e no código estão definidos em `07-glossario.md`.
