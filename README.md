# aegis-storyteller

An open-source framework that helps everyone tell stories to the children in their
life, in their own voice, on their own phone.

Status: **design stage, no code yet.** Research below was gathered on 2026-11-02
from project pages and READMEs; nothing has been installed or tested yet.

## Goal (owner, 2026-11-02)

The goal is to get more people telling stories. Grandparents are the starting point
and the first use case: someone who would like to tell stories to their
grandchildren but finds reading aloud tiring or difficult. The same framework
serves any storyteller (a parent, an aunt, a teacher), so nothing in the design is
specific to grandparents.

## First use case: a grandparent's voice

1. Grandma reads a short consent script aloud into the app.
2. That same recording is the voice sample. A zero-shot TTS model clones her voice
   on the phone.
3. Any story (PDF, text) can then be read aloud in her voice, without her reading it.

The repository ships the code and the built-in public-domain stories. Voices, and
stories a family adds, stay on the family's phone and never enter the repository.

## Design principles

- **Consent recording is the voice sample.** The voice can only be cloned from a
  live microphone recording of the person reading the consent script. No audio file
  import. This ties every cloned voice to a spoken, recorded agreement and blocks the
  obvious misuse (cloning someone from a clip taken elsewhere).
- **On-device only.** Voice samples and generated audio stay on the phone. No cloud
  upload.
- **Revocable.** The speaker can delete their voice at any time, and deletion removes
  the sample and every derived artifact.
- **Stories are the family's responsibility.** Any built-in sample stories must be
  public domain or written by the project. Stories a family adds later are theirs to
  clear.
- A voice is personal data, not copyrighted content. The consent script says what is
  recorded, what it is used for, where it is stored, who uses it, and that it can be
  deleted. This is engineering design, not legal advice.

## Built-in stories: Grimm's fairy tales for ages 5–7 (owner decision, 2026-11-02)

The framework ships with Chinese (Traditional) Grimm's fairy tales suited to
children aged 5–7, in `content/grimm/` (not `stories/`, which is ignored as user data).

Copyright approach:
- The German originals (*Kinder- und Hausmärchen*, 1812–1857) are public domain.
- **Existing Chinese translations are not assumed to be public domain.** A
  translation is its own copyrighted work, protected for decades after the
  translator's death. A 2026-11-02 search found no Chinese translation with a clear
  public-domain status, so none is copied or used as a base.
- The project writes its **own Chinese retellings** from the German public-domain
  text and licenses them with the repository. Each file records its source tale
  (KHM number and edition) so provenance is checkable.

Age 5–7 adaptation:
- Retell, do not translate literally: short sentences, everyday words, a length that
  reads aloud in about 5–10 minutes.
- Soften violent or frightening details from the originals (e.g. mutilation and
  cruel punishments) while keeping the plot.
- Text is written to be read by TTS: avoid rare characters and polyphones where a
  plain word will do, since ZipVoice's pinyin conversion can misread them.

Open: which tales make the first set, and who reviews each retelling before it ships.

## Target device

Motorola moto g34 5G: Snapdragon 695, sold in **4 GB and 8 GB RAM** variants.
Which variant Grandma has is not yet known; it decides the model size.

## Candidate stack (verified 2026-11-02 via GitHub API unless noted)

| Component | Project | License | Notes |
|---|---|---|---|
| On-device inference | [k2-fsa/sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) | Apache-2.0 | Android/JNI; active (pushed 2026-10-29) |
| Zero-shot cloning model | [k2-fsa/ZipVoice](https://github.com/k2-fsa/ZipVoice) | Apache-2.0 | Chinese + English; pinyin via pypinyin, manual `<pinyin>` override for polyphones |
| Reference implementation | [sipeter/CloneTTS](https://github.com/sipeter/CloneTTS) | **none (all rights reserved)** | Android system TTS engine on sherpa-onnx + ZipVoice; v0.7.1 on 2026-10-26. Proves the stack runs on Android. **Do not copy its code.** |
| PDF reader (uses system TTS) | [foobnix/LibreraReader](https://github.com/foobnix/LibreraReader) | NOASSERTION (MuPDF is AGPL-3.0) | Reads PDF/EPUB aloud via the system voice |
| Taiwanese Mandarin (PC only) | [MediaTek-Research/BreezyVoice](https://huggingface.co/MediaTek-Research/BreezyVoice) | Apache-2.0 | Bopomofo pronunciation control; too heavy for the phone, usable for pre-generating audio |

Unverified, from a third-party wrapper's docs: full ZipVoice is about 605 MB and
needs significant RAM; an int8 distilled variant (about 104 MB) is suggested for
phones under 8 GB, and it needs a vocoder (e.g. vocos_24khz) to initialise.

Avoid: [gedoor/legado](https://github.com/gedoor/legado). Its repository was emptied
and replaced with a copyright-infringement notice.

## Known gaps

- Mainland pinyin: words read differently in Taiwan (e.g. 垃圾, 星期) will use the
  mainland reading. The voice itself should still sound like the speaker.
- Traditional Chinese input through pypinyin: untested.
- Taiwanese Hokkien: not supported by this stack. Candidates exist
  (MERaLiON-OmniVoice-Hokkien-TTS, KaedeTai/gpt-sovits-tw) but are untested.
- Real-time speed on a Snapdragon 695: unknown.

## Next step

Before writing code: install CloneTTS + Librera on the target phone, clone a voice,
and read a Chinese PDF. Record three things: does it sound like her, how many
characters are mispronounced, and is the pause between sentences tolerable. If
real-time is too slow, the framework pre-generates audio instead of reading live.

## Open decisions (owner)

- App shape: our own reader app, or a system TTS engine that any reader can use.
- Which Grimm tales make the first set, and who reviews each retelling.
- Whether to also support real recordings: a storyteller who can read aloud records
  the story themselves, and the cloned voice fills only the parts they skip. This
  serves the goal of more people telling stories, not only more stories being read.

See [docs/consent-script.zh-TW.md](docs/consent-script.zh-TW.md) for the draft
consent script.

## License

[Apache License 2.0](LICENSE), matching sherpa-onnx and ZipVoice. This covers the
code and the project's own Grimm retellings in `content/grimm/`. Voices and stories
that a family adds are theirs and are not part of the repository.
