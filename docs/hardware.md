# Hardware

[Voltar ao início](../README.md)

![Sensor montado no ventilador](../media/montagem.jpg)

## Materiais

| Item | Uso |
|---|---|
| ESP32 WROOM-32, placa de 30 pinos | Lê o sensor e envia os dados pela USB |
| Módulo GY-521, compatível com o MPU-6050 | Acelerômetro de 3 eixos |
| Cabo micro USB de dados | Alimentação e comunicação com o computador |
| Jumpers e solda na barra de pinos do sensor | Ligações |
| Ventilador Arno Turbo Silêncio Maxx | Máquina monitorada |

## Ligações

| Sensor | ESP32 |
|---|---|
| VCC | 3V3 |
| GND | GND |
| SCL | D22 (GPIO22) |
| SDA | D21 (GPIO21) |

O sensor fica preso à parte rígida mais próxima do motor. O ESP32 fica fora do ventilador e leva só o
sensor até ele.

## Configuração do sensor

| Parâmetro | Valor |
|---|---|
| Taxa de amostragem | 1000 leituras por segundo |
| Faixa | ±4 g (8192 contagens por g) |
| Filtro interno | Passa-baixa, com análise até 200 Hz |
| Comunicação | I2C a 400 kHz, USB a 921600 bps |
