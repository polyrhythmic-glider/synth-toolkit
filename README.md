# Synth Toolkit

Sintetizzatore hardware ESP32 + Daisy Seed di Polyrhythmic Glider.
La ESP32 gestisce interfaccia, controlli, preset e pattern; Daisy esegue
sequencer, sintesi, effetti e uscita audio stereo. I due firmware condividono
il protocollo UART e vanno aggiornati insieme.

## Contenuto

| Percorso | Contenuto |
| --- | --- |
| [`TheSynthProject/`](TheSynthProject/) | Firmware Arduino ESP32, UI, control surface e protocollo UART |
| [`DaisySynth/`](DaisySynth/) | Firmware Daisy Seed, motore DSP e test host |
| [`references/`](references/) | Riferimenti architetturali Daisy, esclusi dalle build |
| [`tools/`](tools/) | Generazione del database clangd ESP32 + Daisy |

## Avvio

1. Seguire il [cablaggio integrato](TheSynthProject/WIRING_INTEGRATED.md) e
   i [collegamenti Daisy](TheSynthProject/DAISY_MIGRATION.md).
2. Aprire `TheSynthProject/TheSynthProject.ino` in Arduino IDE e selezionare
   **ESP32 Dev Module** (`esp32:esp32:esp32`). Librerie e versioni verificate
   sono nel [README ESP32](TheSynthProject/README.md#build-verificata).
3. Installare la toolchain ARM e preparare libDaisy/DaisySP. Il layout predefinito è:

   ```text
   Documenti/
   ├── synth-toolkit/
   └── DaisyExamples/
       ├── libDaisy/
       └── DaisySP/
   ```

4. Dalla radice della repo compilare Daisy:

   ```sh
   make -C DaisySynth -j2
   ```

   Per caricarla, mettere Daisy in DFU ed eseguire `make -C DaisySynth program-dfu`.
   Se le librerie sono altrove, passare `LIBDAISY_DIR=/percorso/libDaisy`
   e `DAISYSP_DIR=/percorso/DaisySP` a `make`.

## Verifiche senza schede

Con g++, make e le librerie Daisy disponibili:

```sh
make -C DaisySynth test-link-host
make -C DaisySynth test-host
```

I test coprono protocollo/recupero UART e comportamento del motore DSP.
Il collaudo fisico è descritto nei README dei firmware.

## Editor

Aprire [`synth-toolkit.code-workspace`](synth-toolkit.code-workspace) in
VSCodium/VS Code. `./tools/generate_compile_commands.sh` genera il database
clangd per entrambe le toolchain. Dettagli in [`DEVELOPMENT.md`](DEVELOPMENT.md).

## Provenienza

Estratto da [hardware-toolkit](https://github.com/polyrhythmic-glider/hardware-toolkit)
a partire dal commit `0e697fe`, conservando la cronologia Git dei percorsi synth.
I test hardware autonomi, Neko/e-LCD e il vecchio sketch quattro OLED restano
in quella repository. I test specifici del synth sono accanto ai rispettivi firmware.
