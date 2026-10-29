# Working on this repository

Maintained fork of philm5/delonghi-comfort-integration (Apache-2.0), a
Home Assistant custom integration for De'Longhi Comfort devices. The owner
uses it for one Pinguino air conditioner (`climate.pinguino`).

- **Branches:** `dev` for work, `main` for releases. Push to `dev`; merge to
  `main` only for a release the owner asked for. Ask before pushing.
- **Keep the domain `delonghi_comfort`** and the entities' unique IDs:
  changing either makes Home Assistant create new devices and entities.
- **Release:** bump `version` in `manifest.json`, move *Unreleased* in
  `CHANGELOG.md` to `## vX.Y.Z — date`, merge to `main`, tag `vX.Y.Z`, then
  `gh release create vX.Y.Z --verify-tag --notes-file <that section>`. There
  is no release workflow on purpose; `validate.yml` checks every push.
- **Testing:** there are no offline tests — it talks to De'Longhi's cloud.
  `python -m py_compile custom_components/delonghi_comfort/*.py`, then try
  it on the owner's Home Assistant (installed through HACS as a custom
  repository) before calling a change done. Don't change the Pinguino's
  settings while testing beyond what the change needs.
- **Upstream:** remote `upstream` is philm5's repository. Its open PRs and
  issues (dehumidifier #7, "No devices found" #8) may be worth pulling in.
  Keep `LICENSE` and `NOTICE`; list every change in the CHANGELOG.
- **No secrets** in the repo: the De'Longhi login lives only in Home
  Assistant's config entry.
- README and CHANGELOG follow the owner's house style (see `homelab.md` →
  *Every git repository*); update both with every user-visible change.
