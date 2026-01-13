# 🏛️ La Sveglia dell'Architetto v2.0  
### Blueprint Home Assistant – Assistente Domiciliare Intelligente

[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2024.8%2B-blue.svg)](https://www.home-assistant.io/)
[![Blueprint](https://img.shields.io/badge/Blueprint-Ready-green.svg)](https://github.com/flavioporro/sveglia-architetto)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![YouTube](https://img.shields.io/badge/YouTube-ipensieridellarchitetto-red.svg)](https://www.youtube.com/@ipensieridellarchitetto)

> *"La domotica non decide per te, ma anticipa i tuoi bisogni rispettando i tuoi ritmi umani."*

La **Sveglia dell'Architetto** è un blueprint per Home Assistant che orchestra il risveglio domestico in modo **progressivo, naturale e affidabile**, senza automazioni invasive o comportamenti innaturali.

Progettato per funzionare **per mesi senza manutenzione**.

---

## 📺 Video Tutorial

👉 **Guarda il test reale e la spiegazione completa**  
[**I Pensieri dell'Architetto**](https://www.youtube.com/@ipensieridellarchitetto)

📱 **Entra nella community**  
[Telegram: I Pensieri dell'Architetto](https://t.me/pensieridellarchitetto)

---

## ✨ Filosofia del Progetto

> *La casa non si sveglia a un orario.  
> Si sveglia quando tu ti muovi.*

Questa automazione:
- parte **solo all'orario settimanale configurato**
- prepara la zona notte con apertura morbida
- **attende un gesto umano reale** (max 30 minuti configurabili)
- se rileva movimento → accompagna la transizione verso la zona giorno
- se nessun movimento → si ferma automaticamente (risparmio energetico)
- garantisce **una sola esecuzione al giorno**

**Nessuna automazione robotica. Solo ritmo umano.**

---

## 🧠 Logica di Funzionamento

### 1️⃣ Avvio Temporale (Fase Preparazione)
All'orario impostato per il giorno della settimana:

- 🛏️ **Serrande camere** → apertura **15%** (configurabile 0-100%)
  - Luce naturale morbida, non invasiva
  - Abbastanza per svegliarsi naturalmente
- 🚿 **Serranda bagno** → apertura **100%**
  - Bagno pronto e illuminato quando ti alzi
- 📝 Salvataggio timestamp esecuzione
  - Protezione da doppie esecuzioni

**La casa prepara l'ambiente, poi aspetta te.**

---

### 2️⃣ Attesa Gesto Umano (Fase Ascolto)

L'automazione **attende il sensore di movimento** che rileva il passaggio dalla zona notte alla zona giorno.

**⏱️ Timeout configurabile:** 15-120 minuti (default: **30 minuti**)

#### ✅ **Scenario A: Movimento Rilevato**
Ti alzi entro 30 minuti → La routine prosegue con tutte le azioni

#### ❌ **Scenario B: Nessun Movimento (Timeout)**
Dopo 30 minuti senza movimento, l'automazione si **ferma** per risparmio energetico:
- ❌ Luci restano spente
- ❌ Tapparelle piano terra restano chiuse  
- ❌ Caffè non si prepara
- ❌ Porta resta bloccata
- ✅ Serrande camere/bagno già aperte danno luce naturale

**Se dormi di più, la casa non spreca energia.**

---

### 3️⃣ Evento Movimento (Fase Accompagnamento)

**Al rilevamento movimento (T0):**

- 💡 **Luci zona giorno** → **ACCESE IMMEDIATAMENTE**
  - Nessun buio quando scendi
- 📝 Log movimento registrato

**Dopo 10 secondi** (configurabile 0-60s):
- 🏠 **Tapparelle piano terra** → **100%**
  - Tempo per muoverti verso la zona giorno
  - Apertura completa per massima luminosità

**Dopo 10 minuti** (configurabile 5-30min):
- ☕ **Macchina del caffè** → **SEMPRE ACCESA**
  - Tempo perfetto: bagno fatto, pronto per la colazione
- 🚪 **Porta d'ingresso** → **SBLOCCATA**
  - ⚠️ Solo se entity `person` è `home`
  - Sicurezza: se sei fuori casa, la porta resta chiusa

---

## 🛡️ Sicurezza e Affidabilità

### Protezioni Implementate

- ✅ **Una sola esecuzione al giorno** (sistema idempotente con `input_datetime`)
- ✅ **Verifica giorno della settimana** (previene trigger sbagliati)
- ✅ **Timeout intelligente** (non spreca energia se dormi)
- ✅ **Gestione errori** (`continue_on_error` su ogni dispositivo fisico)
- ✅ **Log completi** (ogni fase tracciata nel Logbook)
- ✅ **Compatibile con riavvii** (stato persistente su riavvio HA)
- ✅ **Mode single** (ignora trigger multipli se già in esecuzione)

### Requisiti Tecnici

- **Home Assistant 2024.8+**
- Sintassi moderna: `action:` (non `service:`)
- Blueprint format standard

---

## 📦 Requisiti

### Entità Necessarie

| Tipo | Uso | Obbligatorio | Esempio |
|------|-----|--------------|---------|
| `input_datetime` | Tracking esecuzione | ✅ Sì | `input_datetime.ultima_sveglia` |
| `cover` | Serrande camere | ✅ Sì | `cover.camera_matrimoniale` |
| `cover` | Serranda bagno | ✅ Sì | `cover.bagno` |
| `cover` | Tapparelle piano terra | ✅ Sì | `cover.cucina, cover.soggiorno` |
| `binary_sensor` | Sensore movimento | ✅ Sì | `binary_sensor.movimento_scala` |
| `light` | Luci zona giorno | ✅ Sì | `light.cucina, light.scala` |
| `switch` | Macchina caffè | ✅ Sì | `switch.macchina_caffe` |
| `person` | Verifica presenza | ✅ Sì | `person.nome` |
| `lock` | Porta ingresso | ✅ Sì | `lock.porta_principale` |

---

## ⚙️ Installazione

### Metodo 1: Import Diretto (Consigliato)

1. Vai in **Impostazioni → Automazioni e Scene → Blueprint**
2. Clicca **➕ Importa Blueprint**
3. Incolla questo URL:
   ```
   https://github.com/flavioporro/sveglia-architetto/blob/main/sveglia_architetto.yaml
   ```
4. Clicca **Anteprima** → **Importa**
5. Crea automazione: **🏛️ Sveglia dell'Architetto v2.0**

### Metodo 2: Manuale

1. Scarica [`sveglia_architetto.yaml`](https://github.com/flavioporro/sveglia-architetto/blob/main/sveglia_architetto.yaml)
2. Copia in `config/blueprints/automation/sveglia_architetto/`
3. Riavvia Home Assistant
4. Vai in Automazioni → Crea da Blueprint

---

## 🧩 Configurazione Helper (OBBLIGATORIO)

### ⚠️ Passaggio Critico: Creazione Input Datetime

Il blueprint **richiede** un helper `input_datetime` dedicato per funzionare.

**Procedura passo-passo:**

1. **Impostazioni** → **Dispositivi e servizi** → **Aiutanti**
2. Clicca **➕ Crea aiutante**
3. Seleziona **Data e/ora**
4. Configurazione:
   - **Nome**: `Ultima esecuzione sveglia architetto`
   - **Icona**: `mdi:clock-check` (opzionale)
   - **Ha data**: ✅ **Sì**
   - **Ha ora**: ✅ **Sì**
5. **Salva**

**⚠️ Importante:**
- Deve essere un helper **nuovo e dedicato**
- Non riutilizzare helper esistenti
- Verrà aggiornato automaticamente ogni mattina

### Collegamento al Blueprint

Quando crei l'automazione dal blueprint, nel campo:

**🧠 Ultima esecuzione routine**

Seleziona dall'elenco: `input_datetime.ultima_esecuzione_sveglia_architetto`

---

## 🎛️ Parametri Configurabili

### Orari Settimanali
Imposta un orario **diverso** per ogni giorno:
- 🗓️ Lunedì
- 🗓️ Martedì  
- 🗓️ Mercoledì
- 🗓️ Giovedì
- 🗓️ Venerdì
- 🗓️ Sabato
- 🗓️ Domenica

### Dispositivi Coperture
- **🪟 Serrande camere** (multipla selezione)
- **📊 Apertura serrande camere** (0-100%, default: 15%)
- **🚿 Serranda bagno** (singola)
- **🏠 Tapparelle piano terra** (multipla selezione)

### Sensori e Luci
- **🚶 Sensore movimento** (binary_sensor)
- **⏱️ Timeout movimento** (15-120 min, default: 30 min)
- **💡 Luci da attivare** (multipla selezione)

### Temporizzazioni
- **⏲️ Ritardo tapparelle PT** (0-60 sec, default: 10 sec)
- **☕ Ritardo caffè** (5-30 min, default: 10 min)

### Dispositivi Smart
- **☕ Macchina del caffè** (switch)
- **👤 Persona** (person entity)
- **🚪 Porta ingresso** (lock)

---

## 📊 Esempio Configurazione Completa

```yaml
# Esempio setup appartamento standard

Helper:
  input_datetime.ultima_esecuzione_sveglia_architetto

Orari:
  Lunedì-Venerdì: 07:00
  Sabato: 08:00
  Domenica: 09:00

Serrande camere:
  - cover.camera_letto
  - cover.cameretta
  Apertura: 15%

Serranda bagno:
  - cover.bagno

Tapparelle piano terra:
  - cover.cucina
  - cover.soggiorno
  - cover.sala_pranzo

Sensore movimento:
  binary_sensor.movimento_scala

Timeout movimento:
  30 minuti

Luci:
  - light.cucina
  - light.corridoio
  - light.scala

Ritardo tapparelle PT:
  10 secondi

Ritardo caffè:
  10 minuti

Macchina caffè:
  switch.macchina_caffe

Persona:
  person.flavio

Porta:
  lock.porta_principale
```

---

## 🔍 Monitoraggio e Debug

### Visualizzare i Log

**Metodo 1: Logbook (Cronologia)**
1. Vai in **Cronologia**
2. Cerca "**Sveglia Architetto**"
3. Visualizza timeline completa:
   - 🌅 Avvio routine
   - 🪟 Apertura serrande
   - 👀 Attesa movimento
   - ✅ Movimento rilevato
   - 💡 Luci / 🏠 Tapparelle / ☕ Caffè / 🚪 Porta
   - ✨ Completamento

**Metodo 2: Log Sistema**
1. **Impostazioni** → **Sistema** → **Log**
2. Cerca "**Sveglia Architetto**"
3. Vedi log tecnici dettagliati

**Metodo 3: Tracce Automazione**
1. **Impostazioni** → **Automazioni**
2. Clicca sulla tua automazione
3. Tab **Tracce**
4. Visualizza JSON esecuzione completa

### Cosa Viene Registrato

Ogni fase ha un log dedicato:

```
🌅 Routine Lunedi avviata alle 07:00
🪟 Serrande camere aperte al 15%
🚿 Serranda bagno aperta completamente
👀 In attesa di movimento (timeout: 30 min)...
✅ Movimento rilevato alle 07:15, proseguo con la routine
💡 Luci accese
🏠 Invio comando apertura tapparelle piano terra...
✅ Comando inviato alle tapparelle piano terra (100%)
☕ Macchina del caffè attivata
🚪 Porta sbloccata (persona in casa)
✨ Routine completata con successo
```

---

## 🧪 Test e Risoluzione Problemi

### Test Rapido (Senza Aspettare l'Orario)

**Test Manuale con Bypass Condizioni:**

1. Apri l'automazione
2. Clicca **⋮ (tre puntini)** in alto a destra
3. Seleziona **Esegui**
4. ✅ Attiva **"Ignora condizioni"**
5. Clicca **Esegui**

Questo bypassa il controllo giorno/orario/helper e esegue subito.

**Test Timeout Veloce:**

1. Modifica `timeout_movimento` temporaneamente a **1 minuto**
2. Esegui manualmente
3. **Scenario A**: Muoviti entro 1 min → prosegue
4. **Scenario B**: Non muoverti → si ferma dopo 1 min

### Problemi Comuni e Soluzioni

| Problema | Causa | Soluzione |
|----------|-------|-----------|
| ❌ **Non parte all'orario** | Helper non configurato | Verifica helper esista e sia selezionato |
| ❌ **Parte più volte al giorno** | Helper condiviso con altre automazioni | Crea helper dedicato nuovo |
| ❌ **Si ferma subito** | Sensore già `on` | Aspetta che sensore torni `off` |
| ❌ **Tapparelle non si aprono** | Entità sbagliate | Testa manualmente i comandi |
| ❌ **Errore `ultima_routine undefined`** | Helper non collegato | Ricontrolla selezione helper nel blueprint |

### Debug Avanzato

**Controlla stato helper:**
```
Strumenti Sviluppatori → Stati
Cerca: input_datetime.ultima_esecuzione_sveglia_architetto
Valore deve essere: 2026-01-13 07:15:23
```

**Controlla trigger automazione:**
```
Strumenti Sviluppatori → Stati
Cerca: automation.tua_automazione
Attributo last_triggered deve aggiornarsi
```

**Verifica sensore movimento:**
```
Strumenti Sviluppatori → Stati
Cerca: binary_sensor.tuo_sensore
Stato: on/off (deve tornare off tra un trigger e l'altro)
```

---

## 🚀 Casi d'Uso e Scenari

### Scenario 1: Lavoratore Dipendente
```yaml
Lunedì-Venerdì: 06:30 (sveglia presto)
Sabato-Domenica: 09:00 (riposo)
Timeout: 30 min (routine fissa)
```

### Scenario 2: Smart Working / Libero Professionista
```yaml
Tutti i giorni: 07:30
Timeout: 60 min (orari più flessibili)
Ritardo caffè: 15 min (colazione lenta)
```

### Scenario 3: Famiglia con Bambini
```yaml
Giorni scuola: 06:30
Weekend: 08:00
Luci: + camerette bambini
Timeout: 20 min (movimento garantito)
```

### Scenario 4: Pensionato / Orari Liberi
```yaml
Tutti i giorni: 08:00
Timeout: 90 min (massima flessibilità)
Serrande camere: 20% (più luce)
```

### Scenario 5: Due Automazioni Stagionali

**Inverno:**
- Orari anticipati (buio)
- Serrande: 15% (luce tenue)
- Timeout: 30 min

**Estate:**
- Orari posticipati (luce naturale)
- Serrande: 10% (già molta luce)
- Timeout: 45 min

Attiva/disattiva manualmente o con automazione stagionale.

---

## 🔧 Personalizzazioni Avanzate

### Condizione Meteo

Aggiungi condizione temperatura prima dell'apertura:

```yaml
- condition: numeric_state
  entity_id: sensor.temperatura_esterna
  below: 35  # Non aprire se troppo caldo
  above: -5  # Non aprire se gelo
```

### Integrazione TTS

Annuncio vocale dopo movimento:

```yaml
- service: tts.google_translate_say
  target:
    entity_id: media_player.cucina
  data:
    message: "Buongiorno! Il caffè sarà pronto tra 10 minuti."
```

### Notifiche Push

Aggiungi notifica su smartphone:

```yaml
- service: notify.mobile_app_iphone
  data:
    title: "🌅 Sveglia Architetto"
    message: "Routine completata. Buona giornata!"
```

### Playlist Mattutina

Avvia musica dopo movimento:

```yaml
- service: media_player.play_media
  target:
    entity_id: media_player.spotify
  data:
    media_content_id: spotify:playlist:xxxxx
    media_content_type: playlist
```

---

## 🎥 Autore

**I Pensieri dell'Architetto**

Creato da chi crede che la domotica debba essere **invisibile, affidabile e umana**.

📺 **YouTube**: [@ipensieridellarchitetto](https://www.youtube.com/@ipensieridellarchitetto)  
📱 **Telegram**: [t.me/pensieridellarchitetto](https://t.me/pensieridellarchitetto)  
💻 **GitHub**: [flavioporro/sveglia-architetto](https://github.com/flavioporro/sveglia-architetto)

---

## 🙏 Supporta il Progetto

Se questo blueprint ha migliorato le tue mattine:

- ⭐ **Stella su GitHub** (apprezzatissima!)
- 📺 **Iscriviti al canale YouTube**
- 📱 **Unisciti alla community Telegram**
- 💬 **Condividi con amici che usano Home Assistant**
- 📸 **Tagga con #SvegliaArchitetto** il tuo setup

Ogni stella, iscrizione e condivisione aiuta il progetto a crescere! 🚀

---

## 📞 Community & Supporto

### Dove Trovare Aiuto

- 📱 **Telegram Community**: [t.me/pensieridellarchitetto](https://t.me/pensieridellarchitetto)  
  Community italiana attiva, risposte veloci
  
- 🐛 **Segnala Bug**: [GitHub Issues](https://github.com/flavioporro/sveglia-architetto/issues)  
  Problemi tecnici, errori, comportamenti anomali
  
- 💡 **Proponi Idee**: [GitHub Discussions](https://github.com/flavioporro/sveglia-architetto/discussions)  
  Nuove funzionalità, varianti, miglioramenti

- 📺 **Tutorial Video**: [YouTube](https://www.youtube.com/@ipensieridellarchitetto)  
  Guide complete, spiegazioni, test reali

### Prima di Chiedere Aiuto

✅ Leggi la sezione **Test e Risoluzione Problemi**  
✅ Controlla le **Tracce** dell'automazione  
✅ Verifica i **Log** del sistema  
✅ Consulta i **Problemi Comuni**  

Questo aiuta a risolvere velocemente! 🚀

---

## 🔄 Changelog

### v2.0 (Gennaio 2025) - Current
**Miglioramenti Maggiori:**
- ✅ Timeout intelligente configurabile (15-120 min)
- ✅ Verifica giorno settimana (previene esecuzioni sbagliate)
- ✅ Gestione errori su tutti i dispositivi
- ✅ Sezione `variables` per compatibilità template
- ✅ Log completi per ogni fase
- ✅ Apertura serrande camere configurabile (0-100%)
- ✅ Ritardi temporali configurabili
- ✅ Source URL per import GitHub

**Bug Fix:**
- 🐛 Fix template condition con `!input`
- 🐛 Fix delay con sintassi corretta
- 🐛 Fix comparazione date con `as_timestamp`
- 🐛 Rimosso controllo stato caffè (sempre on)

**Documentazione:**
- 📝 README completo con esempi
- 📝 Script video YouTube
- 📝 Guide troubleshooting
- 📝 FAQ e casi d'uso

### v1.0 (2024) - Initial Release
- 🎉 Prima versione pubblica
- ⏰ Orari settimanali personalizzabili
- 🪟 Gestione serrande progressive
- 🚶 Attesa movimento umano
- ☕ Sequenza temporale intelligente

---

## 📄 Licenza

**MIT License**

Copyright (c) 2025 I Pensieri dell'Architetto

Questo software è rilasciato sotto licenza MIT.

**Cosa puoi fare:**
- ✅ Uso personale e commerciale
- ✅ Modificare il codice
- ✅ Distribuire e condividere
- ✅ Uso privato

**Cosa devi fare:**
- 📄 Mantenere avviso copyright
- 📝 Includere testo licenza MIT

**Limitazioni:**
- ❌ Nessuna garanzia fornita
- ❌ Nessuna responsabilità dell'autore

Vedi [LICENSE](LICENSE) per testo completo.

---

## 🏛️ Filosofia Finale

> **La domotica vera:**
> - Non ti chiede attenzione
> - Non ti manda notifiche inutili  
> - Non ti obbliga a fare nulla
> 
> **Funziona. E basta.**

### Principi di Design

**1. Rispetto del Ritmo Umano**  
La casa si adatta a te, non viceversa. Nessun orario fisso, nessuna imposizione.

**2. Gesto come Trigger**  
Nessuna azione automatica senza conferma umana. Il movimento è il consenso.

**3. Progressività Naturale**  
Le azioni si susseguono in modo fluido. Prima luce tenue, poi piena, poi comfort.

**4. Affidabilità sopra Tutto**  
Un'automazione che funziona 365 giorni vale più di 10 che richiedono continui fix.

**5. Zero Manutenzione**  
Configurazione una volta. Dimenticare. Funziona per mesi.

**6. Fail-Safe by Design**  
Se qualcosa non va, la casa resta comunque funzionale. Nessun lock-in.

---

## 🌟 Progetti Futuri

Altri blueprint della serie "Architetto":

- 🌙 **Buonanotte dell'Architetto** (coming soon)  
  Routine serale con spegnimento progressivo

- 🏠 **Ritorno a Casa dell'Architetto** (in sviluppo)  
  Accoglienza intelligente al rientro

- 💡 **Illuminazione Adattiva Architetto** (pianificato)  
  Luci che seguono il ritmo circadiano

- 🌡️ **Clima Intelligente Architetto** (idea)  
  Termoregolazione predittiva

Seguimi su **YouTube** e **Telegram** per aggiornamenti! 📺📱

---

## 🤝 Contribuire

I contributi sono benvenuti!

**Come contribuire:**

1. 🍴 **Fork** del repository
2. 🌿 **Crea branch**: `git checkout -b feature/nuova-funzione`
3. 💾 **Commit**: `git commit -m "Aggiunta funzione X"`
4. 📤 **Push**: `git push origin feature/nuova-funzione`
5. 🎯 **Pull Request** su GitHub

**Linee guida:**
- ✅ Mantieni la filosofia del progetto
- ✅ Testa approfonditamente (almeno 3 giorni)
- ✅ Documenta le modifiche nel README
- ✅ Commit message chiari e descrittivi

**Cosa cerchiamo:**
- 🐛 Bug fix e ottimizzazioni
- 📝 Miglioramenti documentazione
- 🌍 Traduzioni (EN, ES, FR, DE)
- 💡 Nuove funzionalità coerenti

---

## 🎓 Risorse Utili

### Home Assistant
- [Documentazione Ufficiale](https://www.home-assistant.io/docs/)
- [Blueprint Documentation](https://www.home-assistant.io/docs/automation/using_blueprints/)
- [Community Forum](https://community.home-assistant.io/)

### Automazioni
- [Triggers](https://www.home-assistant.io/docs/automation/trigger/)
- [Conditions](https://www.home-assistant.io/docs/scripts/conditions/)
- [Actions](https://www.home-assistant.io/docs/scripts/)
- [Templates](https://www.home-assistant.io/docs/configuration/templating/)

### Community Italiana
- [Home Assistant Italia - Facebook](https://www.facebook.com/groups/homeassistantitalia)
- [Telegram Home Assistant Italia](https://t.me/HomeAssistant_Italia)

---

## ⚡ Quick Start (TL;DR)

```bash
# Setup rapido in 5 minuti

1. Crea helper input_datetime "Ultima esecuzione sveglia"
2. Importa blueprint da GitHub URL
3. Crea automazione da blueprint
4. Configura orari + dispositivi
5. Imposta timeout 30 min
6. Salva
7. Test manuale con "Ignora condizioni"
8. ✅ Funziona per mesi
```

---

**Made with 🏛️ by I Pensieri dell'Architetto**  
*Per chi crede che la domotica debba essere invisibile*

---

## 🔗 Link Rapidi

- 🌐 **Repository**: [github.com/flavioporro/sveglia-architetto](https://github.com/flavioporro/sveglia-architetto)
- 📺 **YouTube**: [@ipensieridellarchitetto](https://www.youtube.com/@ipensieridellarchitetto)
- 📱 **Telegram**: [t.me/pensieridellarchitetto](https://t.me/pensieridellarchitetto)
- 🐛 **Bug Report**: [github.com/flavioporro/sveglia-architetto/issues](https://github.com/flavioporro/sveglia-architetto/issues)
- 💡 **Discussioni**: [github.com/flavioporro/sveglia-architetto/discussions](https://github.com/flavioporro/sveglia-architetto/discussions)

---

**Ultimo aggiornamento**: Gennaio 2025  
**Versione**: 2.0  
**Compatibilità**: Home Assistant 2024.8+

**Testato e funzionante da mesi. Affidabile. Umano. Semplice.**

---

⭐ Se ti è piaciuto, lascia una stella su GitHub!  
📱 Unisciti alla community su Telegram!  
📺 Iscriviti al canale YouTube per altri progetti!

**Grazie per aver scelto la Sveglia dell'Architetto.** 🏛️
