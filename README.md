# style-redaction-agent

Une règle de rédaction en français pour agent personnel permanent. Elle marche sur **Hermes Agent**
et sur **OpenClaw**, et elle suit le standard [agentskills.io](https://agentskills.io/specification).

Le but : que ce que tu écris ne se lise pas comme du texte de machine. Pas de tiret cadratin, pas de
point-virgule, pas de rythme ternaire, pas de « ce n'est pas X, c'est Y », pas de vocabulaire
passe-partout. Un contrôle mécanique va avec, il attrape ce qui reste.

## Installation

**Hermes Agent**, par le chemin officiel. Le skill passe le scan de sécurité, verdict `SAFE`, vérifié
le 2026-08-13 sur une installation réelle.

```bash
hermes skills install https://github.com/fullya99/style-redaction-agent
```

Les mises à jour se font ensuite par `hermes skills update`. Un `git clone` dans
`~/.hermes/skills/` marche aussi, avec un `git pull` pour les mises à jour.

**OpenClaw**

```bash
git clone https://github.com/fullya99/style-redaction-agent.git \
  ~/.openclaw/workspace/skills/style-redaction-agent
```

Le dossier de catégorie est du rangement, pas une condition, le scan est récursif. Si ton workspace
OpenClaw n'est pas à l'emplacement par défaut, regarde `agents.defaults.workspace` dans
`~/.openclaw/openclaw.json`.

Ce skill passe le scan parce qu'il ne touche ni à la mémoire ni à la config de l'agent. Il lit des
fichiers que tu lui donnes et il écrit du texte. Son voisin `cloture-agent`, qui range la mémoire de
l'agent, est bloqué à l'installation officielle pour cette raison exacte, et c'est voulu par la
plateforme.

Pas besoin de redémarrer. Le skill est visible tout de suite par l'outil de liste, mais l'index de
skills du prompt système est construit en début de session, donc il n'y apparaîtra qu'à la session
suivante.

**Claude Code** a sa propre version, `style-redaction`, installable par marketplace. Le contenu de la
règle est le même, seule la résolution du script change.

## Quand ça se déclenche

Dès qu'un texte destiné à être lu se produit ou se relit. Doc, README, rapport, note, mail, post.
Aussi sur les demandes directes : « relis mon texte », « ça fait trop IA », « rends ça plus naturel ».

À charger **avant** d'écrire. Repasser un texte après coup marche mal, les tournures générées
s'incrustent dans la structure et pas seulement dans les mots.

Ça ne s'applique pas au code, ni aux noms de variables.

## Le script, utilisable seul

```bash
bash scripts/verif-style.sh <fichier|repertoire>
bash scripts/verif-style.sh --strict <fichier>   # sortie 1 s'il reste une alerte
```

Il compte les cadratins, les points-virgules, les virgules d'Oxford, le vocabulaire de remplissage et
les participes présents décoratifs. Bash seul, aucune dépendance, aucun serveur MCP.

Il ne juge ni le rythme des phrases, ni le rythme ternaire, ni le ton. Ça, c'est la lecture, et le
`SKILL.md` donne les quatre questions à se poser.

## Ce qu'il ne fait pas

Il ne réécrit pas tout seul un texte que tu ne lui as pas donné.

Il n'impose pas un registre. Le tutoiement, le vouvoiement et le niveau de langue dépendent du
destinataire, la règle demande juste qu'ils restent constants dans un même document.

## Licence

MIT.
