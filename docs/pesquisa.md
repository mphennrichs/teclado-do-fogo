# Teclado split: pesquisa sobre o Caldera e proposta

> Pesquisa inicial que originou o projeto. As decisões finais estão na §8.
> Data: 2026-10-13 · Base: [christianselig/caldera-keyboard](https://github.com/christianselig/caldera-keyboard) (MIT, 356★, 97 forks, último commit em 2024-12)

---

## 1. O que é o Caldera (estado atual do upstream)

| Item | Como está no original |
|---|---|
| Layout | 60 teclas (5×6 por lado), colunas com stagger (0, 0, +7, +12, +6, +3 mm) |
| Switches | Kailh **Choc v1** low-profile, **hotswap** (soquetes Kailh) |
| Controlador | **nice!nano v2** (nRF52840, BLE), montado **embaixo das teclas** (não tem "área do MCU") |
| Firmware | ZMK, split BLE (o lado esquerdo é o central) |
| PCB | Gerado com Ergogen v4 e roteado no KiCad. **Não é reversível**: são duas PCBs diferentes (esq. e dir.) |
| Energia | Bateria LiPo (o autor usou uma bateria de reposição de controle de PS3), conector JST-PH, chave liga/desliga MSK-12C02, botão de reset EVQ-PUC |
| Case | Impresso em 3D (PLA), top + bottom por lado, 10× parafusos M2×3 e 5× espaçadores M2×4 por lado. Os arquivos ficam **só no MakerWorld** ([545431](https://makerworld.com/en/models/545431)), não estão no repo |
| Custo do autor | "menos de US$200" em consumíveis, comprando quase tudo na Typeractive (EUA) |

### Problemas encontrados no upstream (revisão do código)

1. **O CI do firmware não roda.** O `build.yml` está em `zmk-config-caldera/.github/workflows/`, e o GitHub só lê `.github/` na raiz. Praticamente todos os forks tiveram de mover os arquivos, e por isso aparecem dezenas de commits de "moved files" / "fix structure".
2. **ZMK sem versão fixa** (`revision: main` no `west.yml`). O ZMK migrou para o Zephyr 4 (HWMv2) e renomeou a board `nice_nano_v2` → `nice_nano`. Com o `main`, o build original **quebra**. Vários forks corrigiram fixando em `v0.3` (cmellwig, lucaycc-a11y, an-nguyen-repo).
3. **A matrix transform está errada.** Ela declara `rows = <5>`, mas o `map` tem **6 linhas** (`RC(5,x)` sobrando). Funciona por acaso; o fork cmellwig comentou a linha extra.
4. **O layer `lower` está incompleto.** Ele tem 48 bindings para 60 posições (falta a última linha). O fork lucaycc-a11y corrigiu.
5. **Os arquivos `.conf` estão duplicados.** O `caldera.conf` e o `caldera_left.conf` são idênticos, e o lado direito também tem um. Isso só faz sentido se um lado tiver configuração própria (ex.: proxy de bateria só no central).
6. **O case não tem fonte editável.** Só existe no MakerWorld (Fusion 360, licença do MakerWorld), então não dá para versionar nem adaptar no nosso repo.
7. **O Ergogen não tem versão fixa.** O fork PrestonHager/magma documentou que é preciso rodar `npx ergogen@4.2.1 .` **no diretório**, e não no `config.yaml`, senão os footprints customizados não carregam.

---

## 2. Forks analisados

Comparei os 40 forks que têm commits próprios com o upstream. A maioria só trocou o keymap ou moveu arquivos. Os que valem a pena:

| Fork | O que propõe | Vale a pena? |
|---|---|---|
| **[cmellwig/caldera-keyboard](https://github.com/cmellwig/caldera-keyboard)** | **ZMK Studio** (edita o keymap pelo navegador, sem recompilar), board `nice_nano` (Zephyr 4), ZMK fixado em `v0.3`, transform corrigida | ✅ **Sim, é a melhor base para o firmware.** Ressalva: o `physical_layout` usa um stagger genérico (0/20/40) que **não corresponde** ao Caldera real. Vamos recalcular (ver §4.1) |
| **[lucaycc-a11y/caldera-keyboard](https://github.com/lucaycc-a11y/caldera-keyboard)** | Estrutura padrão de zmk-config na raiz, ZMK `v0.3.0`, layer "mac" com mídia/brilho, corrige o lower layer | ✅ Sim, pego a estrutura e a ideia do layer de mídia |
| **[jfdmsc/caldera-keyboard](https://github.com/jfdmsc/caldera-keyboard)** | Mostra no host a **bateria do lado direito** (`SPLIT_BLE_CENTRAL_BATTERY_LEVEL_FETCHING/PROXY`), combos para trocar de layer, mod-morph | ✅ Pego o proxy de bateria. Os combos são questão de gosto |
| **[arthurlockman](https://github.com/arthurlockman/caldera-keyboard)** / famichikigirl | `CONFIG_ZMK_BLE_EXPERIMENTAL_CONN`, teclas de mídia, wake por tecla | 🟡 Testar o `EXPERIMENTAL_CONN` só se houver queda de conexão |
| **[matdurand](https://github.com/matdurand/caldera-keyboard)** (já mergeado no upstream) | Wake do deep sleep e suporte ao keymap-editor. Também fez um **case com encaixe (sem parafusos)** no [MakerWorld 1240609](https://makerworld.com/en/models/1240609-caldera-split-keyboard-case-clippable-design) | 🟡 O case com clips elimina 20 parafusos e 10 espaçadores. Avaliar o encaixe na impressão |
| [Gist ZMK Studio (phulsechinmay)](https://gist.github.com/phulsechinmay/142c4afed4d51678bffa0493d3aefb63) | Outro `physical_layout` para o Caldera | 🟡 Também é aproximado, serve de referência |
| [PrestonHager/magma-keyboard](https://github.com/PrestonHager/magma-keyboard) | Documenta como rodar o Ergogen 4.2.1 corretamente | ✅ Só a documentação |
| [shubymao/caldera-mx](https://github.com/shubymao/caldera-mx) | Troca os footprints Choc por **MX** no Ergogen | ❌ É um rascunho ("first compile"): PCB não roteada, não testada, e exigiria redesenhar o case. Seria mais barato (switches/keycaps MX custam menos), mas perde o perfil baixo e é um projeto novo |
| [mturoci/split-keyboard](https://github.com/mturoci/split-keyboard) | Projeto derivado ("hardcode") com outro layout | ❌ É outro teclado, não melhora o Caldera |
| kfpersson/caldera-keyboard-nordic | "Coluna ISO" | ❌ A alteração está quebrada (YAML inválido, gerado por IA) |

---

## 3. Onde baratear sem perder funcionalidade

A funcionalidade fica a mesma: wireless BLE, split, hotswap, bateria, low-profile e o mesmo case. As economias, da maior para a menor:

| # | Troca | Economia aproximada | Risco |
|---|---|---|---|
| 1 | **nice!nano v2 → nRF52840 SuperMini** (clone, mesmo chip e mesma pinagem no header Pro Micro, usa o firmware `nice_nano`) | **~R$250–300** (nice!nano ≈ €26 cada + importação, SuperMini ≈ R$30–40 cada) | Lotes antigos (antes de 04/2024) tinham consumo em standby de ~700 µA (resistor errado), já corrigido de fábrica. Carga de ~100 mA, que é ok para bateria pequena. A qualidade varia por vendedor. **Validar a pinagem B+/RAW com multímetro antes de soldar** |
| 2 | **Comprar switches, soquetes, keycaps e diodos no AliExpress** (loja oficial da Kailh) em vez da Typeractive | ~R$300–500 (e evita a taxa de importação sobre uma compra grande dos EUA) | Prazo maior. Keycaps Choc genéricas têm acabamento inferior às MBK |
| 3 | **Sockets do controlador:** Mill-Max (Typeractive) → barra de pinos fêmea torneada de perfil baixo | ~R$50 | ⚠️ A altura precisa caber embaixo das teclas (o MCU fica sob o PCB, dentro do case). Medir antes. Como o SuperMini é barato, também dá para **soldar direto** |
| 4 | **Bateria no Mercado Livre** (LiPo 3,7 V com JST-PH 2.0) | — | ⚠️ **Muitas baterias chinesas vêm com a polaridade invertida no JST.** Conferir +/− antes de ligar, porque inverter queima o controlador. Medir a cavidade do case antes de escolher o tamanho |
| 5 | **Case com clips** (remix do matdurand) | ~R$30 em parafusos e insertos | Encaixe depende da impressora |
| 6 | **Manter a PCB não reversível** e pedir as duas na JLCPCB (5 de cada) | — | Tornar a PCB reversível baratearia ~US$2, mas exige rerotear tudo com o MCU embaixo das teclas. **Não compensa** |

**Descartado por perder funcionalidade:** tirar o hotswap (economiza ~R$40, mas solda os switches), trocar para MX (perde o low-profile e é um projeto novo) e trocar BLE por fio.

### Estimativa de custo (BRL, out/2026, **valores aproximados, confirmar ao cotar**)

| Componente | Qtd | Original (Typeractive + importação) | Proposta |
|---|---|---|---|
| Controlador | 2 | nice!nano ~R$300+ | SuperMini nRF52840 ~R$70 |
| Switches Choc v1 | 60 (+10 reserva) | ~R$350 | Kailh oficial AliExpress ~R$130–180 |
| Hotswap Choc | 60 (+10) | ~R$90 | ~R$40 (pacote com 100) |
| Keycaps Choc 1u | 60 | MBK ~R$300 | Genéricas/MBK-clone ~R$90–150 |
| Diodos 1N4148W (SOD-123) | 60 | ~R$20 | ~R$10 (100 pçs) |
| Chave MSK-12C02 + reset EVQ-PUC + JST-PH 2.0 | 2 de cada | ~R$50 | ~R$25 |
| Sockets do MCU | 2 pares | Mill-Max ~R$60 | Barra torneada ~R$15 (ou R$0, soldando direto) |
| Bateria LiPo | 2 | ~R$80 | Mercado Livre ~R$50–70 |
| PCB (JLCPCB, 2 designs × 5) | 10 | ~R$120–180 com frete e impostos | igual |
| Filamento PLA | ~150 g | ~R$20 | igual (ou serviço de impressão ~R$80–150) |
| Parafusos M2 + insertos + pés de borracha | — | ~R$40 | ~R$30 (ou ~R$5 com o case de clips) |
| **Total aproximado** | | **~R$1.400–1.800** | **~R$650–850** |

---

## 4. Software

### 4.1 Firmware (ZMK)

Base: o fork **cmellwig** (ZMK Studio) mais a estrutura do **lucaycc**, com estas correções:

- [ ] Repo zmk-config com `.github/workflows/build.yml`, `build.yaml` e `config/` **na raiz do repo**. O CI gera os `.uf2` como artefatos.
- [ ] `west.yml` fixado em uma **tag** do ZMK (`v0.3`) e board `nice_nano_v2` (nome na v0.3, serve também para o SuperMini)
- [ ] Transform 5×12 correta (remover a linha `RC(5,x)`)
- [ ] `physical_layout` com o **stagger real** (pitch vertical 17,75 mm, offsets em centésimos de tecla, y para baixo):
  - colunas 1–6 da esquerda: `y = 68, 68, 28, 0, 34, 51`. O lado direito é espelhado.
- [ ] ZMK Studio no lado central (snippet `studio-rpc-usb-uart`, `CONFIG_ZMK_STUDIO=y`) e uma tecla `&studio_unlock` em algum layer
- [ ] Proxy de bateria do lado direito (configs do jfdmsc), apenas no `.conf` do lado esquerdo
- [ ] `CONFIG_ZMK_SLEEP=y` (deep sleep) com wakeup-source (já existe)
- [ ] Layers: base, lower completo (60 posições), mídia/BT. **Keymap pensado para pt-BR** (ver decisões)
- [ ] Alvo `settings_reset` no build (já existe)

### 4.2 Hardware/CAD

- [ ] Ergogen fixado em `4.2.1`, rodando `npx ergogen@4.2.1 .` no diretório (ver `ergogen/README.md`)
- [ ] Manter os Gerbers *finished* do upstream como estão. **Não vou rerotear a PCB nesta fase**, porque o roteamento original foi validado pelo autor e por vários builds dos forks
- [ ] Fase 2 (opcional): trilhas mais grossas e plano de GND no KiCad. É melhoria de robustez e exige DRC e um novo pedido de PCB

---

## 5. Lista de hardware (por teclado completo = 2 lados)

| Peça | Especificação | Qtd | Onde |
|---|---|---|---|
| PCB esquerda | `gerber_left.zip`, FR4 1,6 mm, HASL sem chumbo | 1 (mín. 5) | JLCPCB |
| PCB direita | `gerber_right.zip` | 1 (mín. 5) | JLCPCB |
| Controlador | nRF52840 SuperMini (ou nice!nano v2) | 2 | AliExpress |
| Switch | Kailh Choc **v1** (PG1350): Red/Brown/Pro Red, a gosto | 60 + reserva | AliExpress (Kailh Official) |
| Hotswap | Kailh Choc hotswap socket (PG1350) | 60 + reserva | AliExpress |
| Keycap | Choc 1u (espaçamento Choc, ~17,5 × 16,5 mm) | 60 | AliExpress |
| Diodo | 1N4148W SOD-123 (o footprint também aceita THT 1N4148) | 60 | AliExpress |
| Chave liga/desliga | MSK-12C02 SMD | 2 | AliExpress |
| Reset | Panasonic EVQ-PUC02K (lateral) | 2 | AliExpress |
| Conector bateria | JST-PH 2.0 mm, 2 pinos, macho para PCB | 2 | AliExpress |
| Bateria | LiPo 3,7 V com JST-PH 2.0 e capacidade de ~300–1000 mAh, **tamanho limitado pela cavidade do case** | 2 | Mercado Livre |
| Sockets do MCU | Barra de pinos fêmea torneada de perfil baixo + pinos (ou solda direta) | 2 conj. | AliExpress |
| Fixação | 20× parafuso M2×3, 10× espaçador M2×4 **ou** inserto M2 (case original) | — | AliExpress/ML |
| Pés | Borracha 6 × 2 mm | 8 | ML |

**Ferramentas:** ferro de solda com ponta fina (ex.: Pinecil), solda 0,6 mm, fluxo, pinça, multímetro (**obrigatório** para checar a polaridade da bateria), impressora 3D.

---

## 6. O que vai ser impresso em 3D

| Peça | Arquivo | Qtd | Material / config |
|---|---|---|---|
| Case top (com plate integrado), esquerdo e direito | MakerWorld 545431 (ou o remix com clips 1240609) | 1 + 1 | PLA, **face para baixo**, paredes grossas, **sem infill** (recomendação do autor) |
| Case bottom, esquerdo e direito | idem | 1 + 1 | idem |
| (opcional) Plate avulso para testes | `ergogen/output/cases/plate.stl` (1,2 mm) | 1 | PLA. Serve para testar o encaixe dos switches antes do case |
| (opcional, nova) Pés de tenting/inclinação | a desenhar | 2–4 | PLA/PETG |

> ⚠️ **Licença:** os arquivos do MakerWorld têm licença própria e não posso redistribuí-los no repo. Vou **linkar** para eles. Se quisermos o case versionado e editável, isso vira um item da fase 2: refazer o case paramétrico (OpenSCAD/build123d) a partir dos DXF do Ergogen.

---

## 7. Plano de montagem

1. **Encomendar** as PCBs (JLC) e as peças (Ali/ML) em paralelo. Prazo estimado de 3 a 6 semanas.
2. **Imprimir o case** e o plate de teste enquanto as peças chegam. Conferir o encaixe de um switch no plate.
3. **Testar os controladores antes de soldar.** Plugar no USB, gravar o `.uf2` e confirmar que aparecem como BLE. Medir a pinagem B+/GND do SuperMini.
4. **Diodos (60).** Soldar todos com a **orientação da faixa conferida** no silkscreen. Os SMD vão no lado indicado pelo footprint.
5. **Hotswaps (60)** no verso.
6. **MSK-12C02, EVQ-PUC e JST-PH** (componentes pequenos SMD, usar fluxo).
7. **Sockets e controlador.** Atenção à orientação (componente virado para baixo, conforme o silkscreen) e à **altura máxima** embaixo das teclas.
8. **Teste elétrico sem bateria, via USB.** Gravar o firmware e testar cada tecla com pinça (curto nos contatos do hotswap) em um site de teste de teclado.
9. **Bateria.** **Conferir a polaridade com o multímetro**, ligar e testar a chave on/off e o pareamento entre os lados.
10. **Switches** no plate do case e depois na PCB (hotswap). Fixar o bottom (parafusos ou clips), keycaps e pés.
11. **Pareamento final.** Se der problema, usar `settings_reset` nos dois lados e depois parear de novo.

---

## 8. Decisões tomadas (2026-10-13)

| # | Decisão | Motivo |
|---|---|---|
| 1 | **nRF52840 SuperMini** | Maior economia (~R$250), mesma funcionalidade |
| 2 | **Controlador soldado direto**, sem sockets | É o mais fácil: sem peça extra, mais baixo (sobra folga sob as teclas) e o SuperMini é barato. Os controladores são testados antes de soldar |
| 3 | **US-International** no sistema operacional | ç e acentos via dead keys, sem precisar de layer especial |
| 4 | **Kailh Choc v1 Pro Red** | O Gateron KS-33 que foi pedido não encaixa (3 pinos, outro footprint e outras keycaps). O Redragon MX também não. O Choc v2 exigiria aumentar os furos da PCB. O Pro Red é linear e leve, foi o switch usado pelo autor e não exige mudança |
| 5 | **Case original com parafusos**, impresso em loja | O encaixe por clips depende da calibração da impressora, que não controlamos |
| 6 | Repo **público** `teclado-do-fogo`, novo (não fork), com crédito e licença MIT | O Actions é gratuito em repo público. A estrutura foi reorganizada (zmk-config na raiz) |
