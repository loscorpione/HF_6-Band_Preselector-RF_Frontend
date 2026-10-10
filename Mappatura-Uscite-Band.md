# Configurazione delle uscite digitali per la selezione dei filtri

## Introduzione

Se si utilizza il preselettore **HF 6-Band Preselector RF Frontend** insieme al progetto [VFO/BFO HF ESP32 + Si5351](https://github.com/loscorpione/VFO-BFO-HF-ESP32-Si5351), è necessario configurare la mappatura delle uscite digitali in funzione della frequenza selezionata sul VFO.

Il firmware del VFO deve generare il codice binario corrispondente alla banda di frequenza utilizzata, in modo da comandare il circuito di selezione dei filtri del preselettore.

## Dove modificare il firmware

La configurazione si trova nel file `DigiOUT.cpp`, all'interno della funzione:

```cpp
void updateModeOutputs()
```

In questa funzione viene verificato il valore di `displayedFrequency` e vengono impostate le uscite digitali in base alla banda di frequenza selezionata.

Per ogni filtro vengono definiti un limite inferiore e un limite superiore. Quando la frequenza rientra nell'intervallo specificato, alla variabile `outputState` viene assegnato il codice binario corrispondente.

**Attenzione:** gli intervalli di frequenza riportati nel codice devono corrispondere alle bande effettive dei filtri installati nel preselettore.

## Tabella di selezione dei filtri

La configurazione di esempio prevede sei filtri passa-banda e una condizione aggiuntiva per le frequenze fuori banda.

| Frequenza selezionata | Codice binario | Banda |
|---|---|---|
| 200 kHz – 1 MHz | `000` | Filtro 1 |
| 1 – 2 MHz | `001` | Filtro 2 |
| 2 – 4 MHz | `010` | Filtro 3 |
| 4 – 8 MHz | `011` | Filtro 4 |
| 8 – 15 MHz | `100` | Filtro 5 |
| 15 – 30 MHz | `101` | Filtro 6 |
| Fuori banda | `110` | Nessun filtro attivo |

I codici binari sono quelli impostati nel firmware e devono essere compatibili con il circuito digitale di decodifica del preselettore.

## Esempio di configurazione nel codice

Il seguente frammento mostra come vengono associate le frequenze ai codici binari:

```cpp
void updateModeOutputs() {
    uint8_t outputState = 0;

    if (displayedFrequency >= 200000 &&
        displayedFrequency < 1000000) {
        outputState = 0b000;
    }
    else if (displayedFrequency >= 1000000 &&
             displayedFrequency < 2000000) {
        outputState = 0b001;
    }
    else if (displayedFrequency >= 2000000 &&
             displayedFrequency < 4000000) {
        outputState = 0b010;
    }
    else if (displayedFrequency >= 4000000 &&
             displayedFrequency < 8000000) {
        outputState = 0b011;
    }
    else if (displayedFrequency >= 8000000 &&
             displayedFrequency < 15000000) {
        outputState = 0b100;
    }
    else if (displayedFrequency >= 15000000 &&
             displayedFrequency <= 30000000) {
        outputState = 0b101;
    }
    else {
        outputState = 0b110;
    }

    // La gestione delle uscite digitali prosegue qui.
}
```

*Nota: questo è un esempio della logica di mappatura. Per mantenere il funzionamento del firmware originale, conservare anche le istruzioni successive della funzione che trasferiscono `outputState` alle uscite fisiche.*

## Come personalizzare la mappatura

Se si utilizzano filtri con intervalli di frequenza differenti, è possibile modificare i limiti delle condizioni `if` e `else if`.

Per ogni filtro occorre:

1. Impostare la frequenza minima di attivazione.
2. Impostare la frequenza massima di attivazione.
3. Assegnare il codice binario previsto dal circuito di decodifica.
4. Verificare che gli intervalli non si sovrappongano e che non rimangano intervalli indesiderati.
5. Verificare il corretto funzionamento delle uscite digitali e dei relè.

Gli estremi degli intervalli devono essere scelti con attenzione per evitare ambiguità nei punti di passaggio tra una banda e la successiva.

## Verifica del funzionamento

Dopo aver modificato e caricato il firmware:

- Selezionare frequenze appartenenti a ciascuna delle sei bande.
- Verificare che il codice binario generato sia quello previsto.
- Controllare che venga attivato il filtro corrispondente.
- Provare anche frequenze inferiori a 200 kHz e superiori a 30 MHz per verificare la condizione fuori banda.

Per maggiori informazioni sul circuito RF e sul relativo sistema di selezione, consultare il [repository HF 6-Band Preselector RF Frontend](https://github.com/loscorpione/HF-6-Band_Preselector-RF_Frontend).
