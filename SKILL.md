---
name: beautyplus-ai
description: "A comprehensive AI image editing and beautification suite. Use this tool for portrait retouching, body reshaping (breast/butt), AI hair & AI Clothes changes, expression modification, photo restoration (upscaling), and artistic filters."
version: 1.0.4
author: BeautyPlus
metadata: {"openclaw":{"emoji":"🖼️","requires":{"bins":["python3"],"env":{"BP_AK":{"required":true},"BP_SK":{"required":true}}},"tags":["image-processing","portrait-retouching","body-reshape","virtual-try-on","photo-restoration","image-upscaling", "ai-cosplay", "makeover"]}}
---

# BeautyPlus Skills & API

## When to Use This Skill

Activate this skill whenever the user requests any modifications, enhancements, or artistic changes to a **photo / image** (provided via path, URL, or attachment). 

**Trigger Intent Keywords for Agent:** 
"Make me look better", "change my clothes", "fix this blurry photo", "make me smile", "enhance my body", "change my hair color", "change my hair style","add a filter", "cosplay", "upscale".

## 🛠️ Effects Dictionary (How to choose the right effect)

Agent Instruction: Analyze the user's request. Find the matching category below, select the most appropriate specific style/effect, and pass the corresponding `Effect KEY` to the API.

### 1. 🪄 Portrait Retouch & Makeup
*   **Use when:** The user wants to look prettier, remove blemishes, apply makeup, or improve overall facial aesthetics without changing identity.
*   **Available Effects:** 
    *   **Natural Beauty**: Subtle enhancement, clear skin.
    *   **Glamour Beauty**: Full makeup, high-end look.
    *   **Sweet Beauty / Youthful**: Brighter, softer, younger appearance.

### 2. 🎭 Expression Changer
*   **Use when:** The user wants to change the emotion or facial expression of the person in the photo.
*   **Available Effects:** 
    *   **Smile**: Open Smile, Closed Smile.
    *   **Other**: Wink, Cool Expression.

### 3. 👗 AI Clothes Changer
*   **Use when:** The user asks to change AI Clothes, try on different styles, or dress up for a specific occasion.
*   **Available Categories & Effects:**
    *   **Formal / Evening:** Gowns, Rhinestone mesh dress, Beaded dress, Black suit.
    *   **Vacation / Casual:** Slip dress, Puff dress, Hoodie, Lace corset, Bikini overlay.
    *   **Party / Y2K:** Floral Camisole, Red Latex, Shimmer Dress, Y3K Set.
    *   **Sports:** Tennis Set, Racing Suit, White Yoga, Brazilian Bikini.
    *   **Cosplay / Fantasy:** Bunny cop, Fox shirt, Deer girl, Grinch, Victoria Angel, Dallas Cowboy.

### 4. 💇‍♀️ Hair Styling & Coloring
*   **Use when:** The user wants a new hairstyle or hair color.
*   **Available Effects:**
    *   **Colors:** Natural black, Blonde, Brown highlights, Platinum, Silver platinum, Teddy warm brown.
    *   **Styles:** Glossy hair, Layered cut, Soft waves, Latino curls.

### 5. ⏳ Body Reshape
*   **Use when:** The user specifically asks to enhance, enlarge, or reshape their body parts (breast or buttocks).
*   **Available Effects (Each supports Strong / Medium / Weak tiers):**
    *   **Breast Enhancement:** Natural, Teardrop, Round, Outward.
    *   **Butt Enhancement:** Peach butt, O-shape.

### 6. 🖼️ Photo Restoration & Quality
*   **Use when:** The image is blurry, noisy, old, or low resolution. Keywords: "fix", "clear", "upscale", "enhance quality".
*   **Available Effects:** AI Ultra-HD Upscaling, Denoise, Repair.

### 7. 📸 Photo Art & Filters
*   **Use when:** The user wants a specific camera vibe, lighting effect, or skin tone change.
*   **Available Effects:** 
    *   **Filters/Flash:** CCD Flash, Film Flash, Fuji Flash.
    *   **Skin Tone:** Tanning Filter.

**Effect KEY:** The CLI `--task` value must be the **effect KEY** string from the table below. The algorithm spec for each key is returned inline by **`POST /skill/consume.json`** (`invoke_spec`) — do not hard-code AIGC paths.

**⚠️ Crucial Agent Instruction:** 
When calling the tool, you MUST map the user's intent to the exact `<EFFECT_KEY>` required by the API. If the user request is vague (e.g., "make this photo look better"), default to the **Portrait Retouch (Natural Beauty)** or **Photo Restoration** effect based on image quality.

For detailed information on API and skill key integration and use cases, please refer to: [https://www.beautyplus.com/developers](https://www.beautyplus.com/developers/console)

For an overview of BeautyPlus products and features, please visit: [https://www.beautyplus.com](https://www.beautyplus.com)

## Billing and user-facing claims (MANDATORY)

- **Fact:** Each successful **`run-task`** (including inside a **`sessions_spawn`** worker) goes through server-side **quota / credit consumption** for the **BP_AK** tenant. This is a **paid, metered commercial API**, not free compute bundled with the skill or the host.
- **Forbidden:** Do **not** state or imply that the service is **free**, costs nothing, uses **no quota**, has **unlimited trial**, or similar. Do **not** invent **prices**, **plan names**, **promotions**, or **trial rules**.
- **Allowed:** Neutral wording — e.g. processing **uses the BeautyPlus account quota** tied to the configured keys; **billing and plans** are **per your console or administrator**. If the user asks about cost, point them to **admin / official billing docs / console**; do not guess. When the API returns quota or membership errors, follow **Step 3 — MANDATORY (quota / consume failures)** using server **`detail`** and **`pricing_url`** when present.
- **On success too:** Success summaries must stay factual (task completed, delivery). Do **not** add “free” or zero-cost implications.

## Supported Algorithms (effect KEY → `--task`)

All rows use **image** input. Algorithm params for each key are returned as `invoke_spec` by **`POST /skill/consume.json`** — do not hard-code AIGC paths.

### 1. Body Reshape
**Rule:** Select the base effect and append the tier suffix `_strong`, `_medium`, or `_weak`. (Default: `_medium`).
*Example: For a strong natural breast enhancement, output `breast_natural_strong`.*

*   **Breast Shapes:**
    *   `breast_natural` : Natural-looking fullness; subtle lift.
    *   `breast_teardrop`: Teardrop contour (fuller lower pole).
    *   `breast_round`   : Rounded, lifted look with upward emphasis.
    *   `breast_outward` : Fashion-editorial spread with outward emphasis.
*   **Buttocks Shapes:**
    *   `butt_peach`     : Lift and side volume for a rounded peach shape.
    *   `butt_o_shape`   : Smooth, continuous curve with even side profile.

### 2. Hair Editing
*   **Hair Color:**
    *   `hair_black`            : Deep black shine; natural look.
    *   `hair_blonde`           : Classic golden blonde.
    *   `hair_brown_highlights` : Brown with highlights for dimension.
    *   `hair_platinum`         : Soft creamy platinum.
    *   `hair_silver_platinum`  : Cool metallic silver; edgy modern look.
    *   `hair_teddy_brown`      : Warm soft brown; youthful vibe.
*   **Hair Style:**
    *   `hair_glossy`       : Extra shine and sleek fall.
    *   `hair_high_layer`   : Light layers and airy volume.
    *   `hair_soft_waves`   : Romantic large waves.
    *   `hair_latino_curls` : Tight curls, maximum volume.

### 3. Outfits Changer
**Rule:** Use the exact KEY for the `--task` argument to change the user's clothing.

**Formal / Evening:**
*   `dress_yellow_gown`   : Vivid yellow silk evening gown.
*   `dress_arctic_allure` : Rhinestone mesh gown with galaxy sparkle.
*   `dress_ostrich_feather`: Ethereal feather gown.
*   `dress_muse_goddess`  : Couture-heavy beaded goddess gown.
*   `suit_tartan_eve`     : Smart, polished British tartan suit.
*   `suit_red_carpet`     : Classic sharp black suit.

**Vacation / Casual:**
*   `accessory_bunny_ear`   : Playful bunny ear accessory.
*   `dress_butter_moonlight`: Pale yellow slip dress; fresh light look.
*   `dress_pink_puffy`      : Tiered pink puff dress; sweet portrait style.
*   `dress_gold_hoodie`     : Urban casual hoodie dress.
*   `dress_lace_corset`     : French-inspired lace corset set.
*   `dress_chiffon_cake`    : Tiered chiffon maxi; relaxed vacation mood.
*   `dress_sheer_bikini`    : Bikini under sheer cover; resort look.

**Cosplay / Fantasy:**
*   `cosplay_carnival`      : Carnival samba outfit with feather headpiece.
*   `cosplay_bunny_cop`     : Navy bunny police uniform.
*   `cosplay_fox_boyfriend` : Green fox print shirt; relaxed vibe.
*   `cosplay_deer_girl`     : Forest fawn-inspired mini skirt.
*   `cosplay_grinch`        : Green fuzzy Grinch party costume.
*   `victoria_angel`        : Lingerie set with wings (runway showstopper).
*   `dallas_cowboy`         : Iconic blue-and-white cheerleader uniform.

**Party / Y2K:**
*   `floral_camisole` : Botanical floral cami.
*   `puff_skirt`      : Strapless puff corset dress.
*   `one_shoulder_lbd`: One-shoulder little black dress.
*   `red_latex`       : Bold red latex mini skirt.
*   `moonlight_dress` : Full-rhinestone bodycon long gown.
*   `y3k_set`         : Futuristic metallic Y3K co-ord.

**Sports / Activewear:**
*   `brazilian_bikini`: Brazilian-cut bikini.
*   `tennis_set`      : Sporty athletic-chic tennis skirt and top.
*   `cozy_fit`        : Relaxed hoodie and jogger (urban athleisure).
*   `racing_suit`     : Bold motorsport racing driver suit.
*   `white_yoga`      : Minimal white yoga set.

### 4. Facial Retouch & Expression
*   **Face Style (Makeup & Retouch):**
    *   `natural_beauty`  : Soft natural enhancement.
    *   `glamour_beauty`  : Polished and charismatic look with makeup.
    *   `sweet_beauty`    : Youthful sweet-girl vibe.
    *   `luminous_beauty` : Radiant glow finish; bright complexion.
    *   `youthful_beauty` : Fresh and energetic style.
*   **Expression Changer:**
    *   `closed_smile`    : Gentle closed-lip smile.
    *   `open_smile`      : Wide toothy grin.
    *   `cool_expression` : Serious, composed, and edgy.
    *   `wink`            : Single-eye wink animation.

### 5. Photo Quality & Art Filters
*   **Photo Restoration (Quality Upgrade):**
    *   `photo_restoration_v3`: Denoise, deblur, reduce artifacts (keeps natural look).
    *   `ai_ultra_hd_v3`      : Deep-learning upscale and detail recovery for old/small photos.
*   **Photo Art (Lighting & Filters):**
    *   `tanning_filter` : Warm bronzed tan skin filter.
    *   `ccd_flash`      : Vintage CCD-camera flash effect.
    *   `film_flash`     : Film grain with flash overlay.
    *   `fuji_flash`     : Fujifilm-style flash; soft grain and warm tones.

---

## Multi-stage pipelines (chaining tasks)

When the user asks for **more than one** BeautyPlus step on the **same** media (e.g. **photo restoration** then **AI Clothes change**), treat each step as a **separate job** with its own **`--task`** (effect KEY):

| Typical chain | Stages |
|---|---|
| Image (example) | `photo_restoration_v3` → `dress_yellow_gown` |
| Image (example) | `ai_ultra_hd_v3` → `hair_soft_waves` |

**Rules:**

1. After stage A completes (`skill_status: "completed"`), pass `primary_result_url` or `output_urls[0]` as `--input` for the next `--task`. That is a new job, not a retry.
2. **"Do not re-run `run-task`"** means: do not resubmit the same `task_id`. It does **not** forbid the next pipeline stage with a different effect KEY.
3. **Delivery:** Prefer final-stage delivery for the full pipeline. Deliver after the last stage only.
4. **Video chains (reserved):** One `sessions_spawn` = one embedded `run-task`. Chain = multiple spawns. Current catalog uses §3a only.

See also Step 3 success bullets and **`agent_instruction`** in the JSON.

---

## API submission path (MANDATORY)

- **New jobs:** Submit **only** via **`python3 {baseDir}/scripts/beautyplus_ai.py run-task …`** (§3a / §3b), or the **same** `run-task` command embedded in **`spawn-run-task`** → `sessions_spawn`. **Do not** hand-craft HTTP to the skill’s **wapi** gateway or **AIGC / invoke** endpoints to replace that flow — that skips **`POST /skill/consume.json`** (quota and permission) and breaks the supported pipeline.
- **Exception:** **`query-task --task-id`** is **only** for resuming status polling on an **existing** full `task_id` (no upload, no second consume). **Do not** use it instead of **`run-task`** for a **new** submission.
- **No curl replay:** This skill does not emit debug curl for API calls. **Do not** hand-craft HTTP to **wapi / AIGC** to mimic requests — always use the **CLI** above so **`/skill/consume.json`** runs before algorithm submit.

---

## 0. Pre-Flight Check (MANDATORY — run before anything else)

Verify AK/SK are configured (**only run this command**; do not read other Python sources first):

```bash
python3 {baseDir}/scripts/beautyplus_ai.py preflight
```

- Output `ok` → continue to Step 1
- Output `missing` → **stop** and send the user the configuration message below

**Feishu** — send an interactive card via the Feishu API (do not use the `message` tool for this):

```python
import json, urllib.request
cfg = json.loads(open("/home/ec2-user/.openclaw/openclaw.json").read())
feishu = cfg["channels"]["feishu"]["accounts"]["default"]
token = json.loads(urllib.request.urlopen(urllib.request.Request(
    "https://open.feishu.cn/open-apis/auth/v3/tenant_access_token/internal",
    data=json.dumps({"app_id": feishu["appId"], "app_secret": feishu["appSecret"]}).encode(),
    headers={"Content-Type": "application/json"}
)).read())["tenant_access_token"]
card = {
    "config": {"wide_screen_mode": True},
    "header": {"title": {"tag": "plain_text", "content": "🖼️ BeautyPlus — credentials required"}, "template": "blue"},
    "elements": [{"tag": "div", "text": {"tag": "lark_md", "content": "1. Apply for **Access Key** and **Secret Key** at [BeautyPlus Developers](https://beautyplus.com/developers).\n2. Set **BP_AK** and **BP_SK** in `scripts/.env` (see `scripts/.env.example`), then reload env:\n```\nsource scripts/.env\n```\nIf keys are issued by your organization, ask your administrator."}}],
}
urllib.request.urlopen(urllib.request.Request(
    "https://open.feishu.cn/open-apis/im/v1/messages?receive_id_type=open_id",
    data=json.dumps({"receive_id": "<USER_OPEN_ID>", "msg_type": "interactive", "content": json.dumps(card)}).encode(),
    headers={"Authorization": f"Bearer {token}", "Content-Type": "application/json"}
))
```

**Telegram / Discord / other channels** — use the `message` tool with plain text:

```
🖼️ BeautyPlus — credentials required

1. Get Access Key and Secret Key (apply here if needed):
   https://beautyplus.com/developers

2. Set BP_AK and BP_SK in scripts/.env (see scripts/.env.example), then run:
   source scripts/.env

If keys are issued by your organization, ask your administrator.
```

---

## Step 1 — Pick effect KEY and input

Choose **`--task`** = **effect KEY** from **[Supported Algorithms](#supported-algorithms-effect-key---task)** (must exist in server **`algorithm.invoke`** after config fetch). Confirm the input file location.

**Intent → effect KEY (MANDATORY checklist):**

1. **Map user intent to one row** — Match feature / effect name / scene to an **effect KEY** in the table (English `snake_case`). If the user only states a broad category (e.g. “change my hairstyle”), **ask one clarifying question** (e.g. soft waves vs. Latino curls) or offer **2–3** KEYs to pick from.
2. **Body reshape tiers** — Natural breast, teardrop, round, outward, peach butt, and O-shape butt each have **strong / medium / weak** (`*_strong` / `*_medium` / `*_weak`). If unspecified, **default to `medium`** or confirm briefly before submit.
3. **Input medium** — All keys are **still images** (`.jpg` / `.jpeg` / `.png` / `.webp` / `.gif` / `.bmp`). Use **§3a** `run-task`. If video keys appear later, follow §3b.
4. **Ambiguous intent** — With no attachment and vague intent, **ask one question** (which effect / any reference image) or pull media from IM per [docs/im-attachments.md](docs/im-attachments.md); do not guess the wrong KEY.
5. **Video + still in one message** — While no video KEYs exist, use the user-specified image as `--input`. If video keys exist later, apply the video path to the video only.

**Getting media from IM messages** (full detail: [docs/im-attachments.md](docs/im-attachments.md)):

| Platform | How to obtain |
|---|---|
| Feishu | Message resource URL / `image_key` + `message_id` → optional **`resolve-input`** |
| Telegram | `file_id` → **`resolve-input --telegram-file-id`** (needs `TELEGRAM_BOT_TOKEN`) |
| Discord | `attachments[0].url` — often usable directly as `--input` |
| Generic | URL or path |

```bash
python3 {baseDir}/scripts/beautyplus_ai.py resolve-input --file /tmp/saved.jpg --output-dir /tmp
# or: --url, --telegram-file-id, --feishu-image-key + --feishu-message-id
```

Use the JSON **`path`** field as **`--input`**.

**`--input` as a URL:** Quote the full URL in the shell (avoids `&` splitting). Defaults: 120 s read timeout, 100 MB max — override with `MT_AI_URL_READ_TIMEOUT`, `MT_AI_URL_CONNECT_TIMEOUT`, `MT_AI_URL_MAX_BYTES`. For flaky or large links, use `resolve-input --url` first and pass the local `path`.

If the user already gave a path or URL when triggering the skill, go to Step 2 without asking again.

**Reply immediately** to acknowledge the task, for example:

> "🖼️ Processing — please wait a moment…"

---

## Step 2 — Install dependencies

```bash
python3 {baseDir}/scripts/beautyplus_ai.py install-deps
```

If dependencies are already installed this step is quick; then continue to Step 3.

---

## Step 3 — Run the task

**Default:** All listed effect KEYs are image jobs — use **§3a** (`run-task`).

**Reserved:** If a future video effect key appears that the CLI treats as spawn-only, use **§3b** (`spawn-run-task` + `sessions_spawn`). No current keys hit this branch.

### 3a — Inline (blocking, default for all catalog effect KEYs)

Blocking call — use for all listed effect KEYs.

```bash
python3 {baseDir}/scripts/beautyplus_ai.py run-task \
  --task "<effect_key>" \
  --input "<image_url_or_path>"
```

Replace `<effect_key>` (e.g. `hair_soft_waves`) and `<image_url_or_path>` with real values. If the server returns `Unknown invoke preset`, the key is not in the tenant's invoke map — do not invent params; check config or admin.

Default params include `rsp_media_type: url`. For custom JSON params:

```bash
python3 {baseDir}/scripts/beautyplus_ai.py run-task \
  --task "<effect_key>" \
  --input "<url_or_path>" \
  --params '{"parameter":{"rsp_media_type":"url"}}'
```

**When `run-task` exits 0**, stdout is JSON that includes:

- **`skill_status: "completed"`** — job done; result is ready.
  - Single stage → proceed to Step 4.
  - Multi-stage pipeline → pass `primary_result_url` as `--input` for the next `--task`; deliver after the last stage.
  - Do **not** resubmit `run-task` for the same `task_id`; use `query-task` to resume polling.
- **`output_urls`** — ordered `http(s)` links (same extraction as before: `data.result.urls`, `images`, `media_info_list`, etc.).
- **`primary_result_url`** — same as `output_urls[0]` when present; convenient for delivery scripts.
- **`task_id`** — full task id as a top-level string when known (from `data.result.id` or the polling session). Keep it for manual status recovery or support handoff; do not truncate. Some synchronous completions may omit it if the API does not return an id.
- **`agent_instruction`** — short reminder for the model.
- **`meta` / `data`** — full API payload for debugging.

**On `skill_status: "completed"` you must:**
1. Send the user a short natural-language summary of what was done.
2. Complete Step 4 delivery using `primary_result_url` or `output_urls[0]` — unless the user explicitly asked for the URL only.

Do **not** end the turn with raw JSON alone.

**When `run-task` exits non-zero**, stdout has `skill_status: "failed"` — explain the error to the user; do not deliver or treat as success.

**Quota / consume failures** — when `failure_stage: "consume_quota"`:

| `error` | `api_code` | Action |
|---|---|---|
| `credit_required` | 60002 | Show server `detail` to the user; include `pricing_url` as a link if present. Do not retry by tweaking params. |
| `membership_required` | 60001 | Same — show `detail` / `pricing_url`. |
| `consume_param_error` | — | Fix `--task` / `--input` / `--params`; do not tell the user to recharge. |

Never dump raw JSON to the user. Never retry `run-task` for credit/membership errors.

**Video (reserved):** When §3b applies, see [docs/errors-and-polling.md](docs/errors-and-polling.md) and §3c–§3d for polling, timeouts, and recovery.

### 3b — Async worker (`sessions_spawn`, **reserved for video effect keys**)

> **Current catalog is image-only.** `spawn-run-task` is not used — CLI rejects it for image keys. Use §3a for all listed effect KEYs.

When video keys return, the main agent does not block; a sub-session runs `run-task` and delivers the result.

1. Build the payload (`<effect_key>` must be a **video** task name accepted by **`spawn-run-task`** — historically e.g. **`videoscreenclear`** / **`hdvideoallinone`** when server + CLI expose them):

```bash
python3 {baseDir}/scripts/beautyplus_ai.py spawn-run-task \
  --task "<effect_key>" \
  --input "<video_url_or_path>" \
  --deliver-to "<oc_xxx_or_ou_xxx_or_chat_id>" \
  --deliver-channel "feishu"
```

Optional: `--params '<json>'` (same as `run-task`), `--deliver-channel telegram|discord|...`, `--run-timeout-seconds` (default **3600**, aligned with extended poll budget). **Do not reduce** `runTimeoutSeconds` below the payload default unless you accept timeout risk — wall time varies (often minutes to tens of minutes).

2. Call OpenClaw **`sessions_spawn`** with the printed **`sessions_spawn_args`** (`task`, `label`, `runTimeoutSeconds`) **without reducing** `runTimeoutSeconds` unless you intentionally accept timeout risk.

3. Reply immediately to the user that processing has started. The sub-agent runs `install-deps` (if needed), `run-task`, then Step 4 per the embedded task text.

**Multi-stage + spawn:** One embed = one `run-task`. Image chains (current): §3a only. Video chains (reserved): see Multi-stage pipelines, rule 4.

### 3c — Resume polling (`query-task`)

When you already have a **full `task_id`** (from a previous stdout JSON, e.g. success, `poll_timeout`, or `poll_aborted`, or from stderr `task_id=...` lines) and the job may still be running on the server — **do not run `run-task` again** for that id; resume polling only:

```bash
python3 {baseDir}/scripts/beautyplus_ai.py query-task \
  --task-id "<full_task_id>"
```

Optional **`--task`** sets the `task_name` field in the success JSON for your logs (default labels as `query_task`). Uses the same **`BP_AK` / `BP_SK`** and remote config as the original submit. **Stdout JSON and exit codes** match **`run-task`**: exit **0** with `skill_status: "completed"` when the task finishes successfully; exit **non-zero** with `skill_status: "failed"` / `error` on timeout, query errors, or API-reported failure.

### 3d — Last task and history (user-visible)

Local state under **`~/.openclaw/workspace/beautyplus-ai/`** (`last_task.json`, `history/task_*.json`, last **50** records). For async **`run-task`**, **`last_task.json`** may briefly show **`skill_status`: `"polling"`** with **`task_id`** while the client is still polling (checkpoint so **`query-task`** can resume if the process is killed mid-poll):

```bash
python3 {baseDir}/scripts/beautyplus_ai.py last-task
python3 {baseDir}/scripts/beautyplus_ai.py history
```

Use when the user asks whether a recent job finished, or for a short history summary. Do not expose raw secrets.

---

## Step 4 — Deliver result to the channel

**Required after success:** When **`skill_status`** is **`completed`**, deliver here — the CLI does not post to IM by itself. Send the processed image or video back on the user’s platform (and keep the Step 3 **MANDATORY** summary in the same turn).

### Resolve deliver-to target

| Platform | Source | Format |
|---|---|---|
| Feishu group | `conversation_label` or `chat_id` without `chat:` prefix | `oc_xxx` |
| Feishu DM | `sender_id` without `user:` prefix | `ou_xxx` |
| Telegram | Inbound message `chat_id` | e.g. `-1001234567890` |
| Discord | `channel_id` | e.g. `123456789` |

### Feishu — image tasks

```bash
python3 {baseDir}/scripts/feishu_send_image.py \
  --image "<result_url>" \
  --to "<oc_xxx or ou_xxx>"
```

### Feishu — video tasks (reserved; e.g. legacy `videoscreenclear` / `hdvideoallinone` when video keys exist)

```bash
curl -sL -o /tmp/beautyplus_result.mp4 "<primary_result_url_or_output_urls[0]>"
python3 {baseDir}/scripts/feishu_send_video.py \
  --video /tmp/beautyplus_result.mp4 \
  --to "<oc_xxx or ou_xxx>" \
  --video-url "<primary_result_url_or_output_urls[0]>" \
  [--cover-url "<optional_thumb_url>"] \
  [--duration <milliseconds_if_known>]
```

`--video-url` adds a second message with the download link. Optional cover/duration; details: [docs/feishu-send-video.md](docs/feishu-send-video.md).

### Telegram — image tasks

```bash
TELEGRAM_BOT_TOKEN="$TELEGRAM_BOT_TOKEN" python3 {baseDir}/scripts/telegram_send_image.py \
  --image "<result_url>" \
  --to "<chat_id>" \
  --caption "✅ Done"
```

### Telegram — video tasks (reserved; long async video jobs)

```bash
curl -sL -o /tmp/beautyplus_result.mp4 "<primary_result_url_or_output_urls[0]>"
TELEGRAM_BOT_TOKEN="$TELEGRAM_BOT_TOKEN" python3 {baseDir}/scripts/telegram_send_video.py \
  --video /tmp/beautyplus_result.mp4 \
  --to "<chat_id>" \
  --video-url "<primary_result_url_or_output_urls[0]>" \
  [--cover-url "<optional_thumb_url>"] \
  [--duration <seconds>] \
  --caption "✅ Done"
```

`--video-url` sends a follow-up text message with the download link. Max ~**50 MB** for Bot API video; larger files rely on the link line.

### Discord

Download the result, then send with the `message` tool (use **`.mp4`** for video, **`.jpg`** / **`.png`** for image):

```bash
curl -L "<result_url>" -o /tmp/result_image.jpg
```

Then:

```
message(action="send", channel="discord", target="<channel_id>", filePath="/tmp/result_image.jpg")
```

For files over ~25MB, send the result URL as a link instead.

### WhatsApp / Signal / others

Use the `message` tool with `media`, or send the result URL directly.

---

## Quick commands reference (agent)

| Command | Description | User-facing? |
|---------|-------------|--------------|
| `preflight` | AK/SK ok / missing | No |
| `install-deps` | pip install requirements | No |
| `run-task` | Submit + poll until done | Indirectly |
| `query-task` | Resume poll by `task_id` | When recovering |
| `spawn-run-task` | Print `sessions_spawn` payload — **CLI video task names only** (reserved; none in current image catalog) | No |
| `resolve-input` | IM/URL → local path for `--input` | No |
| `last-task` | Last job JSON | Yes — “last job?” |
| `history` | Up to 50 recent records | Yes — “history?” |

---

## Notes

- **Single business entrypoint**: algorithm runs and config fetch go through `beautyplus_ai.py`; agents do not need to open `client.py` / `ai/api.py`. **Must not** bypass this with direct HTTP to AIGC/wapi for new jobs — see **[API submission path (MANDATORY)](#api-submission-path-mandatory)** above. **`query-task`** is the supported way to resume polling when a **`task_id`** is already known.
- **Video tasks (reserved):** When the CLI again accepts video-only **`--task`** values for **`spawn-run-task`**, use **`spawn-run-task` + `sessions_spawn`** in the main session; the worker runs **`run-task`** and delivery. **Today:** all catalog keys are **image** — use **`run-task`** (§3a) only; **`run-task`** in the main session is also for **recovery** (`query-task`). Polling and env tuning: [docs/errors-and-polling.md](docs/errors-and-polling.md).
- **AK/SK loading**: environment variables `BP_AK` / `BP_SK` first; if unset, `scripts/.env` is read automatically (same as `SkillClient`).
- **Client init** pulls the latest algorithm config from the server; no manual `INVOKE` setup.
- **Bot token safety**: pass `TELEGRAM_BOT_TOKEN` and similar only via environment variables — never as CLI arguments.
- **On failure**: stdout JSON has `skill_status: "failed"` / `error`, **exit code ≠ 0** — explain to the user; check AK/SK, network, quotas; timeouts / SIGKILL / no final JSON: **[docs/errors-and-polling.md](docs/errors-and-polling.md)**. URL input errors may mention **HTTP 403** (expired signed URL) or **timeout** — see **`MT_AI_URL_*`** env vars above.
- **More docs**: [README.md](README.md), [docs/multi-platform.md](docs/multi-platform.md), [docs/im-attachments.md](docs/im-attachments.md), [docs/feishu-send-video.md](docs/feishu-send-video.md).