# Clinical Educator — course library

A FAIR content library: the **Clinical Educator** course (120 hours,
5 ECTS) as version-controlled markdown, rendered to template-bound
PowerPoint decks and published as a queryable catalog. Built on the
[FAIR pipeline](https://github.com/imbowen1973/FAIR); authoring format
reference:
[session-md-format](https://github.com/imbowen1973/FAIR/blob/main/docs/session-md-format.md).

## Layout

```
sessions/                 one .md per session (the course content)
competencies/framework.yaml   CE1–CE5: labels, ESCO mapping slots
credentials/clinical-educator.yaml   the 5 ECTS credential definition
template.pptx             stand-in brand; replace with the real one
layout-map.yaml           region → placeholder binding contract
.github/workflows/publish.yml   renders + publishes on push to main
```

Sessions are the atomic content unit and are tagged with the
competencies they develop; the credential file declares the course
structure, workload, and required competencies over them. The corpus
build validates the credential against the actual sessions (unknown
refs fail; contact-hour drift warns — expected while only 2 of the
planned sessions exist).

## Publishing (one-time setup)

Repo **Settings → Pages → Source: "GitHub Actions"**. Every push to
main then republishes `https://agrifoodskills.github.io/Clinical-Educator-/data/`.

To use it in PowerPoint: open the FAIR assembler pane, and in
**Library → Add** paste:

```
Agrifoodskills/Clinical-Educator-
```

## Authoring

Edit or add `sessions/*.md`, then check locally:

```bash
pip install "fair-renderer @ git+https://github.com/imbowen1973/FAIR.git@main#subdirectory=renderer"
fair-corpus --sessions sessions --template template.pptx \
  --layout-map layout-map.yaml --framework competencies/framework.yaml \
  --credentials credentials --out /tmp/ce-build
```

House rules (CI-enforced):

- images ≤ 500 KB and ≤ 2200 px — prepare with FAIR's
  `prepare_image.py`, which also strips EXIF (clinical photos must not
  carry GPS/device metadata)
- **no video files in the repo** — host them (YouTube etc.) and use
  `{type: video, url: ...}`; the slide gets a click-through poster
- competency labels live in `competencies/framework.yaml`, not in
  session files

## Course status

| Module | Sessions | State |
|---|---|---|
| Foundations of clinical teaching | ce-01 | draft |
| Assessment and feedback | ce-02 | draft |
| Remaining modules (~10 sessions) | — | planned |

The `ce01-05` video URL is a placeholder — record/select the teaching
episode video, host it, and update the link.
