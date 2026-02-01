# Triage Process – SOC Analyst Notes

Acest document descrie modul în care fac triage pe alerte
în Wazuh, cu scopul de a decide rapid dacă un eveniment
este zgomot, suspicious sau necesită escalare.

Nu tratez fiecare alertă ca un incident separat.
Primul obiectiv este identificarea pattern-urilor.

---

## 1. Primul contact cu alerta

Când apare o alertă, mă uit întâi la:
- rule.id și descriere
- tipul evenimentului (auth, process, file, network)
- frecvență (single vs repetitiv)
- agent / user implicat

Întrebarea inițială:
> Este un eveniment izolat sau parte dintr-o secvență?

---

## 2. Gruparea alertelor

Dacă observ:
- aceeași regulă
- același user
- aceeași sursă
- interval scurt de timp

tratez alertele ca **un singur incident**, nu ca evenimente separate.

---

## 3. Ordinea de analiză (importantă)

Urmez întotdeauna aceeași ordine:

1. **Autentificare**
   - Există 4625 (fail)?
   - Există 4624 (success)?
   - Succesiune fail → success?

2. **Execuție de procese**
   - Ce proces rulează?
   - Cu ce parametri?
   - Cine l-a pornit?

3. **Activitate post-login**
   - Enumerare?
   - Tentative de escaladare?
   - Persistență?

4. **Rețea**
   - Există conexiuni outbound?
   - IP-uri neobișnuite?
   - Comunicare imediat după execuție?



## 4. Clasificare inițială

După triage, clasific alerta ca:
- **Benign** – activitate explicabilă, fără pattern
- **Suspicious** – activitate ambiguă, necesită corelare
- **Malicious** – pattern clar sau impact confirmat

Dacă nu sunt suficiente date, nu forțez verdictul.


## 5. Importanța dovezilor negative

Verific explicit dacă LIPSEȘTE:
- autentificare reușită
- persistență
- activitate de rețea
- procese ulterioare

Lipsa acestor elemente este documentată
și influențează decizia finală.



## 6. Rezultatul triage-ului

La finalul triage-ului decid una din următoarele:
- închid alerta ca benignă
- o documentez ca suspicious și monitorizez
- o escaladez pentru investigație avansată

Scopul triage-ului este viteză + claritate,
nu investigație completă.

