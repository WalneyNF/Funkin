# Manual 02 — Criar e interpretar charts
Walney Moreira Klein — walneyk@hotmail.com

Fontes: [SongData.hx](../source/funkin/data/song/SongData.hx), [SongRegistry.hx](../source/funkin/data/song/SongRegistry.hx), [PlayState.hx](../source/funkin/play/PlayState.hx).

## Arquivos
O registro procura metadados de músicas com sufixo -metadata.json em songs/. SongRegistry declara metadata 2.2.4 e chart 2.0.0. Metadados e chart são separados: o primeiro descreve a música e configuração temporal; o segundo guarda scrollSpeed, events e notes por dificuldade.

## Nota
t: instante; d: direção e lado; l: duração sustentada (zero para toque simples); k: tipo opcional; p: parâmetros opcionais. Nos caminhos analisados, os tempos usados no jogo são em ms. Direção = d % 4; lado = divisão inteira de d por 4. 0–3 jogador, 4–7 adversário. Ordem: esquerda, baixo, cima, direita.

```json
{"version":"2.0.0","scrollSpeed":{"normal":1},"events":[],"notes":{"normal":[{"t":500,"d":4,"l":0},{"t":1000,"d":0,"l":0},{"t":1500,"d":2,"l":500}]},"generatedBy":"WMK"}
```

Fragmento didático: não é pacote completo de música. Ainda são necessários metadados, áudio e recursos válidos.

## Leitura
PlayState.generateSong prepara vocais quando aplicável e chama regenNoteData. Esta rotina recebe currentChart, dispara SongLoadScriptEvent, usa notas e eventos resultantes, ignora notas anteriores ao início selecionado e distribui as notas por strumline. Notas pontuáveis do jogador entram no total esperado.

## Criação manual
Escolha BPM e áudio, marque os instantes, atribua direção e lado, defina sustains, teste no editor e ajuste a sincronização. Chart não representa diretamente altura musical. Uma nota MIDI C4 não implica uma seta específica.

## Exercício
A 120 BPM, crie quatro toques alternados em 0, 500, 1000 e 1500 ms, usando d=4,0,5,1. Depois crie um sustain de 1000 ms. Confira no editor após obter build funcional.
