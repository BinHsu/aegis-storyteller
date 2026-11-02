# aegis-storyteller — agent notes

Read README.md first: it holds the design principles and the verified research.

- Never commit family-added stories (`stories/`). `.gitignore` covers voices, audio,
  models and `stories/`; check `git status` before every commit anyway.
- The consent-as-voice-sample rule is the core design constraint: no code path may
  clone a voice from imported audio.
- Do not copy code from sipeter/CloneTTS; it has no license. Build on sherpa-onnx and
  ZipVoice (Apache-2.0) directly.
- Never commit voice samples, generated audio or models. Built-in Grimm stories in
  `content/grimm/` are the project's own retellings from the German public-domain
  originals; never copy or lightly edit an existing Chinese translation.
- Mark facts in README with how they were verified; untested claims stay labelled.
- Committed text is English, except user-facing Chinese strings and the consent script.
