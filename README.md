# Kursinis-20243339

# Birthday Reminder - Ataskaita

## 1. Tema
**Birthday reminder** – programa, leidžianti vartotojams registruotis, įrašyti draugų ir šeimos narių gimtadienius, peržiūrėti juos bei gauti priminimus, jei šiandien kažkieno gimtadienis.

## 2. Objektinio programavimo principai

| OOP principas     | Įgyvendinimas                                                                   |
|-------------------|---------------------------------------------------------------------------------|
| **Abstrakcija**   | `Asmuo` – abstrakti klasė su metodu `gauti_informacija()`                       |
| **Paveldėjimas**  | `Draugas` ir `SeimosNarys` paveldi `Asmuo`                                      |
| **Polimorfizmas** | Abi paveldėtos klasės turi savitą `gauti_informacija()` implementaciją          |
| **Kompozicija**   | `Vartotojas` turi `GimtadieniuValdiklis` objektą                                |
| **Inkapsuliacija**| Kiekviena klasė slepia duomenis ir metodus susijusius su jos atsakomybėmis      |
| **Singleton**     | `GimtadieniuPrograma` – tik vienas egzempliorius per paleidimą                  |

## 3. Duomenų saugojimas

- Kiekvieno vartotojo gimtadieniai saugomi atskirame `.csv` faile: `vardas_gimtadieniai.csv`.
- Naudojamas `csv.writer` ir `csv.reader` duomenų įrašymui ir nuskaitymui.

## 4. Funkcionalumas

- Vartotojų registracija ir prisijungimas
- Draugų ir šeimos narių gimtadienių pridėjimas
- Gimtadienių šalinimas ir peržiūra
- Priminimas, jei šiandien gimtadienis

* **1.1 Kas per programa?**

    Birthday Reminder yra Python programa, skirta padėti vartotojams sekti svarbius gimtadienius. Ji leidžia vartotojams kurti paskyras, pridėti, šalinti ir peržiūrėti gimtadienio įrašus bei gauti priminimus. Programa išsaugo gimtadienio duomenis .CSV failuose, kad jie būtų išlaikyti tarp sesijų.
    
* **1.2 Kaip paleisti programą?**

    Norint paleisti programą:
    
    1.  Reikia išsaugoti programos `.py` failą.
    2.  Atidaryti programą, esančią `.py` faile.
    3.  Paspausti `Run Code` mygtuką.
    
* **1.3 Kaip naudoti programą?**

    Kai programa prasideda, pateikiamas meniu:
    
    1.  **Prisijungti prie paskyros:** Įvesti savo paskyros vardą, kad prisijungti. Jei paskyra egzistuoja, vartotojas nukreipiamas į sesijos meniu.
    2.  **Registruoti naują paskyrą:** Įvesti naują paskyros vardą, kad sukurti naują paskyrą.
    3.  **Išeiti:** Uždaro programą.
    
    Prisijungus, sesijos meniu leidžia:
    
    1.  **Pridėti gimtadienį:** Pridėti naują gimtadienį, įvedant vardą, datą (YYYY-MM-DD) ir tipą (draugas/seima).
    2.  **Pašalinti gimtadienį:** Pašalinti gimtadienį, įvedant vardą.
    3.  **Peržiūrėti gimtadienius:** Rodo visus išsaugotus gimtadienius dabartiniam vartotojui.
    4.  **Atsijungti:** Grįžta į pagrindinį meniu.

**2. Analizė**

* **2.1 Kaip programa įgyvendina funkcinius reikalavimus**
    * **Pridėti/pašalinti gimtadienius:** `gimtadieniai` klasės metodai `prideti_gimtadieni` ir `pasalinti_gimtadieni` įgyvendina šį funkcionalumą.
    * **Spausdinti gimtadienio priminimus:** `gimtadieniai` klasės metodas `priminti_snd` patikrina, ar yra gimtadienių einamąją dieną, ir atspausdina priminimą.
    * **Išsaugoti gimtadienius į failą:** `gimtadieniai` klasė naudoja .CSV failus gimtadienio duomenims saugoti, o `issaugoti_gimtadienius` ir `irasyti_gimtadieniai` metodai tvarko išsaugojimą ir įkėlimą.
    * **Palaikyti kelis vartotojus:** `paskyra` klasė valdo vartotojų paskyras, leidžiant keliems vartotojams registruotis ir saugoti savo gimtadienių sąrašus.

    * **Kodo ištraukos:**
    
        * Pridėjimas gimtadienio:
    
            ```python
            def prideti_gimtadieni(self, vardas, data, tipas):
                if tipas == "draugas":
                    asmuo = draugas(vardas, data)
                elif tipas == "seima":
                    asmuo = seima(vardas, data)
                else:
                    print("Tipas turi būti 'draugas' arba 'seima'")
                    return
                self.zmones.append(asmuo)
                self.issaugoti_gimtadienius()
                print(f"{vardas} gimtadienis pridėtas.")
            ```
            
        * Išsaugojimas į CSV:
    
            ```python
            def issaugoti_gimtadienius(self):
                with open(self.failas, mode="w", newline="") as file:
                    writer = csv.writer(file)
                    for asmuo in self.zmones:
                        tipas = "draugas" if isinstance(asmuo, draugas) else "seima"
                        writer.writerow([asmuo.vardas, asmuo.gimtadienis.strftime("%Y-%m-%d"), tipas])
### Meniu pavyzdys:

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

**3. Rezultatai ir Apibendrinimas**

* **3.1 Rezultatai**

    * Programa sėkmingai leidžia vartotojams registruotis, prisijungti ir valdyti savo gimtadienių sąrašus, įskaitant pridėjimą, pašalinimą ir peržiūrą.
    * Gimtadienio duomenys yra išsaugomi .CSV formatu, užtikrinant duomenų išlikimą tarp programos paleidimų, o prisijungus vartotojui, priminimai apie šiandienos gimtadienius yra pateikiami.
    * Vienas iš iššūkių buvo tinkamai suvaldyti skirtingų klasių (pvz., naudotojas, gimtadieniai, paskyra) tarpusavio ryšius ir užtikrinti sklandų duomenų srautą tarp jų.
    * Taip pat teko kelis kartus tikslinti metodų pavadinimus ir jų kvietimo būdus, kad atitiktų skirtingas klases ir būtų išvengta atributų klaidų.

* **3.2 Išvados**

    Šio darbo metu sėkmingai parašyta Birthday Reminder programa, kuri leidžia vartotojams patogiai valdyti ir sekti svarbias datas. Rezultatas - programa, leidžianti registruotis, prisijungti, pridėti, šalinti ir peržiūrėti gimtadienius, o taip pat gauti priminimus apie šiandienos gimtadienius. Ateities perspektyvos apima pranešimų sistemos tobulinimą, GUI kūrimą bei papildomų funkcijų, tokių kaip gimtadienių redagavimas ir paieška, integravimą.

* **3.3 Kaip būtų galima išplėsti jūsų aplikaciją?**

    Galimi patobulinimai apima:
    
    * Pranešimų įgyvendinimą (pvz., naudojant biblioteką el. laiškams ar darbalaukio pranešimams siųsti).
    * GUI (grafinės vartotojo sąsajos) pridėjimą patogesniam naudojimui.
    * Paieškos funkcionalumo pridėjimą gimtadieniams rasti.
    * Klaidų tvarkymo ir įvesties validacijos tobulinimą.
    * Leidimą vartotojams redaguoti esamus gimtadienius.
