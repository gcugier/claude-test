# ecoNET300 API — Dokumentacja parametrów

## Informacje o instalacji

| Parametr | Wartość |
|----------|---------|
| Regulator | ecoMAX960D |
| Moduł sieciowy | ecoNET300 (router mr3020-v3) |
| UID | 31TPK84Z3EE9I1C300380 |
| Firmware ecoNET | 3.2.3886 |
| Firmware regulatora | 3.11.13.C1 |
| Firmware panelu | 3.20.13 |
| Firmware lambda | 0.97.10 |
| Schemat instalacji | 32 (CO + CWU + bufor + mixer) |
| Adres IP | 192.168.50.144 |
| Protokół | em |
| Interwał odświeżania | 5s |

## Połączenie z API

```bash
# Autoryzacja: Basic Auth
curl -u admin:admin http://192.168.50.144/econet/regParams

# Dostępne endpointy:
# /econet/regParams     — dane bieżące (temperatury, statusy, pompy)
# /econet/sysParams     — konfiguracja systemu, alarmy, wersje
# /econet/editParams    — parametry edytowalne
# /econet/rmParamsData  — pełne menu serwisowe (150+ parametrów)
```

---

## Parametry bieżące (regParams → curr)

### Temperatury

| Klucz API | Opis | Jednostka | Zakres |
|-----------|------|-----------|--------|
| `tempCO` | Temperatura kotła (woda zasilająca CO) | °C | 0–100 |
| `tempCOSet` | Temperatura zadana kotła | °C | 70–90 |
| `tempCWU` | Temperatura ciepłej wody użytkowej | °C | 0–100 |
| `tempCWUSet` | Temperatura zadana CWU | °C | 55–80 |
| `tempFlueGas` | Temperatura spalin w kominie | °C | 0–450 |
| `tempUpperBuffer` | Temperatura góry bufora ciepła | °C | 0–100 |
| `tempLowerBuffer` | Temperatura dołu bufora ciepła | °C | 0–100 |
| `tempLowerSolar` | Temperatura dolna solar (ten sam czujnik co dolny bufor) | °C | 0–100 |
| `tempFeeder` | Temperatura podajnika (zabezpieczenie cofnięcia ognia) | °C | 0–100 |
| `tempOpticalSensor` | Temperatura sensor optyczny (żar w komorze spalania) | °C | 0–100 |
| `mixerTemp1` | Temperatura za mieszaczem 1 (obieg grzewczy) | °C | 0–100 |
| `mixerTemp2` | Temperatura za mieszaczem 2 | °C | null = niepodłączony |
| `mixerTemp3` | Temperatura za mieszaczem 3 | °C | null = niepodłączony |
| `mixerTemp4` | Temperatura za mieszaczem 4 | °C | null = niepodłączony |
| `mixerSetTemp1` | Temperatura zadana mieszacza 1 | °C | 20–90 |
| `mixerSetTemp2` | Temperatura zadana mieszacza 2 | °C | 20–55 |
| `mixerSetTemp3` | Temperatura zadana mieszacza 3 | °C | 20–55 |
| `mixerSetTemp4` | Temperatura zadana mieszacza 4 | °C | 20–55 |

### Moc i paliwo

| Klucz API | Opis | Jednostka | Uwagi |
|-----------|------|-----------|-------|
| `boilerPower` | Moc kotła procentowa | % | Kalkulacja regulatora na podstawie spalin, wentylatora, lambda |
| `boilerPowerKW` | Moc kotła w kilowatach | kW | boilerPower × moc_nominalna / 100 |
| `fanPower` | Moc wentylatora nadmuchowego | % | 0–100 |
| `fuelStream` | Strumień paliwa | kg/h | Tylko pellet (podajnik). Dla drewna = 0 |
| `fuelConsum` | Zużycie paliwa skumulowane | kg | Tylko pellet. Dla drewna = 0 |
| `fuelLevel` | Poziom paliwa w zasobniku | — | Tylko pellet |

### Sonda lambda

| Klucz API | Opis | Jednostka | Uwagi |
|-----------|------|-----------|-------|
| `lambdaLevel` | Aktualny poziom tlenu w spalinach | ‰ | Pomiar co ~1s |
| `lambdaSet` | Zadany poziom tlenu | ‰ | Cel regulacji |
| `lambdaStatus` | Status sondy | — | 0=stop, 1=rozgrzewanie, 2=praca |

### Pompy

| Klucz API (config) | Klucz API (status) | Opis |
|--------------------|--------------------|------|
| `pumpCO` | `pumpCOWorks` | Pompa obiegowa CO |
| `pumpCWU` | `pumpCWUWorks` | Pompa ładująca CWU |
| `pumpCirculation` | `pumpCirculationWorks` | Pompa cyrkulacji CWU |
| `pumpSolar` | `pumpSolarWorks` | Pompa solarna |
| `pumpFireplace` | `pumpFireplaceWorks` | Pompa kominka |
| — | `mixerPumpWorks1` | Pompa mieszacza 1 |
| — | `mixerPumpWorks2` | Pompa mieszacza 2 |
| — | `mixerPumpWorks3` | Pompa mieszacza 3 |
| — | `mixerPumpWorks4` | Pompa mieszacza 4 |

### Wentylatory i podajnik

| Klucz API (config) | Klucz API (status) | Opis |
|--------------------|--------------------|------|
| `fan` | `fanWorks` | Wentylator nadmuchowy (spalanie) |
| `fan2Exhaust` | `fan2ExhaustWorks` | Wentylator wyciągowy spalin |
| `feeder` | `feederWorks` | Podajnik paliwa główny |
| `feederOuter` | `feederOuterWorks` | Podajnik zewnętrzny |
| `feeder2Additional` | `feeder2AdditionalWorks` | Podajnik dodatkowy |
| `lighter` | `lighterWorks` | Zapalarka elektryczna |
| `blowFan1` | `blowFan1Active` | Wentylator nawiewny 1 |
| `blowFan2` | `blowFan2Active` | Wentylator nawiewny 2 |
| `alarmOutput` | `alarmOutputWorks` | Wyjście alarmowe |

### Tryb i status

| Klucz API | Opis | Wartości |
|-----------|------|---------|
| `mode` | Tryb pracy kotła | 0=OFF, 1=KINDLING (rozpalanie), 2=WORKING (praca), 3=SUPERVISION (nadzór), 4=PAUSED, 5=STANDBY, 6=BURNING_OFF (wygaszanie), 7=ALERT, 8=MANUAL, 9=UNSEALING |
| `statusCO` | Status obiegu CO | Bitmask (bit 4 = pompa CO aktywna) |
| `statusCWU` | Status obiegu CWU | 0 = temp. CWU osiągnięta |
| `thermostat` | Termostat pokojowy | 0=nie żąda, 1=żąda ciepła |
| `transmission` | Jakość transmisji do regulatora | 1–10 |
| `contactGZC` / `contactGZCActive` | Styk zewnętrznego źródła ciepła | true/false |
| `outerBoiler` / `outerBoilerWorks` | Kocioł zewnętrzny (bivalentny) | true/false |

---

## Algorytmy sterowania

### Interpolacja mocy (3-punktowa)

Serwisant konfiguruje 3 punkty kalibracyjne pod konkretne paliwo:

| Punkt | Parametry wentylatora | Parametry podajnika |
|-------|----------------------|---------------------|
| 30% mocy | `airflow_power_30` | `fuel_feeding_work_30` / `fuel_feeding_pause_30` |
| 50% mocy | `airflow_power_50` | `fuel_feeding_work_50` / `fuel_feeding_pause_50` |
| 100% mocy | `airflow_power_100` | `fuel_feeding_work_100` / `fuel_feeding_pause_100` |

Regulator interpoluje liniowo między punktami:

```
Dla żądanej mocy X% (np. 70%):
fanPower = value_50 + (value_100 - value_50) × (X - 50) / (100 - 50)
```

Efekt: płynna modulacja mocy, nie skokowa.

### Regulacja lambda (Fuzzy Logic)

Nie klasyczny PID — logika rozmyta uwzględnia:

1. Odchyłkę: `lambdaLevel - lambdaSet`
2. Trend zmian (rośnie / maleje / stabilny)
3. Fazę pracy (rozpalanie → inna strategia niż praca nominalna)

Wyjście: korekcja `fanPower` w zakresie `lambda_correction_range` (±%)

```
Przykład:
  fanPower z interpolacji = 61%
  lambdaLevel = 120‰, lambdaSet = 80‰ → za dużo O₂
  Korekcja: -5%
  Wynikowy fanPower = 56%
```

### Pętla CO (histereza)

```
Jeśli tempCO < (tempCOSet - histereza):
    pumpCO = ON, żądaj mocy od kotła
Jeśli tempCO > tempCOSet:
    pumpCO = OFF (lub zmniejsz moc)
```

### Pętla CWU (priorytet nad CO)

```
Jeśli tempCWU < tempCWUSet:
    Przejmij ciepło z kotła → pumpCWU = ON
    Opcjonalnie: pumpCO = OFF (priorytet CWU)
Jeśli tempCWU >= tempCWUSet:
    pumpCWU = OFF, powrót do CO
```

### Pętla bufora

```
Ładowanie:  tempUpperBuffer < próg → kieruj ciepło do bufora
Rozładowanie: tempLowerBuffer > próg → pobieraj z bufora
ΔT = tempUpperBuffer - tempLowerBuffer (stratyfikacja)
```

### Pętla mieszacza

```
Mieszacz reguluje temperaturę za sobą do mixerSetTemp
Miesza gorącą wodę z bufora/kotła z powrotem (obieg podłogowy/grzejnikowy)
```

---

## Parametry instalacji — paliwo

| Parametr | Wartość |
|----------|---------|
| Moc nominalna kotła | 26 kW |
| Typ paliwa | Drewno kawałkowe — sosna |
| Wilgotność | 15% |
| Kaloryczność sosny 15% | 4.3 kWh/kg |
| Sprawność kotła (praca nominalna) | ~89% |
| Sprawność kotła (podtrzymanie) | ~50% |
| Sprawność kotła (średnia sezonowa) | ~82% |
| Typowy załadunek | 25 kg |

### Obliczenie energii z załadunku

```
Energia brutto  = 25 kg × 4.3 kWh/kg           = 107.5 kWh
Straty (11%)    = 107.5 × 0.11                  = 11.8 kWh
Energia netto   = 107.5 × 0.89                  = 95.7 kWh

Czas palenia:
  100% mocy (26 kW) = 95.7 / 26  = 3h 40min
  70% mocy  (18 kW) = 95.7 / 18  = 5h 15min
  50% mocy  (13 kW) = 95.7 / 13  = 7h 20min
```

### Wzór na szacowanie zużycia drewna z API

```
Zużycie [kg] = ∫ boilerPowerKW × dt / (kaloryczność × sprawność)

Dla sosny 15%:
  Zużycie [kg] = Σ(boilerPowerKW × 5s) / 3600 / (4.3 × 0.89)
               = energia_kWh / 3.827
```

Regulator ecoMAX nie mierzy zużycia drewna bezpośrednio (brak podajnika).
Szacowanie możliwe tylko przez bilans energetyczny z całkowania `boilerPowerKW`.

### Tabela kaloryczności drewna (odniesienie)

| Gatunek | Gęstość [kg/m³] | kWh/kg (15%) | kWh/kg (20%) | kWh/kg (30%) |
|---------|-----------------|--------------|--------------|--------------|
| Buk | 680 | 4.2 | 3.8 | 3.2 |
| Dąb | 670 | 4.1 | 3.7 | 3.1 |
| Brzoza | 610 | 4.2 | 3.8 | 3.2 |
| Sosna | 490 | 4.3 | 4.1 | 3.4 |
| Świerk | 430 | 4.3 | 4.1 | 3.4 |
| Olcha | 510 | 3.9 | 3.5 | 2.9 |
| Grab | 730 | 4.2 | 3.8 | 3.2 |

---

## Historia alarmów (z sysParams)

| Data od | Data do | Kod | Opis |
|---------|---------|-----|------|
| 2026-03-25 15:43 | 2026-03-25 16:10 | 0 | Brak czujnika / utrata komunikacji |
| 2026-03-25 08:14 | 2026-03-25 09:02 | 0 | Brak czujnika / utrata komunikacji |
| 2026-03-22 12:16 | 2026-03-22 12:17 | 0 | Brak czujnika / utrata komunikacji |
| 2026-03-14 08:43 | 2026-03-14 09:35 | 0 | Brak czujnika / utrata komunikacji |
| 2026-03-06 10:33 | 2026-03-06 11:54 | 0 | Brak czujnika / utrata komunikacji |

---

## Kody alarmów ecoMAX (odniesienie)

| Kod | Opis |
|-----|------|
| 0 | Utrata komunikacji / brak czujnika |
| 1 | Zanik zasilania |
| 2 | Awaria czujnika temp. kotła |
| 3 | Awaria czujnika temp. podajnika |
| 4 | Awaria czujnika temp. CWU |
| 5 | Awaria czujnika temp. spalin |
| 6 | Przekroczenie temp. spalin |
| 7 | Przekroczenie temp. podajnika (cofnięcie ognia) |
| 8 | Przekroczenie temp. kotła |
| 10 | Awaria wentylatora |
| 18 | Awaria czujnika ciśnienia |

---

## Schemat instalacji (schemaID: 32)

```
                    ┌──────────┐
                    │  KOMIN   │
                    │ spaliny  │
                    └────┬─────┘
                         │ tempFlueGas
                    ┌────┴─────┐
    drewno 25kg →   │  KOCIOŁ  │ ← tempOpticalSensor (żar)
    (sosna 15%)     │ ecoMAX   │ ← tempFeeder (podajnik)
                    │  960D    │
                    │  26 kW   │
                    └────┬─────┘
                         │ tempCO (75.6°C)
                    ┌────┴─────┐
                    │  BUFOR   │ ← tempUpperBuffer (72.4°C)
                    │  500 L   │
                    │          │ ← tempLowerBuffer (36.7°C)
                    └──┬───┬───┘
                       │   │
              ┌────────┘   └────────┐
              │                     │
         ┌────┴─────┐        ┌─────┴────┐
         │MIESZACZ 1│        │   CWU    │
         │ mixer1   │        │ 60.7°C   │
         └────┬─────┘        └──────────┘
              │ mixerTemp1 (67.1°C)
              │
         ┌────┴─────┐
         │ GRZEJNIKI│
         │ / PODŁOGA│
         └──────────┘
```
