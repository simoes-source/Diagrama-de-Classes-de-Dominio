# Modelo de Domínio — Serviço de Assinatura de Marmitas

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

<img width="1450" height="1151" alt="WhatsApp Image 2026-09-20 at 16 51 04" src="https://github.com/user-attachments/assets/79d6f743-3206-4fe8-9003-e06ea09d1f6d" />

## Associações

| Associação | Sentido | Multiplicidade | Tipo |
|---|---|---|---|
| possui | Assinante → EnderecoEntrega | 1 — 1..* | Simples |
| realiza | Assinante → Assinatura | 1 — 0..* | Simples |
| vincula | Assinatura → PlanoAssinatura | 0..* — 1 | Simples |
| contém | Assinatura → Pedido | 1 — 1..* | Composição |
| processa | Assinatura → Pagamento | 1 — 1..* | Simples |
| contém | Pedido → ItemCardapio | 1 — 1..* | Composição |

## Premissas assumidas (não explícitas no enunciado)

- Endereço fica ligado ao Assinante (não à Assinatura): assumido que o cadastro de endereço é reutilizável entre assinaturas do mesmo assinante. Em contrapartida, o diagrama não deixa explícito qual endereço vale para qual assinatura quando o assinante tem mais de um — ponto a justificar na apresentação, se perguntado.
- Assinatura–Pedido em 1–1..*: considera que uma assinatura gera múltiplos pedidos ao longo do tempo (uma entrega por período, conforme `periodicidade` do plano), não só o pedido inicial da contratação.
- Assinante–Assinatura em 1–0..* (não 1:1): permite histórico/múltiplas assinaturas; "só 1 ativa por vez" fica como regra de `status`, não de multiplicidade.![Uploading WhatsApp Image 2026-09-20 at 16.51.04.jpeg…]()
