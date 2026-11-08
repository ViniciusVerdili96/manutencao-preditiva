# Results and challenges

[Back to the main page](../README.md)

## Measurements on the real fan

| Measurement | Speed 1 (normal) | Speed 2 (stands in for the fault) |
|---|---|---|
| Vibration RMS | 170 to 185 mg | 230 to 250 mg |
| Spectrum change | 1% to 9% | 19% to 50% |
| Readings per second | about 1,000 | about 1,000 |

- **Gravity measured at rest:** 0.98 g, which confirms the sensor's scale.
- **Spectrum peaks:** two peaks of similar height, near 45 Hz and 120 Hz. The 120 Hz peak is twice the
  frequency of the power grid.
- **Learned baseline:** RMS of 173.5 mg with a limit of 208.2 mg, and a 12% limit on the spectrum
  change.

## Challenges solved

The first tests with the real hardware revealed problems that never show up with simulated data.

| Problem observed | Cause found | Solution |
|---|---|---|
| The sensor did not respond at any address, with its LED on | The chip locks up and only recovers after losing power | An on-screen instruction to replug the USB cable, and fewer writes to the sensor while reading |
| The axes were occasionally swapped | The module's chip is not an original MPU-6050: it identifies as `0x74` and its FIFO holds only 512 bytes, not 1,024 | FIFO limit set for the smaller model |
| About 14 communication failures per second | On the board, the pin that sends data over USB sits next to the sensor's clock pin, and with both active at once they produced false pulses | The ESP32 takes turns: it reads the sensor only after the USB transmission finishes |
| Readings of 4,000 mg that were not vibration | A single failure misaligned the FIFO, and the following bytes were read out of order | The firmware realigns the FIFO and discards impossible batches |
| False alarms at speed 1 | The initial spectrum limit of 5% was lower than the fan's normal variation | A 12% limit, a 20 s calibration, and median-based limits |

## Limitations and next steps

- **Communication:** the prototype still has sporadic communication failures, probably from contact
  issues in the wiring. The analysis discards bad readings, but a long test is still needed to
  measure the false alarm rate.
- **Frequency range:** in this project the sensor measures well up to about 200 Hz. Early-stage
  bearing faults usually show up at higher frequencies, measured with piezoelectric sensors.
- **Validation:** the next step is testing with a real physical fault, such as a controlled
  imbalance, at the same speed.
- **Wireless link:** sending the data over Bluetooth is planned, to remove the need for the data
  cable.
