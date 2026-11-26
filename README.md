# VoiceLib.pack

Model files for VoiceLib's on-device speech engines (VoiceLib is apphaus's speech library for iOS, Android, and Meta Quest), kept as GitHub releases so an app's model download has somewhere to go when the model's own upstream source (Hugging Face for most) fails. VoiceLib asks for a file here only after its upstream has failed, and verifies every file it unpacks against the SHA-256 its manifest pins, so a file from here is byte for byte the upstream's.

## These are not our models

Every file in these releases is a third party's work, redistributed under that party's own licence. The `LICENSE` in this repository (MIT) covers only this repository's own text; it does not cover the model files. Each release carries:

- the model's licence files, as the upstream publishes them, among its assets;
- `voicelib-attribution.txt`: the model's name, its SPDX licence, the attribution its authors ask for, the upstream sources pinned to a commit, and each voice's own licence;
- release notes with the same attribution and a table of every asset with its path, size, and SHA-256.

Only models whose licences allow redistribution are here, and never a voice or a model whose licence forbids commercial use. A model whose redistribution terms are not plain from its licence and its source page is not here; VoiceLib simply has no such source for it and downloads it from its upstream alone.

The Pocket TTS releases (`pocket-tts-en`, `-de`, `-es`, `-fr`, `-it`, `-pt`) are mirrored copies of files published upstream under CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/legalcode): the model by Kyutai at https://huggingface.co/kyutai/pocket-tts, the ONNX files at https://huggingface.co/IgnitiveLabs/PocketTTS-ONNX and https://huggingface.co/OpenVoiceOS/phoonnx-pocket-tts, and the voices' licences at https://huggingface.co/kyutai/tts-voices; the two non-commercial voices (`cosette`, `jean`) are not here.

If you use these files, the licence and attribution of each model apply to you as they would to a copy from the upstream.

## How the names map

One release per model version, never changed once published:

- **Tag**: `<id>@<version>`, the model's id and version in VoiceLib's manifests, for example `silero-vad@v6.2.3` or `kokoro-82m@v1.0`.
- **Asset**: the file's path inside the model's directory with every `/` written `__`, for example `voices/af_heart.bin` becomes `voices__af_heart.bin`. GitHub's asset names are flat and keep only letters, digits, `.`, `-`, and `_`.
- **Compression**: an asset whose name ends in `.xz` is that file compressed (LZMA2 in the `.xz` container, a dictionary of at most 8 MiB, in blocks of 64 MiB). Files under 1 MiB and files that are compressed already (`.zip`) are stored as they are. The SHA-256 in the notes is always the raw file's.

So the file `<id>/<version>/<path>` of VoiceLib's model store is at

```
https://github.com/apphaus2/VoiceLib.pack/releases/download/<id>@<version>/<path with / as __>[.xz]
```

The mapping is one function in VoiceLib's core (`vl_core::models::packed::release_asset`), used by the core, by the pack tool that writes the assets, and by the script that uploads them (`scripts/upload-model-releases.sh` in VoiceLib).

To unpack a file by hand: `xz -d -k <asset>.xz`, then `shasum -a 256` on the result and compare it with the notes.
