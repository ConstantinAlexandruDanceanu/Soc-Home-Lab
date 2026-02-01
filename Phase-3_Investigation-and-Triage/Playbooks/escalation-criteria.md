
# Escalation Criteria – When to Escalate an Incident

Acest document definește condițiile clare în care un incident
trebuie escaladat din triage către incident response
sau nivel superior de analiză.

Escalarea se bazează pe comportament observat,
nu pe presupuneri sau context extern.

---

## 1. Autentificare (High Priority)

Escalare imediată dacă există:
- succesiune **4625 → 4624**
- autentificare reușită după încercări repetate
- login din context neobișnuit (timp, user, metodă)

Acest tip de eveniment indică posibilă compromitere.

---

## 2. Activitate post-authentication

Escalare dacă, după un login reușit, observ:
- execuții de enumerare (whoami, net, sc, powershell)
- tentative de escaladare a privilegiilor
- execuții multiple într-un interval scurt

Chiar dacă escaladarea nu reușește,
intenția este suficientă pentru escalare.

---

## 3. Persistență

Escalare automată dacă sunt detectate:
- registry run keys
- scheduled tasks
- servicii noi
- modificări care supraviețuiesc reboot-ului

Persistența indică impact și necesită răspuns imediat.

---

## 4. Network Activity

Escalare dacă apare:
- trafic outbound imediat după execuție
- conexiuni către IP-uri sau domenii necunoscute
- beaconing sau pattern repetitiv

Corelarea proces → rețea este un indicator puternic.

---

## 5. Volum și pattern

Escalare dacă:
- evenimentele sunt repetitive
- afectează mai mulți useri sau sisteme
- indică automatizare

Chiar și fără succes imediat,
pattern-ul justifică escalarea.

---

## 6. Ce NU escaladez imediat

Nu escaladez dacă:
- evenimentul este izolat
- nu există follow-up
- nu există impact observabil
- activitatea poate fi explicată legitim

În aceste cazuri, documentez și monitorizez.

---

## 7. Principiul final

Escalarea este o decizie bazată pe:
- **impact**
- **intenție**
- **corelare**

Nu pe:
- numărul de alerte
- severitatea inițială
- presupuneri.
