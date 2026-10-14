# Origami

## Names

Anything Claude names itself in this repo gets a small paper or origami reference. One or two words, never at the cost of clarity.

- Branches Claude creates: `claude/<fold>-<topic>`, like `claude/valley-fold-readme` or `claude/crane-weather`.
- Session titles: the same idea, like "Crease: README fixes".
- Releases: `Origami <version> · <name>`. 2.0 is The Unfolding. Ideas for later ones: Valley Fold, Mountain Fold, Squash Fold, Petal Fold, Crane, Waterbomb, Kite Base, Washi.
- Scratch files can use the same words. Anything a screenshot shows, like a demo entity's name, stays plain.

Commit messages, PR titles and the README stay plain.

## Writing

The docs must read like plain, natural English from a native technical writer. This applies to the README, the changelog, the module reference, module descriptions and code comments.

- Every sentence has a subject and a verb. Never use fragments as sentences, also not in lists. "One sections view, made for phones." is wrong. "The dashboard is a single sections view for phones." is right.
- No first person. Describe the project, not its author.
- No colon inside a sentence. A colon may only end a sentence that introduces a list, a table or a code block.
- No dashes, semicolons, exclamation marks, contractions or abbreviations like e.g. and etc.
- No idioms, buzzwords or marketing words, such as swap, under the hood, calm, matters, made for, grown, jumps, out of the box, seamless or powerful. No metaphors.
- No bold labels in front of fragments, such as "**Media.** The TV while it's on".
- Use possessives where a native writer would ("the room's light group", "Google's servers"). Avoid long "of the" chains.
- Prefer short sentences, but vary how they start.
- Check every claim against the YAML before writing it. The entities are examples from one home, so say what is optional.
- No AI attribution anywhere: no `Co-Authored-By` trailers, no "Generated with" footers in commits, PRs or comments.

## Images

- Screenshots come from a real Home Assistant with the real theme, font and cards.
- Before using a screenshot, look at it at full size and check the spacing against the rest of the dashboard. Measure gaps in the browser instead of guessing.
- Phone mockups use the 3D phone with the camera at eye level. The phones stand upright, and the screens are never tilted away.
- README images use absolute `raw.githubusercontent.com/hazymorning/origami/main/...` URLs, because HACS doesn't rewrite relative ones.
