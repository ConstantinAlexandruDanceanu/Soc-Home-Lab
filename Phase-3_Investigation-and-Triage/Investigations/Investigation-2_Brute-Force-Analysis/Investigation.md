Am analizat alerte Wazuh generate de autentificări eșuate repetate,
urmate de un login reușit și activitate post-autentificare
care indică tentative de escaladare a privilegiilor.

---

Prima fază a incidentului constă într-un număr mare de evenimente
Windows Security **4625** (failed logon), toate pentru același user,
într-un interval scurt de timp.

Volumul și frecvența evenimentelor indică un pattern automatizat,
nu o greșeală punctuală de autentificare.
Wazuh a corelat aceste evenimente sub alerte de tip
authentication_failed.

---

Ulterior, apare un eveniment **Windows Security 4624**
(successful logon) pentru același user, în același interval de timp.

Succesiunea **4625 → 4624** confirmă o autentificare reușită
după încercări repetate, ceea ce ridică incidentul
de la tentativă la compromitere reușită.

---

Imediat după autentificarea reușită, am observat execuții
de comenzi care necesită privilegii administrative
(ex. net.exe, sc.exe, runas).

Aceste execuții au eșuat cu mesaje de tip access denied
sau autentificare eșuată, indicând tentative de escaladare
a privilegiilor după compromiterea inițială.

Activitatea este consistentă cu pașii de post-compromise
întâlniți frecvent după obținerea accesului inițial.

---

Nu am observat:
- obținerea de privilegii administrative
- mecanisme de persistență (registry run keys, scheduled tasks, servicii)
- activitate ulterioară care să indice control extins asupra sistemului
în această etapă.

---

Clasificare incident:
- brute-force authentication
- urmat de autentificare reușită
- cu activitate post-compromise și tentative de escaladare a privilegiilor

Nivel de severitate: **ridicat**

Incidentul este eligibil pentru escalare către incident response
pentru investigație suplimentară și măsuri de containment.

