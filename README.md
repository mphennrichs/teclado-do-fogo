# 🔥 Teclado do Fogo

Teclado split, ergonômico, sem fio (Bluetooth) e low-profile, com 60 teclas (5×6 por lado).

É uma versão do [Caldera](https://github.com/christianselig/caldera-keyboard), de Christian Selig (MIT), com o firmware corrigido e atualizado e componentes mais baratos, **mantendo a mesma funcionalidade**: BLE, hotswap, bateria e o mesmo PCB e case.

## O que muda em relação ao Caldera

| | Caldera | Teclado do Fogo |
|---|---|---|
| Controlador | nice!nano v2 | **nRF52840 SuperMini** (clone com a mesma pinagem e o mesmo firmware), soldado direto |
| Firmware | ZMK `main` (o build atual quebra) | ZMK fixado em **`v0.3`**, com o CI na raiz funcionando |
| Edição do keymap | Recompilar | **ZMK Studio** pelo navegador, com o layout físico real |
| Bateria do lado direito | Não aparece | Aparece no computador (proxy de bateria) |
| Keymap | Layer `lower` incompleto, transform com uma linha a mais | Corrigidos, pensado para **US-International** (acentos e ç) |
| Custo estimado | ~R$1.400–1.800 | ~R$650–850 |

PCB e case: **os mesmos do Caldera**, sem alteração.

## Estrutura

| Caminho | Conteúdo |
|---|---|
| [`config/`](config/) | Firmware ZMK (shield `teclado_do_fogo`, keymap, west) |
| [`build.yaml`](build.yaml) + [`.github/workflows/`](.github/workflows/) | Build do firmware no GitHub Actions |
| [`ergogen/`](ergogen/) | PCB (Ergogen + KiCad) e **Gerbers prontos para fabricar** |
| [`docs/hardware.md`](docs/hardware.md) | Lista de compras |
| [`docs/impressao-3d.md`](docs/impressao-3d.md) | O que imprimir e o que pedir à loja de impressão |
| [`docs/montagem.md`](docs/montagem.md) | Passo a passo da montagem |
| [`docs/firmware.md`](docs/firmware.md) | Como baixar, gravar e editar o firmware |
| [`docs/pesquisa.md`](docs/pesquisa.md) | Revisão do Caldera, dos forks e as decisões tomadas |

## Créditos

- [Christian Selig](https://github.com/christianselig/caldera-keyboard), autor do projeto original: PCB, case e firmware base
- [cmellwig](https://github.com/cmellwig/caldera-keyboard), suporte ao ZMK Studio
- [lucaycc-a11y](https://github.com/lucaycc-a11y/caldera-keyboard) e [jfdmsc](https://github.com/jfdmsc/caldera-keyboard), correções de firmware e proxy de bateria
- [matdurand](https://github.com/matdurand/caldera-keyboard), wake do deep sleep e o case com clips

Licença MIT (veja [LICENSE](LICENSE)). Os arquivos do case ficam no MakerWorld, com a licença de lá.
