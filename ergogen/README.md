# Ergogen

Configuração original do [Caldera](https://github.com/christianselig/caldera-keyboard), sem alterações.
Os Gerbers prontos para fabricar estão em `output/pcbs/gerber_left.zip` e `output/pcbs/gerber_right.zip`.

Para regenerar os arquivos (só necessário se mudar o `config.yaml`), rode **no diretório**, com a versão fixa:

```bash
cd ergogen
npx ergogen@4.2.1 .
```

Rodar `npx ergogen config.yaml` falha, porque assim os footprints em `footprints/` não são carregados.

> Atenção: o Ergogen gera a PCB **sem trilhas**. As versões roteadas são os arquivos `*_finished.kicad_pcb`.
> Se regenerar, é preciso rotear de novo no KiCad.
