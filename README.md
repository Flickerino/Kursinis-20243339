# Kursinis-20243339

## 1. Tema
**Gimtadienių priminimo sistema** – programa, leidžianti vartotojams registruotis, įrašyti draugų ir šeimos narių gimtadienius, peržiūrėti juos bei gauti priminimus, jei šiandien kažkieno gimtadienis.

## 2. Objektinio programavimo principai

| OOP principas    | Įgyvendinimas                                                                   |
|------------------|----------------------------------------------------------------------------------|
| **Abstrakcija**  | `Asmuo` – abstrakti klasė su metodu `gauti_informacija()`                       |
| **Paveldėjimas** | `Draugas` ir `SeimosNarys` paveldi `Asmuo`                                      |
| **Polimorfizmas**| Abi paveldėtos klasės turi savitą `gauti_informacija()` implementaciją          |
| **Kompozicija**  | `Vartotojas` turi `GimtadieniuValdiklis` objektą                                |
| **Encapsulation**| Kiekviena klasė slepia duomenis ir metodus susijusius su jos atsakomybėmis      |
| **Singleton**    | `GimtadieniuPrograma` – tik vienas egzempliorius per paleidimą                  |

## 3. Duomenų saugojimas

- Kiekvieno vartotojo gimtadieniai saugomi atskirame `.csv` faile: `vardas_gimtadieniai.csv`.
- Naudojamas `csv.writer` ir `csv.reader` duomenų įrašymui ir nuskaitymui.

## 4. Funkcionalumas

- Vartotojų registracija ir prisijungimas
- Draugų ir šeimos narių gimtadienių pridėjimas
- Gimtadienių šalinimas ir peržiūra
- Priminimas, jei šiandien gimtadienis

## 5. Naudojimas

### Pavyzdys:

```text
--- Gimtadienių Priminimas ---
1. Prisijungti kaip vartotojas
2. Registruoti naują vartotoją
3. Išeiti
```

### Gimtadienio įrašymas:

```text
Vardas: Jonas
Gimtadienio data (YYYY-MM-DD): 1990-05-01
Tipas (draugas/seima): draugas
```

## 6. Išvados

Šio darbo metu pavyko sėkmingai sukurti funkcinę gimtadienių priminimo sistemą, pritaikant pagrindinius objektinio programavimo principus bei dizaino šablonus. Vienas iš iššūkių buvo tinkamai panaudoti abstrakčią klasę ir užtikrinti polimorfizmo veikimą tarp skirtingų kontaktų tipų. Taip pat reikėjo užtikrinti duomenų išsaugojimą naudojant .csv formatą ir pasirūpinti vartotojo sąsajos paprastumu.

---
