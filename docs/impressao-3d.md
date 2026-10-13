# Impressão 3D

O case é o **original do Caldera**, sem alterações. Ele é publicado no MakerWorld, que tem licença própria, então os arquivos
**não ficam neste repositório**: baixe lá e envie para a loja de impressão.

- Case original (com parafusos): <https://makerworld.com/en/models/545431>
- Alternativa com encaixe/clips, sem parafusos (remix do matdurand): <https://makerworld.com/en/models/1240609-caldera-split-keyboard-case-clippable-design>

**Escolha: case original com parafusos.** O encaixe por clips depende muito da calibração da impressora,
e numa loja de impressão a gente não controla isso. Parafuso funciona em qualquer impressão.

## Peças

| Peça | Qtd | Observação |
|---|---|---|
| Top esquerdo (já inclui o plate onde os switches encaixam) | 1 | |
| Top direito | 1 | |
| Bottom esquerdo | 1 | |
| Bottom direito | 1 | |
| *(opcional)* Plate de teste: [`ergogen/output/cases/plate.stl`](../ergogen/output/cases/plate.stl) | 1 | Plate de 1,2 mm do lado esquerdo. É barato e serve para conferir se um switch encaixa firme antes de imprimir tudo |

## O que pedir à loja

Copie e envie:

> - Material: **PLA** (PLA+ ou PETG também servem)
> - Orientação: **face superior para baixo** (top e bottom), como recomendado pelo autor
> - **Paredes grossas** (4 perímetros ou mais) e **sem infill (0%)**. As peças são finas e ficam praticamente sólidas
> - Altura de camada: 0,2 mm (0,16 mm se quiser um acabamento melhor)
> - Suportes: só se o fatiador indicar como necessário. Avisar antes, porque suporte na face visível deixa marca
> - Furos M2: **não alargar nem fechar**. O autor deixou os furos um pouco largos de propósito, para aceitar espaçador ou inserto
> - Cor: a combinar (o original é PLA branco mármore)

Material estimado: ~100–150 g de PLA para as 4 peças.

## Ao receber

1. Confira se um switch Choc encaixa no plate do top com um "clique" firme, sem folga.
2. Teste a PCB dentro do top (só apoiada) para ver se os furos de fixação batem.
3. **Meça a cavidade da bateria** no bottom e só então compre a bateria (ver [hardware.md](hardware.md)).
