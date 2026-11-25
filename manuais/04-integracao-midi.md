# Manual 04 — Projeto de integração MIDI e WMK Music Studio
Walney Moreira Klein — walneyk@hotmail.com

Referências: [OsuManiaImporter.hx](../source/funkin/data/song/importer/OsuManiaImporter.hx), [StepManiaImporter.hx](../source/funkin/data/song/importer/StepManiaImporter.hx), [SongData.hx](../source/funkin/data/song/SongData.hx).

## O que já existe
Os importadores convertem dados externos em SongMetadata e SongChartData. OsuManiaImporter reconstrói mudanças de BPM e converte objetos de hit. StepManiaImporter interpreta linhas de metadados, BPM, paradas e medidas. São referências para um futuro importador MIDI, não prova de suporte MIDI nativo.

## Proposta WMK
1. Ler MIDI com Tonejs/Midi, mantendo mapa de tempos.
2. Selecionar faixas de melodia, adversário e jogador.
3. Converter segundos de início e duração em ms.
4. Mapear alturas para quatro direções por regra configurável; d = direção + 4 × lado.
5. Resolver colisões de notas simultâneas na mesma pista, reduzir densidade por dificuldade e permitir revisão manual.
6. Exportar chart e metadados conforme versões do fork; renderizar ou importar áudio correspondente separadamente.
7. Calibrar offset e testar a fase.

Exemplo didático: nota MIDI 60, início 1.5 s, duração 0.5 s, lado jogador, direção 0 => {"t":1500,"d":0,"l":500}. O mapeamento de altura é decisão de design, não regra original.

## Áudio
MIDI não contém gravação vocal. Para música própria em MP3/WAV, alinhar chart ao áudio e converter para o formato aceito pelo jogo quando necessário. Transcrição automática é aproximada e precisa de revisão. Separar vozes e instrumental pode ajudar, mas não é requisito de um protótipo externo.

## Validação proposta
Testar BPM constante e variável, notas simultâneas, sustain, pausa, seek, offset, dificuldades, arquivos inválidos e compatibilidade no editor. Não há implementação nem build validado neste manual.
