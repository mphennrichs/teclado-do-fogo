# Montagem

Vídeo do autor original, que vale a pena ver inteiro antes de começar: <https://www.youtube.com/watch?v=7UXsD7nSfDY>

As duas PCBs são **diferentes** (esquerda e direita, não reversíveis). Faça um lado inteiro, teste, e só depois faça o outro.

## 0. Encomendas (semana 0)

- [ ] PCBs na JLCPCB: enviar `gerber_left.zip` e `gerber_right.zip` como **dois pedidos/designs separados**
- [ ] Peças no AliExpress (ver [hardware.md](hardware.md))
- [ ] Mandar imprimir o case (ver [impressao-3d.md](impressao-3d.md))
- [ ] A bateria fica **para depois de receber o case**, porque é preciso medir a cavidade

## 1. Testar os controladores antes de soldar

Como o SuperMini vai soldado direto, é bem mais fácil trocar um controlador com defeito agora do que depois.

1. Baixe o firmware (ver [firmware.md](firmware.md)).
2. Plugue o SuperMini no USB e dê **dois toques rápidos** no botão de reset (ou faça um curto entre RST e GND). Ele aparece como um pendrive (`NICENANO` ou similar).
3. Copie `settings_reset-nice_nano_v2-zmk.uf2`. Ele reinicia sozinho.
4. Repita e copie `teclado_do_fogo_left-…uf2` em um e `teclado_do_fogo_right-…uf2` no outro.
5. No lado esquerdo, confira se "Teclado do Fogo" aparece no Bluetooth do computador.
6. **Marque com caneta qual SuperMini é o esquerdo e qual é o direito.**

## 2. Diodos (30 por lado)

1. Os diodos 1N4148W (SOD-123) vão nos pads pequenos ao lado de cada tecla. O footprint tem pads nas duas faces: use o **verso** (a mesma face dos hotswaps), como no vídeo do autor. Não há rótulo "D" no silkscreen; siga o contorno do diodo.
2. **Orientação:** a faixa do diodo fica do lado da faixa/linha no silkscreen. Confira **todos** antes de soldar o segundo pad.
3. Técnica: estanhe um pad, posicione o diodo com a pinça enquanto reaquece esse pad, depois solde o outro lado.
4. Os diodos ficam em posições diferentes em algumas teclas, deslocados para não bater no controlador e nos furos. Siga o silkscreen.

## 3. Hotswaps (30 por lado)

1. Os sockets Kailh vão no **verso** (onde estão os pads grandes).
2. Coloque fluxo, encaixe o socket e solde os dois terminais metálicos com bastante solda, porque eles levam esforço mecânico toda vez que um switch é trocado.

## 4. Componentes de energia (no verso)

1. **Chave MSK-12C02:** SMD com pinos pequenos. Solde primeiro as abas de fixação, depois os pinos.
2. **Reset EVQ-PUC:** botão lateral, com o lado do botão voltado para a borda da PCB.
3. **Conector JST-PH:** a PCB **não tem marcação de `+`/`−`**. O **pad quadrado é o `+`** (bateria, `pos`) e o pad redondo é o `GND`. Anote isso com caneta na PCB, porque vai precisar dessa informação na etapa 7.

## 5. Controlador

> ⚠️ **Esta é a etapa mais perigosa.** O footprint não é reversível (a PCB esquerda é rotacionada 180° em relação à direita).
> Se o controlador for soldado com a face trocada, o `RAW` da PCB cai num pino de sinal e o `GND` cai em `RST`/`VCC`.
> Como ele vai soldado direto, não dá para desfazer.

1. Siga o vídeo do autor para a face e a orientação. O controlador fica **embaixo das teclas**.
2. **Antes de soldar**, encaixe o SuperMini com os pinos apenas apoiados e confira **pino a pino** com o silkscreen da PCB:
   - o pad `RAW` da PCB tem de coincidir com o pino `B+`/`RAW` do controlador;
   - os pads `GND` da PCB com os pinos `GND`;
   - o pad `RST` com `RST`.
   Use o multímetro em continuidade para confirmar que `RAW` na PCB vai até a chave liga/desliga.
3. Encaixe a barra de pinos na PCB **com o SuperMini já posicionado em cima** (assim os pinos ficam alinhados) e solde primeiro um pino de cada canto. Confira de novo e só então solde o resto.
4. Corte o excesso dos pinos rente à placa, já que o espaço embaixo das teclas é pequeno.
5. Os 3 pads extras do SuperMini (P1.01/P1.02/P1.07) não são usados.

## 6. Teste elétrico (sem bateria, via USB)

1. Plugue no USB. O lado esquerdo conecta ao computador e o direito se pareia com o esquerdo sozinho. Para testar o **lado direito**, o esquerdo precisa estar ligado (via USB ou bateria), porque só o esquerdo fala com o computador.
2. Abra um testador de teclado (ex.: keyboard-test.space) e **faça um curto nos dois contatos de cada hotswap** com a pinça. Cada posição tem de registrar a tecla certa.
3. Se uma tecla não funcionar: confira o diodo (orientação e solda) e o socket. Se uma linha ou coluna inteira falhar, confira os pinos do controlador.

## 7. Bateria

1. Meça a cavidade do case e compre a bateria (ver [hardware.md](hardware.md)).
2. **Com o multímetro, confira a polaridade do plugue:** o fio `+` (vermelho) tem de ir no **pad quadrado** do JST. Se estiver invertida, troque os fios no plugue (dá para soltar os terminais do conector JST com uma agulha).
3. Plugue a bateria, ligue a chave e confira se o lado funciona sem USB. **A bateria só carrega pelo USB com a chave ligada**, porque a chave desconecta a bateria do controlador. Fixe a bateria com fita dupla-face fina.

## 8. Fechamento

1. Encaixe os switches no plate do top, com a PCB por baixo, alinhando os pinos nos sockets hotswap.
2. Rosqueie os espaçadores M2×4 (ou coloque os insertos) e feche o bottom com os parafusos M2×3.
3. Keycaps e pés de borracha.

## 9. Atualizar o firmware com o case fechado

O botão de reset fica dentro do case. Para entrar no modo de gravação sem abrir: segure `LOWER` e aperte a tecla do **canto inferior esquerdo** (`BOOT`). Ele monta como pendrive. Faça isso em cada lado (no direito, a tecla equivalente é a do canto inferior direito).

## 10. Pareamento final

- Se os lados não se encontrarem: grave `settings_reset` **nos dois**, depois o firmware de cada lado, e ligue os dois ao mesmo tempo.
- No computador, remova o "Teclado do Fogo" antigo do Bluetooth antes de parear de novo.
- `LOWER` + 2ª linha da esquerda (`BT1`–`BT5`) troca entre até 5 computadores. `LOWER` + `BT CLR` apaga o pareamento do perfil atual.
