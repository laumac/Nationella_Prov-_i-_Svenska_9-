# Träningsverktyg inför Nationella Prov i Svenska (Åk 9)

Detta är ett interaktivt verktyg utvecklat för att hjälpa elever att förbereda sig inför de nationella proven i svenska för årskurs 9. Systemet simulerar provets tre delar (Delprov A, B och C) och använder AI-integration för automatisk rättning och poängbedömning i realtid.

## 🔗 Live Demo
Applikationen är distribuerad och tillgänglig här:
👉 **https://svenska-np-prov.vercel.app/**

---

## 🛠️ Projektöversikt & Arkitektur

* **Frontend:** Semantisk HTML5, CSS3 (med responsiv design och utskriftsoptimering via `@media print`).
* **Backend & Logik:** Vanilla JavaScript (ES6+) för sessionshantering, flikar och DOM-manipulering.
* **Integrations:** REST API-anrop till AI-tjänst (`/api/generate`) för utvärdering av elevsvar.
* **Källkod:** Projektets källkod är privat av upphovsrättsliga skäl.

---

## 🧪 QA & Testdokumentation (Quality Assurance)

Som QA/Test Engineer har stort fokus lagts på applikationens stabilitet, gränssnitt och felhantering. Nedan följer en sammanfattning av de testfall som har exekverats manuellt:

### 1. Sessionshantering & Logik (Boundary Value Analysis)
* **Testmål:** Verifiera att timern låser officiellt facit korrekt.
* **Kriterier:** Facit-knappen ska vara inaktiv i exakt 3600 sekunder (1 timme). Timern visar återstående tid i realtid. Efter 60 minuter ändras status till "Upplåst!" och knappen blir klickbar.
* **Resultat:** **PASS**

### 2. Integrations- och Felhanteringstest (API Error Handling)
* **Testmål:** Verifiera systemets beteende vid nätverksfel eller tomma svar från AI-servern.
* **Exekvering:** Simulering av uteblivet svar från `/api/generate`.
* **Beteende:** Systemet fångar felet i en `catch`-block, återaktiverar knappen med texten "Försök igen" och förhindrar att applikationen kraschar eller visar en tom skärm.
* **Resultat:** **PASS**

### 3. Kompatibilitetstest för Utskrift (Compatibility & Print Testing)
* **Testmål:** Säkra att PDF-genereringen via `window.print()` är användarvänlig.
* **Kriterier:** Vid utskrift ska dynamiska element såsom `sessionTimer`, `no-print`-knappar, laddningsindikatorer (`#loader`) och footern döljas helt. Textkort och frågor ska formateras rent utan visuella buggar.
* **Resultat:** **PASS**

### 4. UI/UX & Validering av Indata
* **Testmål:** Kontrollera flervalsfrågornas logik (Delprov B).
* **Kriterier:** Användaren har maximalt två försök på radio-buttons. Vid rätt svar läggs klassen `.correct` till (+1p). Vid två felaktiga försök låses frågan automatiskt och det korrekta alternativet markeras för att ge feedback till eleven.
* **Resultat:** **PASS**

* ## ⚠️ Known Limitations & Edge Cases

* **Serverless Function Timeout (Vercel Hobby Tier Constraints):** Because the application requests a high volume of complex, structured content in a single prompt (simultaneously generating the exam theme, Section A text, three distinct reading comprehension texts with questions for Section B, Section C instructions, and the corresponding Answer Key), the execution time can occasionally exceed the 10-second limit imposed by Vercel's free tier during peak AI traffic. 
  
  **Handling:** The application gracefully handles this via a robust `try/catch` block. If the server returns a timeout or failure, the system prevents a frontend crash, stops the loading animation, alerts the user, and re-enables the generation button for a retry attempt.
