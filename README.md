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

Kodas:
import csv
import datetime
import os
from abc import ABC, abstractmethod
import unittest


class birthday_reminder:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance.paskyros = paskyra()
        return cls._instance

    def paleisti(self):
        while True:
            print("\n--- Birthday Reminder ---")
            print("1. Prisijungti prie paskyros")
            print("2. Registruoti naują paskyrą")
            print("3. Išeiti")
            pasirinkimas = input("Pasirinkite: ")

            if pasirinkimas == "1":
                vardas = input("Įveskite paskyros vardą: ")
                naudotojas = self.paskyros.gauti_paskyra(vardas)
                if naudotojas:
                    sesija(naudotojas).paleisti()
                else:
                    print("Paskyra nerasta.")
            elif pasirinkimas == "2":
                vardas = input("Naujas paskyros vardas: ")
                self.paskyros.prideti_paskyra(vardas)
            elif pasirinkimas == "3":
                print("Programa baigta.")
                break
            else:
                print("Neteisingas pasirinkimas.")


class zmogus(ABC):
    def __init__(self, vardas, gimtadienis):
        self.vardas = vardas
        self.gimtadienis = datetime.datetime.strptime(gimtadienis, "%Y-%m-%d").date()

    @abstractmethod
    def info(self):
        pass


class draugas(zmogus):
    def info(self):
        return f"Draugo {self.vardas} gimtadienis: {self.gimtadienis}"


class seima(zmogus):
    def info(self):
        return f"Šeimos nario {self.vardas} gimtadienis: {self.gimtadienis}"


class naudotojas:
    def __init__(self, vardas):
        self.vardas = vardas
        self.gimtadienis = gimtadieniai(vardas)


class paskyra:
    def __init__(self):
        self.naudotojai = []

    def prideti_paskyra(self, vardas):
        if not any(naudotojas.vardas == vardas for naudotojas in self.naudotojai):
            naujas_naudotojas = naudotojas(vardas)
            self.naudotojai.append(naujas_naudotojas)
            print(f"Paskyra {vardas} pridėta.")
        else:
            print("Tokia paskyra jau egzistuoja.")

    def gauti_paskyra(self, vardas):
        for naudotojas in self.naudotojai:
            if naudotojas.vardas == vardas:
                return naudotojas
        return None


class gimtadieniai:
    def __init__(self, vardas):
        self.vardas = vardas
        self.zmones = []
        self.failas = f"{vardas}_gimtadieniai.csv"
        self.irasyti_gimtadieniai()

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

    def pasalinti_gimtadieni(self, vardas):
        self.zmones = [z for z in self.zmones if z.vardas != vardas]
        self.issaugoti_gimtadienius()
        print(f"{vardas} pašalintas.")

    def rodyti_gimtadienius(self):
        if not self.zmones:
            print("Nėra gimtadienių.")
            return
        for asmuo in self.zmones:
            print(asmuo.info())

    def priminti_snd(self):
        siandien = datetime.date.today()
        for asmuo in self.zmones:
            if asmuo.gimtadienis.month == siandien.month and asmuo.gimtadienis.day == siandien.day:
                print(f"🔔 Šiandien yra {asmuo.vardas} gimtadienis!")

    def issaugoti_gimtadienius(self):
        with open(self.failas, mode="w", newline="") as file:
            writer = csv.writer(file)
            for asmuo in self.zmones:
                tipas = "draugas" if isinstance(asmuo, draugas) else "seima"
                writer.writerow([asmuo.vardas, asmuo.gimtadienis.strftime("%Y-%m-%d"), tipas])

    def irasyti_gimtadieniai(self):
        if not os.path.exists(self.failas):
            return
        with open(self.failas, mode="r") as file:
            reader = csv.reader(file)
            for eilute in reader:
                if len(eilute) == 3:
                    vardas, data, tipas = eilute
                    self.prideti_gimtadieni(vardas, data, tipas)


class sesija:
    def __init__(self, naudotojas):
        self.naudotojas = naudotojas

    def paleisti(self):
        print(f"\n🔐 Prisijungėte kaip {self.naudotojas.vardas}")
        self.naudotojas.gimtadienis.priminti_snd()
        while True:
            print("\n1. Pridėti gimtadienį")
            print("2. Pašalinti gimtadienį")
            print("3. Peržiūrėti gimtadienius")
            print("4. Atsijungti")
            pasirinkimas = input("Pasirinkite: ")

            if pasirinkimas == "1":
                vardas = input("Vardas: ")
                data = input("Gimtadienio data (YYYY-MM-DD): ")
                tipas = input("Tipas (draugas/seima): ")
                self.naudotojas.gimtadienis.prideti_gimtadieni(vardas, data, tipas)
            elif pasirinkimas == "2":
                vardas = input("Įveskite vardą: ")
                self.naudotojas.gimtadienis.pasalinti_gimtadieni(vardas)
            elif pasirinkimas == "3":
                self.naudotojas.gimtadienis.rodyti_gimtadienius()
            elif pasirinkimas == "4":
                print("Atsijungta.")
                break
            else:
                print("Neteisingas pasirinkimas.")


class testavimas(unittest.TestCase):
    def setUp(self):
        self.valdiklis = gimtadieniai("testas")
        self.valdiklis.failas = "testas_gimtadieniai.csv"
        self.valdiklis.zmones = []

    def tearDown(self):
        if os.path.exists("testas_gimtadieniai.csv"):
            os.remove("testas_gimtadieniai.csv")

    def test_prideti(self):
        self.valdiklis.prideti_gimtadieni("Jonas", "1990-05-01", "draugas")
        self.assertEqual(len(self.valdiklis.zmones), 1)

    def test_pasalinti(self):
        self.valdiklis.prideti_gimtadieni("Jonas", "1990-05-01", "draugas")
        self.valdiklis.pasalinti_gimtadieni("Jonas")
        self.assertEqual(len(self.valdiklis.zmones), 0)

    def test_priminimas(self):
        siandien = datetime.date.today().strftime("%Y-%m-%d")
        self.valdiklis.prideti_gimtadieni("Tomas", siandien, "šeima")
        self.valdiklis.priminti_snd()


if __name__ == "__main__":
    unittest.main(exit=False)
    programa = birthday_reminder()
    programa.paleisti()
