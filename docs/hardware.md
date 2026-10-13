# Lista de hardware

Quantidades para **um teclado completo (os dois lados)**, já com reserva onde vale a pena.
Os preços são estimativas de out/2026 e precisam ser confirmados ao comprar.

## Eletrônica

| Peça | Especificação exata | Qtd | Onde | ~R$ |
|---|---|---|---|---|
| PCB esquerda | `ergogen/output/pcbs/gerber_left.zip`. FR4, 1,6 mm, 2 camadas, HASL lead-free, cor a gosto | 5 (mínimo) | JLCPCB | 120–180 (as duas, com frete e impostos) |
| PCB direita | `ergogen/output/pcbs/gerber_right.zip`, mesmas opções | 5 (mínimo) | JLCPCB | ↑ |
| Controlador | **nRF52840 SuperMini** (pinagem Pro Micro, compatível com nice!nano v2), lote 2024+ | 2 (+1 reserva) | AliExpress | 70–105 |
| Switch | **Kailh Choc v1 Pro Red** (PG1350, linear, 35 gf) | 60 (+6) | AliExpress (Kailh Official Store) | 150–200 |
| Hotswap | Kailh Choc hotswap socket **PG1350** (v1) | 60 (+10) | AliExpress | ~40 (pacote com 100) |
| Keycaps | Choc **v1** 1u (haste Choc, não MX). Ex.: perfil MBK ou similar | 60 | AliExpress | 90–150 |
| Diodo | **1N4148W, SOD-123** (SMD) | 60 (+20) | AliExpress | ~10 (100 pçs) |
| Chave liga/desliga | **MSK-12C02** (SMD, lateral) | 2 (+2) | AliExpress | ~10 |
| Botão de reset | **Panasonic EVQ-PUC02K** (ou clone EVQPUC lateral) | 2 (+2) | AliExpress | ~10 |
| Conector de bateria | **JST-PH 2.0 mm, 2 pinos**, macho para PCB (THT) | 2 (+2) | AliExpress | ~5 |
| Bateria | LiPo 3,7 V com **plugue JST-PH 2.0**. O tamanho é limitado pela cavidade do case: **medir o case impresso antes de comprar**. O autor usou bateria de reposição de controle de PS3 | 2 | **Mercado Livre** (o AliExpress não envia baterias para o BR) | 50–70 |

> ⚠️ **Polaridade da bateria:** muitas baterias chinesas vêm com o JST invertido. **Meça com o multímetro**
> antes de plugar: o fio vermelho tem de ir no **pad quadrado** do conector na PCB, que é o `+`. A PCB não tem marcação de `+`/`−`. Bateria invertida queima o controlador.

> ℹ️ O controlador vai **soldado direto** com a barra de pinos que vem com o SuperMini, sem sockets.
> Por isso é preciso testá-lo antes de soldar (ver [montagem](montagem.md)).

## Mecânica

| Peça | Especificação | Qtd | ~R$ |
|---|---|---|---|
| Parafuso | M2×3 (cabeça panela ou chata) | 20 (+5) | 30 (kit M2) |
| Espaçador | M2×4 fêmea-fêmea (ou inserto M2 para colocar a quente) | 10 (+2) | ↑ |
| Pés de borracha | Adesivos, 6 mm × 2 mm | 8 | ~10 |
| Case impresso | ver [impressao-3d.md](impressao-3d.md) | 4 peças | 80–150 (loja) |

## Ferramentas

- Ferro de solda com ponta fina (Pinecil, TS101 ou estação)
- Solda 0,6 mm e **fluxo** (pasta ou caneta), essenciais para os SMD
- Pinça
- **Multímetro**, obrigatório para a bateria e para testar continuidade
- Malha dessoldadora (para corrigir pontes de solda)
- Cabo USB-C com dados

## Total estimado

**~R$650–850** para os dois lados, incluindo PCB e impressão, sem as ferramentas.
