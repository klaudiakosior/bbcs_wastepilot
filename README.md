# WastePilot – Proof of Concept

**Modul:** Business Case Studies (BBCS / WPR2) – FS26  
**Gruppe 19:** Caruso Alexia, Fernando Brightney, Grossen Celine, Kosior Klaudia, Sornalingam Sameera, Töndury Nora

---

## Was ist WastePilot?

WastePilot ist eine webbasierte SaaS-Plattform, die Entsorgungsunternehmen bei der datenbasierten Routenplanung unterstützt. Durch die Verknüpfung von Wetter-, Luftqualitäts- und Eventdaten mit historischen Abfalldaten berechnet das System, wo und wann mit erhöhtem Abfallaufkommen zu rechnen ist.

Dieser PoC demonstriert die Kernfunktion: **Abfallprognose für die 12 Zürcher Stadtkreise** auf Basis öffentlich zugänglicher Daten und einem Random-Forest-Modell.

---

## Inhalt des Notebooks

| Abschnitt | Beschreibung |
|-----------|-------------|
| 1. Setup | Imports, Stammdaten der 12 Stadtkreise |
| 2. Wetterdaten | Abruf via Open-Meteo API (Temperatur, Niederschlag, Wind) |
| 3. Luftqualität | Stundenmessungen der Stadt Zürich (NO₂, PM10, O₃) |
| 4. Events | Veranstaltungsdaten mit swisstopo-Stadtkreiszuordnung |
| 5. Synthetische Abfalldaten | Kalibrierte Tagesmengen pro Stadtkreis (Ersatz für fehlende Realdaten) |
| 6. Feature Engineering | Kombination aller Datenquellen, Gewichtungslogik |
| 7. Modell | Random Forest Regressor, Training & Evaluation |
| 8. Prognose | 7-Tage-Vorschau je Stadtkreis, Export als JSON |
| 9. Visualisierungen | Interaktive Risikokarte (Folium) + Balkendiagramm |

---

## Datenquellen

| Quelle | Was | Zugang |
|--------|-----|--------|
| [Open-Meteo](https://open-meteo.com/) | Wetter: Temperatur, Niederschlag, Wind (letzte 90 Tage) | kostenlos, kein API-Key |
| [Stadt Zürich – Luftqualität](https://data.stadt-zuerich.ch/dataset/ugz_luftschadstoffmessung_stundenwerte) | NO₂, PM10, O₃ an 3 Messstationen | kostenlos, kein API-Key |
| [Stadt Zürich – Events](https://data.stadt-zuerich.ch/dataset/zt_kultur) | Veranstaltungs-Locations mit GPS-Koordinaten | kostenlos, kein API-Key |
| [swisstopo](https://www.geo.admin.ch/de/programmierschnittstelle-api) | Reverse Geocoding (GPS → Stadtkreis) | kostenlos, kein API-Key |
| [Stadt Zürich – Bevölkerung](https://data.stadt-zuerich.ch/dataset/bev_bestand_jahr_kreis) | Einwohnerzahlen pro Stadtkreis | kostenlos, kein API-Key |

> **Hinweis Abfalldaten:** Tägliche Abfallmengen pro Stadtkreis sind nicht als Open Data verfügbar. Im PoC werden synthetische, fachlich kalibrierte Daten verwendet. In der Produktivversion werden diese durch historische Daten aus dem Dispositionssystem des Entsorgungsunternehmens ersetzt.

---

## Voraussetzungen

Python 3.9+ empfohlen.

```bash
pip install pandas numpy matplotlib seaborn scikit-learn requests folium
```

---

## Ausführen

```bash
jupyter notebook WastePilot_PoC.ipynb
```

Alle Zellen nacheinander ausführen (`Run All`). Die API-Aufrufe benötigen eine Internetverbindung.

**Ausgabedateien:**
- `waste_predictions.json` – Prognosedaten je Stadtkreis
- `district_stats.json` – Statistische Kennwerte

---

## Abgrenzung zum Produktivsystem

Dieser PoC deckt die Bausteine **Datenintegration** und **ML-Prognose-Engine** ab. Nicht Teil des PoC: Webapplikation, Anbindung an Dispositionssystem, Google Maps Platform.
