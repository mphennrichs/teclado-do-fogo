# Firmware (ZMK)

- ZMK **v0.3** (versão fixa em [`config/west.yml`](../config/west.yml))
- Board `nice_nano_v2`, que também serve para o SuperMini nRF52840
- Shield `teclado_do_fogo_left` (central, com ZMK Studio) e `teclado_do_fogo_right` (periférico)

## Baixar o firmware

Cada push que altera `config/` ou `build.yaml` dispara o GitHub Actions.

1. Abra a aba **Actions** do repositório e entre no último build verde.
2. Baixe o artefato `firmware.zip`, que tem três arquivos:
   - `teclado_do_fogo_left-nice_nano_v2-zmk.uf2`
   - `teclado_do_fogo_right-nice_nano_v2-zmk.uf2`
   - `settings_reset-nice_nano_v2-zmk.uf2`

Para gerar sem alterar nada: **Actions → Run workflow**.

## Gravar

1. Plugue o lado no USB.
2. Dois toques rápidos no reset: ele monta como um pendrive.
3. Arraste o `.uf2` correspondente. Ele reinicia sozinho.

Com o case fechado, entre no modo de gravação segurando `LOWER` e apertando a tecla do canto inferior externo (`BOOT`) de cada lado.

Grave **os dois lados** sempre que mudar o firmware. Se só o keymap mudou, gravar o esquerdo basta.

## Editar o keymap

**Sem recompilar (recomendado):** [ZMK Studio](https://zmk.studio)

1. Plugue o **lado esquerdo** no USB e abra <https://zmk.studio> no Chrome/Edge (ou use o app desktop).
2. Desbloqueie segurando `LOWER` e apertando `STUDIO`, que é a tecla `GUI` interna da última linha à esquerda (6ª tecla da linha de baixo).
3. Edite e salve. A mudança fica gravada no teclado.

**Pelo código:** edite [`config/teclado_do_fogo.keymap`](../config/teclado_do_fogo.keymap) e faça push.

## Layout no sistema operacional

Configure o teclado como **"US-International"** (Windows/Linux: *Inglês (EUA) – Internacional*; macOS: *U.S. International – PC*).

| Para digitar | Aperte |
|---|---|
| ç | `'` e depois `c` |
| á é í ó ú | `'` e depois a vogal |
| ã õ | `~` (Shift+`` ` ``) e depois a vogal |
| â ê ô | `^` (Shift+6) e depois a vogal |
| à | `` ` `` e depois `a` |
| `'` sozinho | `'` e depois espaço |

> No Linux, `'` + `c` pode gerar **ć** em vez de ç, dependendo do locale. Use o layout *English (US, intl., with dead keys)*
> com o locale `pt_BR.UTF-8`, ou ajuste o `~/.XCompose`. No Windows e no macOS funciona direto.

## Configuração

| Arquivo | Para quê |
|---|---|
| [`config/teclado_do_fogo.conf`](../config/teclado_do_fogo.conf) | Os dois lados: deep sleep, potência do BLE, teclas de mídia |
| [`…/teclado_do_fogo_left.conf`](../config/boards/shields/teclado_do_fogo/teclado_do_fogo_left.conf) | Só o central: mostra a bateria do lado direito no computador |
| [`…/teclado_do_fogo.dtsi`](../config/boards/shields/teclado_do_fogo/teclado_do_fogo.dtsi) | Matriz, pinos e layout físico (gerado a partir do stagger do `ergogen/config.yaml`) |

Se houver quedas de conexão Bluetooth, experimente adicionar `CONFIG_ZMK_BLE_EXPERIMENTAL_CONN=y` ao `.conf`.
