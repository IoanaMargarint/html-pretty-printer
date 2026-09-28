# HTML Pretty Printer (Bash CLI)

Acest proiect este un utilitar CLI dezvoltat în **Bash** pentru sistemele POSIX. Preia un fișier HTML "minificat" sau formatat haotic și generează o versiune reformatată cu o indentare vizuală corectă (4 spații per nivel), reconstruind clar structura arborelui DOM.

## Arhitectură și Implementare
S-a optat pentru o arhitectură de **Stream Processing**. Scriptul procesează fișierul linie cu linie, evitând încărcarea întregului document în memorie.
* **Procesare:** Folosește `grep -oP` (PCRE) pentru a tokeniza textul și a separa tag-urile, iar `sed` pentru curățarea spațiilor redundante.
* **Logică:** Menține un contor de indentare bazat pe tipul tag-ului (deschidere vs. închidere). 
* **Excepții:** Include o listă de verificare pentru tag-urile "self-closing" (ex: `img`, `br`, `input`) pentru a preveni creșterea la infinit a indentării.

## Performanță
Testat în WSL2 (Ubuntu), scriptul menține o amprentă de memorie extrem de redusă (~18-22 MB) și formatează un fișier complex de 500 KB în aproximativ 3.24 secunde.

## Cum se utilizează

**Pasul 1:** Acordă drepturi de execuție scriptului tastând în terminal:
`chmod +x pretty.sh`

**Pasul 2:** Rulează scriptul furnizând fișierul de intrare și cel de ieșire:
`./pretty.sh input.html output.html`

*(Dacă nu pui fișierul de ieșire, rezultatul va fi afișat direct în terminal).*
