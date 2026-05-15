<p align="center">
  <img width="376" height="376" alt="Poullet_Gacha_pull_30%" src="https://github.com/user-attachments/assets/6a972c0c-c5d5-4333-94ed-c36b0134139a" />
</p>

## Poulet's-Gacha-Pull V(1.5)
### *script of mine does do Actual Gacha Mechanics.*

---
What does this script do?

| Feature | Description |
|:--------|:------------|
| **assigns a rarity** | assigns a rarity when you run the command  : /pull |
| **rarity affects the character creation** | make it so a B character is less cool than an S tier |
| **custom format** | It writes back information in a custom format modifiable in story card. |

---

What does is this script cappable of?

| Feature | Description |
|:--------|:------------|
| **Simple gacha mechanic** | (ready to copy and paste in a story card) |
| **a rarity system** | a rarity system with custom instruction for each one |
| **personalisable rarity odds** | trough script only for now |
| **custom description** | Player friendly way to modify the gacha's output trough a SC |
---

What does this script not do?
-Work by itself, the AI will need a custom context in your AI instruction, plot essentials and author's notes.
---

what do i still want to do with it?
| Feature | Description |
|:--------|:------------|
| **rarity odd** | (a way for players to change rarity odds trough a story card) |
| **more consistancy** | the ai still sometime give back uncomplete forms |
| **make the Script cleaner** | make the script actually stop clean, not repeat itself and work only when the command is pulled, and when not, actual story to continue. |

----

## *Scenario Script Install Guide:*

1. Use the [AI Dungeon website](https://aidungeon.com/) on PC (or view as desktop if mobile-only)
2. [Create a new scenario](https://help.aidungeon.com/faq/what-are-scenarios) or edit an existing scenario
3. Open the `DETAILS` tab at the top while editing your scenario
4. Scroll down to `Scripting` and toggle ON → `Scripts Enabled`
5. Select `EDIT SCRIPTS`
6. Select the `Input` tab on the left
7. Delete all code within said tab
8. Copy and paste the following code into your empty `Input` tab:

```javascript
const modifier = (text) => {
  if (!state) state = {};

  const t = text.toLowerCase();

  if (t.includes("/pull")) {
    const args = text.replace(/\/pull/ig, "").trim();

    state.pull = true;
    state.banner = args || "Default Banner";
    
    // Rarity
    const r = Math.random();
    if (r < 0.40) state.rarity = "B";
    else if (r < 0.70) state.rarity = "A";
    else if (r < 0.85) state.rarity = "S";
    else if (r < 0.95) state.rarity = "SS";
    else state.rarity = "X";

    // Feedback visible
    let debugMsg = "🎰 PULL\n";
    debugMsg += "Rarity: " + state.rarity + "\n";

    // Chargement config
    const loaded = loadGachaConfig();
    debugMsg += loaded;

    return { text: debugMsg + "\n" };
  }

  return { text };
};

function loadGachaConfig() {
  state.customFormat = null;
  state.customInstructions = null;

  const configCode = findStoryCard("pullconfig");

  if (!configCode || configCode.trim() === "") {
    return "⚠️ PullConfig card NOT FOUND";
  }

  try {
    eval(configCode);
    return "✅ custom format loaded";
  } catch (e) {
    return "❌ ⛓️‍💥 default format loaded";
  }
};

modifier(text);
