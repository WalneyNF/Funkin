# Manual 01 — Tempo musical e Conductor
## WMK Funk Battle
Walney Moreira Klein — walneyk@hotmail.com

Fonte: [Conductor.hx](../source/funkin/Conductor.hx), [SongData.hx](../source/funkin/data/song/SongData.hx).

## Fundamentos
BPM é a quantidade de semínimas por minuto no modelo do código. A duração da batida em ms é 60000/BPM × 4/denominador. O compasso dura essa batida × numerador. O step dura a batida dividida por STEPS_PER_BEAT (quatro no modelo padrão).

| BPM | Batida 4/4 (ms) | Step (ms) | Compasso (ms) |
|---|---:|---:|---:|
| 60 | 1000 | 250 | 4000 |
| 120 | 500 | 125 | 2000 |
| 150 | 400 | 100 | 1600 |

## Relógio real
Conductor.update lê FlxG.sound.music.time quando não recebe uma posição explícita. Aplica combinedOffset quando solicitado, limita a posição durante a reprodução e seleciona a mudança temporal ativa. Calcula currentStepTime, currentBeatTime e currentMeasureTime, e dispara sinais quando seus índices inteiros mudam. songPositionDelta estima avanço entre atualizações do áudio; não é a fonte primária do relógio.

## Mudança de andamento
mapTimeChanges ordena os eventos, limita timestamps negativos a zero e acumula beatTime usando o andamento e denominador anteriores. Exemplo em 4/4: 120 BPM até 4000 ms = 8 batidas. A partir daí, 150 BPM. Em 4800 ms temos 10 batidas acumuladas, não 12.

```json
[{"t":0,"bpm":120,"n":4,"d":4},{"t":4000,"bpm":150,"n":4,"d":4}]
```

## Exercícios
1. Calcule batida, step e compasso em 90 BPM, 4/4. Resposta: 666,667; 166,667; 2666,667 ms.
2. Em 120 BPM, 6/8, a unidade de batida do código dura 250 ms e o compasso 1500 ms. Isso não equivale necessariamente à pulsação musical percebida em dois tempos compostos.
3. Localize onBeatHit, onStepHit e onMeasureHit no código.
4. Compare o relógio do áudio com um acumulador de elapsed durante uma pausa.

## Limites
O campo beatTuplets existe no esquema; não se deve presumir que todas as rotinas respeitem subdivisões arbitrárias. A fórmula do getter stepLengthMs usa STEPS_PER_BEAT. Não alteramos nem executamos o motor.
