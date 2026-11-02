# family-storyteller — agent notes

Read README.md first: it holds the design principles and the verified research.

- Never commit voice samples, generated audio, models or stories. `.gitignore`
  covers the common formats; check `git status` before every commit anyway.
- The consent-as-voice-sample rule is the core design constraint: no code path may
  clone a voice from imported audio.
- Do not copy code from sipeter/CloneTTS; it has no license. Build on sherpa-onnx and
  ZipVoice (Apache-2.0) directly.
- Mark facts in README with how they were verified; untested claims stay labelled.
- Committed text is English, except user-facing Chinese strings and the consent script.
