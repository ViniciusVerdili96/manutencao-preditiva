# Resultados e desafios

[Voltar ao início](../README.md)

## Medições no ventilador real

| Medida | Velocidade 1 (normal) | Velocidade 2 (simula o defeito) |
|---|---|---|
| Vibração RMS | 170 a 185 mg | 230 a 250 mg |
| Mudança no espectro | 1 % a 9 % | 19 % a 50 % |
| Leituras por segundo | cerca de 1000 | cerca de 1000 |

- **Gravidade medida em repouso:** 0,98 g, o que confirma a escala do sensor.
- **Picos do espectro:** dois picos de altura parecida, perto de 45 Hz e de 120 Hz. O de 120 Hz
  corresponde ao dobro da frequência da rede elétrica.
- **Referência aprendida:** RMS de 173,5 mg, com limite de 208,2 mg, e limite de 12 % na mudança do
  espectro.

## Desafios resolvidos

Os primeiros testes com o hardware real mostraram problemas que não aparecem com dados simulados.

| Problema observado | Causa encontrada | Solução |
|---|---|---|
| O sensor não respondia em nenhum endereço, com o LED aceso | O chip trava e só volta depois de ficar sem energia | Orientação na tela para religar o cabo USB, e menos escritas no sensor durante a leitura |
| Os eixos apareciam trocados de vez em quando | O chip do módulo não é um MPU-6050 original: ele se identifica como `0x74` e guarda só 512 bytes na fila, não 1024 | Limite da fila ajustado para o menor modelo |
| Cerca de 14 falhas de comunicação por segundo | Na placa, o pino que envia dados pela USB fica ao lado do pino de relógio do sensor, e os dois ativos ao mesmo tempo geravam pulsos falsos | O ESP32 alterna: lê o sensor só com a transmissão USB terminada |
| Leituras de 4000 mg que não eram vibração | Uma falha isolada desalinhava a fila, e os bytes seguintes eram lidos fora de ordem | O firmware realinha a fila e descarta lotes impossíveis |
| Alarmes falsos na velocidade 1 | O limite inicial do espectro, de 5 %, era menor que a variação normal do ventilador | Limite de 12 %, calibração de 20 s e limites pela mediana |

## Limitações e próximos passos

- **Comunicação:** ainda há falhas esporádicas de comunicação no protótipo, provavelmente de contato
  nos fios. A análise descarta as leituras ruins, mas falta um teste longo para medir a taxa de
  alarmes falsos.
- **Faixa de frequência:** o sensor mede bem até cerca de 200 Hz neste projeto. Defeitos de rolamento
  em estágio inicial costumam aparecer em frequências mais altas, medidas com sensores
  piezoelétricos.
- **Validação:** o próximo passo é testar com um defeito físico real, como um desbalanceamento
  controlado, mantendo a mesma velocidade.
- **Conexão sem fio:** o envio por Bluetooth está planejado, para dispensar o cabo de dados.
