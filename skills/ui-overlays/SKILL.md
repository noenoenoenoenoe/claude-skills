---
name: ui-overlays
description: Vocabulaire précis des calques d'interface (modal, popover, tooltip, toast, sheet, drawer, dropdown menu, sidebar…) pour décrire, construire, nommer ou relire une UI sans ambiguïté. Utilise cette skill dès qu'une demande parle d'un élément qui s'ouvre, apparaît ou se superpose à l'écran (« une fenêtre qui s'ouvre », « un popup », « un panneau qui glisse », « une sidebar qui s'ouvre », « un menu déroulant », « une bulle d'info », « une notif en bas »), quand on maquette ou nomme des frames Figma contenant ce genre d'élément, ou quand on code un composant d'overlay. À utiliser aussi pour traduire une description floue en terme exact avant de construire.
---

# UI overlays

Un mot flou (« popup », « fenêtre », « sidebar qui s'ouvre ») produit le mauvais composant. Avant de construire ou de nommer un calque, choisir le terme exact ci-dessous, puis l'utiliser partout : dans la conversation, les noms de frames Figma, les noms de composants et le code.

## 1. Choisir le terme

Deux questions suffisent dans la plupart des cas :

1. **Le calque bloque-t-il le reste de l'écran ?** (voile sombre, clic dehors = fermeture, focus piégé)
2. **Est-il attaché à un élément déclencheur, ou à un bord de l'écran ?**

| Terme | Bloque l'écran | Position | Déclenché par | Contenu typique | Fermeture |
|---|---|---|---|---|---|
| **Modal** (dialog) | Oui, voile | Centré | Action volontaire | Confirmation, petit formulaire | Bouton, Échap, clic sur le voile |
| **Alert dialog** | Oui, voile | Centré | Action risquée | « Supprimer ? » + 2 boutons | Uniquement par un bouton |
| **Popover** | Non | Ancré à son déclencheur | Clic | Petit contenu interactif (réglages, filtres) | Clic dehors, Échap |
| **Dropdown menu** | Non | Ancré sous son déclencheur | Clic | Liste d'actions ou de liens | Choix d'un item, clic dehors |
| **Context menu** | Non | Au curseur | Clic droit | Actions sur l'élément visé | Choix, clic dehors |
| **Tooltip** | Non | Ancré, très petit | Survol ou focus | Libellé de quelques mots, jamais interactif | Fin du survol |
| **Hover card** | Non | Ancré | Survol prolongé | Aperçu (profil, lien) | Fin du survol |
| **Toast** | Non | Coin ou bas de l'écran | Résultat d'une action | Message court + action optionnelle (« Annuler ») | Automatique après quelques secondes |
| **Sheet** | Oui, voile | Collé à un bord (souvent la droite), toute la hauteur | Clic | Détail, édition, liste longue | Croix, Échap, clic sur le voile |
| **Drawer** | Oui, voile | Monte depuis le bas, poignée en haut | Clic, surtout sur mobile | Actions ou formulaire mobile | Glisser vers le bas, voile |
| **Sidebar** | Non | Colonne fixe, fait partie de la mise en page | Toujours visible (repliable) | Navigation principale | Ne se ferme pas, se replie |
| **Command palette** | Oui, voile léger | Centré en haut | Raccourci (⌘K) | Recherche + actions | Échap, choix |

## 2. Pièges fréquents

- **Sidebar ≠ sheet.** Une sidebar fait partie de la page et pousse le contenu. Un panneau qui glisse par-dessus avec un voile est une **sheet**. Appeler une sheet « sidebar » mène à un composant mal construit (pas de voile, pas de focus piégé).
- **Popover ≠ modal.** S'il est ancré à un bouton et ne bloque pas l'écran, c'est un popover (ou un dropdown menu s'il ne contient que des actions).
- **Popover ≠ dropdown menu.** Que des items cliquables qui ferment le calque : dropdown menu. Des champs, interrupteurs ou contenu libre : popover.
- **Tooltip = jamais interactif.** S'il faut cliquer dedans, c'est un popover ou une hover card.
- **Toast ≠ alerte bloquante.** Un toast n'exige rien de l'utilisateur. Une erreur qui demande une décision va dans un alert dialog ou en ligne, près du champ.
- **Drawer ≠ sheet.** Le drawer vient du bas (mobile). Sur desktop, un panneau latéral est une sheet.
- « Popup », « fenêtre », « bulle », « panneau » ne sont pas des termes : toujours les traduire.

## 3. Traduire une demande floue

| L'utilisateur dit | Terme probable | Question à poser si doute |
|---|---|---|
| « une fenêtre qui s'ouvre au milieu » | Modal | Faut-il pouvoir fermer en cliquant dehors ? Sinon alert dialog. |
| « un menu quand je clique sur mon nom » | Dropdown menu | Contient-il autre chose que des liens et actions ? Sinon popover. |
| « un panneau qui glisse à droite » | Sheet | Le reste de l'écran reste-t-il utilisable ? Si oui, c'est une sidebar. |
| « une petite bulle au survol » | Tooltip | Faut-il cliquer dedans ? Si oui, hover card ou popover. |
| « une notif qui confirme » | Toast | Faut-il une action de l'utilisateur ? Si oui, pas un toast. |
| « un truc qui monte du bas sur mobile » | Drawer | — |
| « le menu à gauche » | Sidebar | — |

## 4. Exigences par terme

Pour chaque calque construit (maquette ou code), vérifier :

- **Modal, alert dialog, sheet, drawer, command palette** : voile derrière (opacité ~30 %), focus piégé dans le calque, Échap ferme (sauf alert dialog), focus rendu au déclencheur à la fermeture, titre accessible (`role="dialog"` + `aria-labelledby`, `role="alertdialog"` pour l'alert dialog).
- **Popover, dropdown menu** : ancré visuellement à son déclencheur (écart de 4 à 8 px), pas de voile, flèches clavier dans un dropdown menu (`role="menu"` / `menuitem`), `aria-expanded` sur le déclencheur.
- **Tooltip** : apparaît aussi au focus clavier, délai court (~300 ms), texte de quelques mots, `role="tooltip"` + `aria-describedby`.
- **Toast** : région `aria-live="polite"`, disparaît seul (4 à 6 s), pause au survol, ne masque pas d'action importante.
- **Sidebar** : `<nav>` avec un libellé, élément actif marqué (`aria-current="page"`).

Dans le code, préférer les primitives existantes du projet (Radix, shadcn/ui, Headless UI, `<dialog>` natif) plutôt que de les réécrire : elles gèrent déjà focus et clavier. Leurs noms de composants suivent ce vocabulaire (`Dialog`, `AlertDialog`, `Popover`, `DropdownMenu`, `Tooltip`, `HoverCard`, `Toast`/`Sonner`, `Sheet`, `Drawer`).

## 5. Nommer dans Figma

Nommer les frames et calques avec le terme exact, suivi de ce qu'ils contiennent : `Sheet · Mes talents`, `Dropdown menu · Profil`, `Modal · Supprimer le deal`, `Toast · Contrat envoyé`. Le voile s'appelle `Overlay`. Ainsi, le nom passe tel quel dans le code et dans les échanges avec l'IA.

## 6. Relecture

Sur une maquette ou une UI existante, lister chaque calque avec : nom actuel → terme exact → écart (mauvais nom, voile manquant, pas ancré, tooltip interactif, etc.). Proposer les renommages et corrections, sans les appliquer sans accord.
