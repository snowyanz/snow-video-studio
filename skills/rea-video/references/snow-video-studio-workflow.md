# SNOW VIDEO STUDIO — reusable video-analysis workflow

This project-specific addition supplements `../SKILL.md`; it does not replace
its evidence requirements, visual-review steps, uncertainty rules, or delivery
structure. Apply it when the user supplies a video and requests analysis. Do not
start analysis for an attachment alone, a verification-only request, or a workflow
configuration request.

## 1. Scope and fresh-task preflight

- Stay within this standalone project, its runtime dependencies, and files the
  user supplies for this analysis. Do not connect to other ChatGPT Projects,
  repositories, shared preferences, credentials, or unrelated private files.
- Verify the selected environment, checkout identity, Python, FFmpeg, FFprobe,
  CLI, dependencies, and existing model assets anew. Do not assume a previous
  task's successful checks establish readiness. Report identity mismatches.
- Prefer the existing repository `.venv/bin/rea-video` and `.venv/bin/python`
  when present; the default shell PATH may select an incomplete Python runtime.
  Follow the original installation instructions if needed, subject to the user's
  task restrictions. Never silently download optional AI models during setup.
- Keep source videos unchanged. Record the input filename, SHA-256, dimensions,
  duration, audio streams, frame-rate metadata, and timestamp basis. Recheck the
  hash after processing. Store all extracted assets and reports in a new run
  directory outside the checkout, e.g. `/workspace/video-analysis/<run-id>/`.
  Do not overwrite earlier runs or include the source video in the ZIP by default.
- Set `REA_VIDEO_HOME` to a project-scoped analysis-state directory before using
  the skill's memory/preferences commands. Do not read another project's memory.
  Keep the English report preference in this isolated state; explicit later user
  language requests take precedence.
- Inspect automatic configuration and model-loading paths before execution.
  Do not load unrelated `.env` files or inherited cross-project configuration.
  Use a controlled invocation that avoids those paths when necessary.

## 2. Permission and offline execution

- Default to local analysis with external VLM calls disabled (`--no-vlm`). Do not
  use paid APIs or download optional AI models without explicit user permission.
  A configured credential, catalog entry, or automatic fallback is not permission.
- Enable applicable features whose dependencies and models are already available
  locally. Verify model files without invoking auto-download helpers. Use a
  network-denied execution method where possible; otherwise explicitly prevent
  all download/API paths before calling them. Do not rely on network allowlists
  alone to prevent downloads.
- Check YuNet face detection, UVR stem separation, and non-bundled OCR recognition
  models in particular. `--no-vlm --no-stems` does not disable every download path.
  Use existing local fallbacks when supported, or disable the affected feature
  and record why. Do not modify repository code merely to run the analysis.
- Continue independent local analysis when an optional feature is unavailable.
  If its use would materially improve the requested result, identify the exact
  model/service, purpose, download size or cost if known, and request permission.
  Do not block the entire report while waiting for optional permission.
- Production/remake recommendations are report content only. Do not invoke
  generation, assembly, capsule-cinema, or another project's integration as part
  of the default analysis workflow.

## 3. Required coverage and review

Run the skill's decompose, summary, review, and apply-review workflow using
English output (`--lang en`). Enable applicable local OCR, packaging, audio,
and stem features when the preflight permits them. Do not silently omit a feature
to shorten the task. After review, regenerate reports and read the final summary.

Maintain a coverage table in the delivered reports with one row for each area:

| Area | Coverage to attempt |
|---|---|
| Shot timing | Shot starts/ends/durations, frame indices and their convention, transitions, uncertain or missed cuts |
| Camera | Framing, camera movement/support, shake, speed changes; separate lens/equipment guesses from measurements |
| Lighting and grade | Direction, softness, contrast, color, likely setup; distinguish visible evidence from inferred equipment |
| Editing | Cut rhythm, jump cuts, punch-ins, inserts, transitions and audio/beat alignment |
| VFX and packaging | Text, stickers, picture-in-picture, particles, compositing and possible AI edits; manual review for semantic VFX claims |
| Audio | Voice, music, tempo/key/sections, sound-effect candidates and event alignment; stems when locally available |
| Subtitles | OCR text, timing, style and SRT/ASS when supported; distinguish visible-caption OCR from speech transcription |
| Storyboard | Whole-video storyboard, per-shot contact sheets and relevant detail frames |
| Production | Production-method evidence, cast/scenes/props, real-material inventory, planning recommendations and uncertainty |
| Detailed action | Apply section 4 for fights, choreography, sports or other timing-sensitive sequences |

For each row, state `completed`, `partial`, `unavailable`, `not applicable`, or
`untested`, with evidence, limitations, and needed manual validation. A capability
catalog entry is not proof of an installed or working implementation. Do not
claim comprehensive VFX reconstruction, source attribution, or action recognition
from heuristic output alone.

Follow the original independent visual review, especially for low-confidence
shots. Review audio only with available listening capabilities; never claim to
have heard it if only signal measurements were inspected. Preserve measured,
external-model, and agent-review evidence as separate sources. Leave unseen
shots unreviewed and explicitly flag unresolved disagreements.

## 4. Detailed action sequence timing

Prioritize detailed timing when actions are central to the video or requested by
the user. The standard storyboard and sampled shot features are insufficient to
establish frame-accurate action boundaries.

- Establish the decoded frame order and presentation timestamps; check variable
  frame rate, duplicate/dropped frames, slow motion, and discontinuities. Document
  zero-/one-based indexing, endpoint conventions, source time origin, and any
  conversion. Do not assume frame number divided by nominal FPS is always valid.
- Inspect relevant sequences densely and inspect consecutive original decoded
  frames around candidate events. Use FFmpeg/FFprobe and local image inspection
  where available. Do not interpolate frames or alter playback timing to infer
  source event times.
- Record action phases where visible: preparation, initiation, approach,
  apparent contact/block/dodge, follow-through, and recovery. Keep participant
  identities consistent; record camera cuts, occlusion, motion blur and offscreen
  events that limit interpretation. Do not invent hidden contact or exact hits.
- Produce `action-timeline.md` and `action-timeline.json`. Each event must include
  an event ID, shot ID, participant(s), action/phase description, start/end seconds,
  frame indices when validated (otherwise null), evidence-frame references,
  timing method, confidence, validation status, uncertainty interval when known,
  and notes. Distinguish measured frame timestamps from estimated semantic action
  boundaries even when both appear in the same row.
- Use explicit timing labels such as `measured timestamp; action estimated`,
  `frame boundary manually validated`, or `unvalidated estimate`. Never claim
  frame accuracy merely because a tool prints frame indices or decimal seconds.
  Validate each claimed boundary against source frames and timestamps and record
  the validation method. Express uncertain events as intervals where appropriate.
- If automated pose tracking, multi-person tracking, or action recognition is
  needed beyond the installed capabilities, explain the gap and request permission
  before adding optional models or paid services. Continue useful manual analysis
  and clearly mark any remaining gaps.

## 5. Complete English delivery and downloadable bundle

- Deliver `report.html`, `report.md`, `breakdown.json`, and `storyboard.jpg`, plus
  generated shot images, `audio.png`, packaging timeline/subtitle files, review
  files and audio stems where applicable and available. Include the additional
  action-timeline files when section 4 applies. These action reports are supplemental
  agent-authored artifacts, not claimed native rea-video exports.
- Preserve the skill's report structure and generated analysis schema. If coverage,
  timing methodology or limitations cannot be represented by the built-in report,
  include them in `analysis-notes.md` and an HTML companion, linked from the main
  reports after the final regeneration. Link applicable action reports there too.
  Do not fabricate missing outputs or empty success placeholders.
- Reports and agent-authored descriptions must be in English by default. Preserve
  original subtitle quotations and proper names, with translations labeled as such.
- Include a manifest listing the input hash, tool versions, commands/options,
  enabled/disabled features, feature-status table, output files, timing conventions,
  validation performed, and unresolved limitations. Exclude secrets and unrelated
  local paths or configuration contents.
- Verify JSON parses, expected outputs are nonempty, HTML/image links resolve,
  source hashes match, and timing ranges are consistent with the source duration.
  Validate the ZIP's integrity and contents. Do not equate structural checks with
  accuracy validation.
- Create a ZIP of the complete report folder, preserving relative asset paths.
  Exclude source videos, credentials, project memory and unrelated files. Provide
  downloadable ZIP and individual report links using the current Codex file/artifact
  mechanism. Do not claim successful download delivery if the interface cannot
  expose it; report the actual path and specific limitation instead.
- Finish with the main findings, coverage/accuracy limitations, manual-validation
  gaps, and links. Do not paste the entire JSON into chat. If any required artifact
  failed, label the delivery partial and explain what remains.
