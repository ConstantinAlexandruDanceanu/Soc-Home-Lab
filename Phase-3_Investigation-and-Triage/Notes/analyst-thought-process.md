
# Phase 2 – Analyst Notes & Thought Process

Scopul acestei faze este să documentez modul în care gândesc atunci când
analizez alerte într-un SIEM, nu să demonstrez tool usage sau atacuri.

Notele de mai jos reflectă procesul meu mental în timpul analizei,
așa cum s-ar întâmpla într-un SOC real.

---

## 1. Prima reacție la alertă

Când apare o alertă în Wazuh, nu pornesc de la ideea că este automat un atac.
Prima întrebare este întotdeauna:
> „Este un eveniment izolat sau face parte dintr-un pattern?”

Mă uit întâi la:
- rule.id
- tipul evenimentului (process, auth, file, network)
- frecvență (single vs repetitive)
- context temporal

---

## 2. Zgomot vs semnal

Multe alerte sunt zgomot operațional.
Nu le tratez individual dacă au aceleași caracteristici.

Dacă văd:
- același user
- aceeași sursă
- aceeași regulă
- într-un interval scurt

le tratez ca **un singur incident**, nu ca alerte separate.

---

## 3. Ordinea în care analizez datele

Ordinea contează. Nu sar direct la concluzii.

1. **Autentificare**
   - Există 4624 după 4625?
   - Există login neobișnuit?

2. **Execuție de procese**
   - Ce proces rulează?
   - Cine l-a pornit?
   - Cu ce parametri?

3. **Impact**
   - A urmat ceva după?
   - Persistență?
   - Network?

Dacă nu există impact, nu forțez un verdict malițios.

---

## 4. Ce caut explicit după un login reușit

Un login reușit schimbă complet severitatea.

După 4624 caut imediat:
- Sysmon Event ID 1 (Process Create)
- PowerShell / cmd / net / sc
- Tentative de escaladare
- Persistență

Dacă apar comenzi de enumerare, tratez activitatea ca post-compromise,
chiar dacă nu există exploit sau payload.

---

## 5. Evenimente „gri” (Benign vs Malicious)

Nu toate evenimentele sunt clare.

Un PowerShell cu:
- comandă lizibilă
- fără obfuscare
- fără network
- fără persistence

poate fi:
- admin legit
- troubleshooting
- recon post-compromise

În astfel de cazuri, decizia nu este „benign” sau „malicious” imediat,
ci **suspicious / ambiguous**, cu nevoie de corelare.

---

## 6. Importanța dovezilor negative

Lipsa dovezilor este și ea o informație.

Dacă verific și NU găsesc:
- network activity
- persistence
- execuții ulterioare

notez explicit acest lucru.
Nu presupun că „nu e nimic”, ci că **nu există indicatori suficienți**.

---

## 7. Când escaladez

Escalarea nu se face pentru fiecare alertă.

Escalez când există:
- fail → success (4625 → 4624)
- activitate post-login
- tentative de escaladare
- impact confirmat

Dacă aceste elemente lipsesc, documentez și monitorizez.

