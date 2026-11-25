# Manual 03 — Renderização, entrada e pontuação
Walney Moreira Klein — walneyk@hotmail.com

Fontes: [Strumline.hx](../source/funkin/play/notes/Strumline.hx), [PlayState.hx](../source/funkin/play/PlayState.hx), [Scoring.hx](../source/funkin/play/scoring/Scoring.hx).

## Renderização
updateNotes usa janela de renderização futura e nextNoteIndex para não percorrer ou remover toda a lista a cada frame. Cria sprites apenas quando próximos, cria SustainTrail se length > 0, e calcula posição via GRhythmUtil.getNoteY. Notas perdidas fora da tela são removidas. A fórmula didática y = alvo + (tempoNota - tempoAtual) × velocidade não reproduz todos os ajustes do motor original.

## Entrada
processInputQueue agrupa notas elegíveis por direção, consome eventos de pressão, seleciona primeira nota não lowPriority (ou primeira disponível), chama goodNoteHit e remove a candidata. Trata soltura separadamente. Ghost tapping depende da configuração e compilação.

## Latência
goodNoteHit calcula latência da fila em nanossegundos, converte para ms e usa Std.int(songPosition - note.time - inputLatencyMs). Exemplo: posição 2035, nota 2000, fila 15 ms => erro +20 ms. Isso não mede automaticamente toda latência física de áudio e teclado.

## PBOT1
Erro absoluto até 45 ms: sick; até 90: good; até 135: bad; até 160: shit; acima: miss. O limiar killer está declarado, mas sua classificação está comentada. Erro menor que 5 ms recebe 500; acima de 160 recebe -100; demais valores usam Std.int(500 × (1 - 1/(1+exp(-0.080×(erro-54.99)))) + 9). Pontuação e classificação são cálculos distintos. A função tem transição discreta no limiar de 5 ms.
O evento HitNoteScriptEvent pode alterar ou cancelar o processamento; applyScore aplica pontuação, julgamento, vida e combo. Sustains possuem tratamento próprio, não equivalem a vários taps.

## Exercícios
Calcule a classificação para erros -20, +70, +120, +150, +180 ms: sick, good, bad, shit, miss. Teste o efeito de compensar 15 ms de fila. Compare notas longas com taps no editor.
