# Modelo de Domínio — Serviço de Assinatura de Marmitas

Caso de uso: **Assinar Plano de Refeições**

## Dicionário de Classes

| Classe | Descrição | Atributos |
|---|---|---|
| Assinante | Contrata o serviço, identificado por celular validado via SMS | `numeroCelular`, `preferenciaAlimentar` |
| EnderecoEntrega | Endereço de entrega de uma assinatura | `logradouro`, `numero`, `cep` |
| Assinatura | Contrato ativo do assinante com um plano | `numeroProtocolo`, `status` |
| PlanoAssinatura | Regras comerciais contratadas | `nome`, `quantidadeRefeicoes`, `periodicidade`, `valor` |
| Pedido | Seleção de itens do cardápio dentro de uma assinatura | `status` |
| Pagamento | Tentativa/transação de cobrança | `dadosCartaoCredito`, `statusAutorizacao` |
| ItemCardapio | Prato/acompanhamento/sobremesa, diferenciado por `categoria` | `nome`, `categoria` |

Operadora de Cartão de Crédito não entrou como classe: é ator externo, fora do domínio interno.

## Diagrama

[Diagrama em branco.pdf](https://github.com/user-attachments/files/32443270/Diagrama.em.branco.pdf)

## Associações

| Associação | Sentido | Multiplicidade | Tipo |
|---|---|---|---|
| realiza | Assinante → Assinatura | 1 — 0..* | Simples |
| utiliza | Assinatura → EnderecoEntrega | 1 — 1 | Simples |
| contém | Assinatura → Pedido | 1 — 1 | Composição |
| vincula | Assinatura → PlanoAssinatura | 0..* — 1 | Simples |
| processa | Assinatura → Pagamento | 1 — 1..* | Simples |
| contém | Pedido → ItemCardapio | 1 — 1..* | Composição (candidata a classe associativa `ItemPedido` com `quantidade`, se o grupo quiser refinar) |

## Premissas assumidas (não explícitas no enunciado)

- Endereço fica na Assinatura, não no Assinante — é capturado no fluxo de contratação, não como cadastro solto.
- Assinatura–Pedido em 1:1 cobre só o pedido inicial da contratação. Se o sistema tratar entregas recorrentes como novos Pedidos, vira 1–0..*.
- Assinante–Assinatura em 1–0..* (não 1:1): permite histórico/múltiplas assinaturas; "só 1 ativa por vez" fica como regra de `status`, não de multiplicidade.

