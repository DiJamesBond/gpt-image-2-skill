# gpt-image-2-skill

Generate or edit images with OpenAI's GPT Image 2 — best-in-class for fine typography — via Fal AI.

A Claude Code skill that wires GPT Image 2 (ChatGPT Images 2.0) into your agent in one drop-in file. Two endpoints: text-to-image AND image edit (with optional masks). Sync API, no polling, single POST.

Why this model
GPT Image 2 is currently the best public model for legible text inside images. Use it when the picture is the typography:

Chalkboards, store signs, hand-lettered posters
Menu boards, packaging mockups, magazine covers
UI screenshot / app store mockups
Social cards, banners, multilingual designs
For photoreal portraits or strict style preservation, use a different model (Nano Banana, Flux). For everything text-heavy, this wins.

Quick Start
1. Install the skill
Copy the SKILL.md file to your Claude Code project's .claude/skills/gpt-image-2/ directory.

2. Add your Fal AI key
echo "FAL_KEY=your-fal-ai-key" >> .env
Get a key at fal.ai/dashboard/keys.

3. Generate
Tell Claude Code:

Generate a vintage diner chalkboard reading "TODAY SPECIAL — Lobster Roll $24"
Or for an edit:

Edit https://example.com/photo.png — same scene but everyone is on their phones now
Two endpoints
Endpoint	When	Required input
openai/gpt-image-2	Generate from a prompt	prompt
openai/gpt-image-2/edit	Modify an existing image	prompt + image_urls (+ optional mask_url)
The skill ships with one combined helper in Node, Python, and bash — pass image_urls to use the edit endpoint, omit for text-to-image.

What You Get
Sync API — single POST returns the image URL. No polling, no taskId.
Up to 4 images per call for picking from variants.
Custom dimensions up to 3840×2160 (aspect ratio ≤ 3:1, multiples of 16).
Image edit with optional masks for surgical region edits.
Combined helper in Node, Python, and bash — same shape across languages.
Defaults
Param	Default	Why
quality	medium	Best cost/quality sweet spot (~$0.05/image). High is ~3.5x more — reserve for finals where typography really matters.
image_size	landscape_4_3 (T2I) / auto (Edit)	Sensible defaults, override per call.
num_images	1	Bump to 4 for variants.
output_format	png	
Prompting patterns
The community has converged on two patterns the skill documents:

Structured JSON prompts for multi-element scenes (posters, infographics, comics, app mockups). Pass the prompt as a JSON object with explicit slots (type, subject, style, background, header, layout). Notably better than prose for complex layouts.
Raycast-style placeholders — {argument name="quote" default="Stay hungry, stay foolish"} — for parameterizable prompt templates.
The skill also points at the awesome-gpt-image-2 community library (700+ curated prompts, CC BY 4.0) — when a user asks "how would I prompt this?" Claude can fetch the README, grep the relevant category, and adapt an example.

Key Rules
Default quality: medium — high is ~3.5x the cost. Use medium for iteration, high for finals only.
Auth header is Key, not Bearer — Fal-specific. The skill notes this, but if you're hand-rolling: don't burn an hour like I did.
Reference images need public URLs — Fal storage, prior Fal output URL, or any external host. The skill explains how.
No real transparent backgrounds — Fal's wrapper doesn't expose OpenAI's background: "transparent" param. Asking the prompt for "transparent bg" gets you a fake checkerboard baked into RGB pixels. Chain fal-ai/imageutils/rembg on the output if you need real alpha.
Edit endpoint preserves more when you describe the change, not the whole scene. "Same workers, but on phones now" beats re-describing everything.
Pricing
Token-based, billed via Fal:

Quality	Rough cost (1536×1024)
low	~$0.02 / image
medium	~$0.05 / image ← default
high	~$0.18 / image
Quality is the biggest cost lever. Check actual usage in fal.ai/dashboard.

Credits
Community prompt library: YouMind OpenLab — awesome-gpt-image-2 (CC BY 4.0). When you reuse a prompt verbatim, credit the original author named in each prompt's Details section.
Hosted via Fal AI.
Model by OpenAI.
