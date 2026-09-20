# Modelo de Domínio — Serviço de Assinatura de Marmitas

## Participantes 

João Pedro Nascimento Simões - 10427517
Renan Dos Santos Jesus - 10748027

Caso de uso: **Assinar Plano de Refeições**

## Dicionário de Classes

| Classe | Descrição | Atributos |
|---|---|---|
| Assinante | Contrata o serviço, identificado por celular validado via SMS | `numeroCelular`, `preferenciaAlimentar` |
| EnderecoEntrega | Endereço de entrega vinculado ao assinante | `logradouro`, `numero`, `cep` |
| Assinatura | Contrato ativo do assinante com um plano | `numeroProtocolo`, `status` |
| PlanoAssinatura | Regras comerciais contratadas | `nome`, `quantidadeRefeicoes`, `periodicidade`, `valor` |
| Pedido | Seleção de itens do cardápio gerada dentro de uma assinatura | `status` |
| Pagamento | Tentativa/transação de cobrança da assinatura | `dadosCartaoCredito`, `statusAutorizacao` |
| ItemCardapio | Prato/acompanhamento/sobremesa, diferenciado por `categoria` | `nome`, `categoria` |

Operadora de Cartão de Crédito não entrou como classe: é ator externo, fora do domínio interno.

## Diagrama

<img width="1450" height="1151" alt="WhatsApp Image 2026-09-20 at 16 51 04" src="https://github.com/user-attachments/assets/91dd0105-e15e-49e4-abdc-2b0fac45f329" />

## Associações

| Associação | Sentido | Multiplicidade | Tipo |
|---|---|---|---|
| possui | Assinante → EnderecoEntrega | 1 — 1..* | Simples |
| realiza | Assinante → Assinatura | 1 — 0..* | Simples |
| vincula | Assinatura → PlanoAssinatura | 0..* — 1 | Simples |
| contém | Assinatura → Pedido | 1 — 1..* | Composição |
| processa | Assinatura → Pagamento | 1 — 1..* | Simples |
| contém | Pedido → ItemCardapio | 1 — 1..* | Composição |
