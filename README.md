# AI Lab – GitHub Copilot CLI Workshop

Labb-repo for intern Copilot CLI-utbildning. Klona detta repo och följ övningarna nedan.

## Förberedelser

1. Installera Copilot CLI:
   ```bash
   npm install -g @github/copilot
   ```

2. Klona detta repo:
   ```bash
   git clone https://github.com/MSP-SSG/AILab.git
   cd AILab
   ```

3. Starta en session:
   ```bash
   copilot
   ```

## Övningar

### Övning 1: Kom igång

- Starta en interaktiv session med `copilot`
- Byt modell med `/model`
- Kolla token-budget med `/context`
- Kör `/init` för att generera en `copilot-instructions.md`

### Övning 2: Utforska en kodbas

- Använd `/agent explore` för att förstå repots struktur
- Ställ 3 frågor om koden
- Referera specifika filer med `@`
- Kör `/agent code-review` på staged changes

### Övning 3: Plan mode

- Välj en feature eller förbättring
- Aktivera Plan mode med `Shift+Tab`
- Låt Copilot skapa en plan
- Granska och godkänn planen
- Kolla resultatet med `/diff`
- Testa `Esc Esc` för att rulla tillbaka ändringar

### Övning 4: Custom instructions

- Öppna `copilot-instructions.md` (eller skapa med `/init`)
- Definiera kodstandarder och arkitekturprinciper
- Testa att Copilot följer dem i nästa prompt
- Skapa en enkel skill som markdown-fil i `.github/skills/`

### Övning 5: Verkligt scenario – CIS Benchmark-uppgradering

En avancerad övning som kombinerar alla tekniker.

**Scenario:** Vi har ett assessment-verktyg byggt för CIS Microsoft 365 Foundations Benchmark v1.4. Nu har v1.6 släppts med nya kontrollpunkter. Din uppgift:

1. Referera CIS-kravdokumentet (PDF) med `@`
2. Använd `/agent explore` för att förstå kodbasen
3. Aktivera Plan mode (`Shift+Tab`)
4. Låt Copilot skapa en gap-analys: vilka nya kontrollpunkter finns i 1.6 som saknas i 1.4?
5. Identifiera om vi behöver nya Azure-behörigheter (Graph API scopes)
6. Planen ska vara exekverbar i framtida kodningssessioner

### Övning 6: Bygg en Copilot Skill

En färdig Advania-brandad presentationsskill finns redan i repot.

1. Kontrollera att `.github/skills/presentation.md` finns
2. Starta en Copilot-session
3. Prompta: *"Skapa en 15 min presentation om [valfritt ämne]"*
4. Iterera: ändra slides, lägg till innehåll, byt format
5. Öppna den genererade HTML-filen i webbläsaren
6. Studera `presentation.md` – förstå hur en skill är uppbyggd

## Repo-struktur

```
AILab/
  .github/
    skills/
      presentation.md     # Färdig skill för övning 6
  README.md               # Denna fil
```

## Tips

- `Shift+Tab` växlar mellan Default, Plan och Autopilot
- `Ctrl+T` visar/döljer AI:ns resonemang
- `Esc` avbryter, `Esc Esc` rullar tillbaka ändringar
- `@fil.py` refererar en fil direkt i prompten
- `!kommando` kör shell-kommandon utan AI
- `/compact` komprimerar historiken om kontexten blir full
