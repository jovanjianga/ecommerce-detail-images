# Ecommerce Detail Images

[中文](README.md) | **English**

Turn product photos or a written description into a consistent set of ecommerce product detail images.

`ecommerce-detail-images` is a Codex Skill. It analyzes the product, defines a shared visual style, plans the pages, writes complete prompts, calls an image tool, and checks and saves the results. You can request a warm minimalist look, a dark technical style, editorial layouts, or a visual direction based on your own reference.

**This is a workflow Skill, not a standalone application or an image model. Rendering requires an image generation/editing tool in the host environment.**

## Example results

This insulated-tumbler example was generated from a real product reference: 8 separate images, a 3:4 portrait layout, Chinese copy, and a warm cream minimalist style. Four of the images are shown below.

| Hero | Lid detail |
|---|---|
| ![Cream insulated tumbler hero](examples/thermos-cup/01-hero.png) | ![Transparent tumbler lid detail](examples/thermos-cup/02-lid-detail.png) |

| Detail overview | Lifestyle |
|---|---|
| ![Tumbler detail overview](examples/thermos-cup/07-detail-overview.png) | ![Tumbler on a desk](examples/thermos-cup/08-lifestyle.png) |

This run produced 8 separate PNG files at 1086 × 1448 pixels. That is the observed output of this run, not a guaranteed resolution in every environment. The images followed the supplied product appearance without adding unsupported capacity, insulation-duration, or material-grade claims.

## What it does

- **Photo input:** use product photos as the source of truth for appearance.
- **Text input:** establish a consistent concept appearance from a description, then reuse it across the set.
- **Style control:** specify colors, backgrounds, typography, lighting, layout, copy density, or style references.
- **Full sets:** organize hero, benefits, lifestyle, detail, specification, and informational pages around the information available.
- **Page-level edits:** revise selected images without regenerating the rest; preserve earlier versions.
- **Flexible workflows:** generate the full set, start with a sample, produce prompts only, or resume unfinished work.
- **Fact constraints:** do not invent specifications, certifications, performance claims, or material properties that a photo cannot establish.

## Requirements

| Requirement | Details |
|---|---|
| A host with Skill support | Primarily designed for Codex; the example was tested in a Codex desktop environment with a built-in image tool |
| An image generation/editing tool | Uses built-in `image_gen` by default; installing this Skill does not enable that tool for your account |
| Product input | At least one product photo, or a sufficiently clear description |
| File access | Local delivery requires access to the reference files and a writable output directory |
| Git or a ZIP extractor | Only required for the corresponding manual installation method |

The built-in rendering path does not require a separate API key. Availability, quotas, and speed depend on the host. This repository does not provide image credits, prescribe a subscription, or include a standalone API client. Without an image tool, it can produce plans and prompts, but those files are not rendered images.

The core workflow instructions are currently written in Chinese. You can make requests in Chinese or English and choose the language of the image copy. A full English-copy set has not yet been separately visually tested. Other tools that understand `SKILL.md` may read the instructions, but their image tools and reference-image APIs need adaptation; installation alone does not establish rendering compatibility.

## Installation

Choose one method. Avoid installing duplicate copies of this Skill in multiple discovery directories.

### Option 1: ask Codex to install it

If your Codex environment includes `$skill-installer`, send this in chat:

```text
Use $skill-installer to install https://github.com/jovanjianga/ecommerce-detail-images.
The Skill is at the repository root: repo path ".", install name ecommerce-detail-images.
```

The corresponding installer arguments are `--repo jovanjianga/ecommerce-detail-images --path . --name ecommerce-detail-images`. These are installer arguments, not a standalone shell command.

### Option 2: install with Git

**macOS / Linux:**

```bash
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/jovanjianga/ecommerce-detail-images.git \
  "$HOME/.agents/skills/ecommerce-detail-images"
```

**Windows PowerShell:**

```powershell
$ecomSkillRoot = Join-Path $env:USERPROFILE ".agents\skills"
New-Item -ItemType Directory -Force -Path $ecomSkillRoot | Out-Null
git clone https://github.com/jovanjianga/ecommerce-detail-images.git (Join-Path $ecomSkillRoot "ecommerce-detail-images")
```

If the destination already exists, do not overwrite customized files. Follow the update instructions below.

### Option 3: download a ZIP

1. Select **Code → Download ZIP** on GitHub.
2. Extract the archive and rename its root directory to `ecommerce-detail-images`.
3. Move the entire directory into `~/.agents/skills/`, or `%USERPROFILE%\.agents\skills\` on Windows.
4. Verify that `~/.agents/skills/ecommerce-detail-images/SKILL.md` exists without an extra nested repository directory.
5. Keep `references/` and `agents/`; do not copy only `SKILL.md`.

Some older installers use `$CODEX_HOME/skills`, normally `~/.codex/skills`. The local example used that compatibility location. Do not install a duplicate if you already have a working copy there. For new manual installations, this README uses the `~/.agents/skills` location listed in the [official Codex documentation](https://learn.chatgpt.com/docs/build-skills#where-to-save-skills).

### Verify installation

In your next message, type `$ecommerce-detail-images`, or find it in your client's Skill picker. Codex CLI/IDE users can inspect `/skills`. Codex detects Skill changes automatically; restart the client if it does not appear.

Discovery confirms that the instructions are installed. Actual rendering still requires an image tool in the current session.

## First run: four steps

1. **Open the workspace where you want the results saved**, or name an output directory in your request.
2. **Attach product photos.** One front view is enough to start. Additional angles provide stronger evidence for details. A written description also works.
3. **Describe the job:** product, image count, style, aspect ratio, and copy language. Omitted options use defaults.
4. **Let the Skill plan, generate, and inspect the images.** It does not wait for approval on every page unless you request a sample-first workflow.

A complete starting request:

```text
Use $ecommerce-detail-images to create 8 detail images from the attached product photo.
The product is an insulated tumbler. Use a warm cream minimalist style,
a 3:4 portrait layout, and English copy.
Focus on the lid, handle, and body. Generate the full set.
Do not add unspecified capacity, insulation-duration, or material claims.
```

When attachments have different roles, identify them: “Images 1 and 2 show the same product from different angles; image 3 is style reference only; image 4 is the brand logo.”

## What you can customize

These are natural-language options, not CLI flags. You do not need to complete a fixed form.

| Option | Examples | Default |
|---|---|---|
| Count | 4, 6, 8, 10 images, etc. | 10 images |
| Aspect ratio | 3:4, 9:16, 1:1, etc. | 9:16 portrait |
| Style | Warm minimal, dark technical, editorial, colorful, traditional-inspired | Clear, restrained ecommerce visuals appropriate to the product |
| Colors | Brand palette, HEX values, background and accent colors | Brand colors first, then product colors |
| Layout | Large headlines, left alignment, centered composition, information cards | One main message per image, clear hierarchy |
| Typography | Editorial serif, modern sans-serif, restrained type | Matches the chosen style |
| Copy language | English, Chinese, bilingual | Matches the request language |
| Page contents | Exact page order or a required number of detail shots | Adapted to the available product information |
| Branding | Brand name, supplied logo, placement rules | Omitted if not supplied; no invented brand |
| Workflow | Full set, sample first, prompts only | Full set |
| Output location | A local directory | Workspace `output/ecommerce-detail-images/` |
| Edit scope | One page, a text change, or a new style for the set | The requested scope |

Reference images and descriptions do not guarantee pixel-perfect reproduction. A typography request does not produce editable font layers. Exact resolution depends on the image tool; the Skill checks the output instead of treating an aspect ratio as a fixed pixel size.

## 10 usage examples

### 1. Generate a complete set from photos

Attach one or more photos of the same tumbler.

```text
Use $ecommerce-detail-images to create 8 product detail images from these tumbler photos.
Use a cream minimalist style, 3:4 portrait, and English copy.
Include 1 hero, 5 detail images, 1 overview, and 1 desk scene.
Keep the cup shape, lid, handle, and color. Do not add a logo, straw, or unsupported specifications.
```

### 2. Start with a description only

```text
Use $ecommerce-detail-images to create 6 detail images for a mug.
It is a matte pale-green ceramic mug with a cylindrical body and a C-shaped handle.
There is no brand or printed pattern.
Use a warm off-white minimalist style, 3:4, and English copy.
Establish one consistent product appearance first.
I have not supplied capacity or dimensions, so omit those claims.
```

This mode creates a concept based on the description. It cannot guarantee a match to an unseen physical product.

### 3. Borrow a visual style from another image

Attach the product as image 1 and a style reference as image 2.

```text
Use $ecommerce-detail-images to create 6 detail images for the shoes in image 1.
Use only the dark background, side lighting, and large-headline layout from image 2.
Keep the shoe shape, laces, sole, and colors from image 1.
Do not copy the reference's product, logo, or text.
Use 4:5 portrait and English copy. Focus on visible upper and sole details.
```

### 4. Use your logo and exact copy

Attach the product and logo files.

```text
Use $ecommerce-detail-images to make 6 detail images for this canvas tote.
Use the supplied logo, small and fixed in the top-left.
Background: #F5F1E8. Text: #38342E.
The hero headline must read “Carry the everyday”.
The subtitle must read “Minimal canvas tote”.
Use English, 3:4, and generous whitespace.
Base the remaining pages on the bag shape, handles, and printed design.
```

### 5. Specify the job of every page

```text
Use $ecommerce-detail-images to make 6 detail images from the product photo, in this order:
1. Full-product hero
2. Lid close-up
3. Handle close-up
4. Body surface
5. Lower-body shape
6. Desk lifestyle scene
Use one cream minimalist style, 3:4, and English copy.
Do not include a specification table or purchase buttons.
```

### 6. Review one sample before continuing

```text
Use $ecommerce-detail-images. I eventually need 8 tumbler detail images.
For now, generate only the first hero image in a cream minimalist style,
3:4 portrait, with English copy. Wait for my feedback before generating the other 7.
```

Then continue:

```text
Keep this color palette, typography, and lighting, and generate the remaining 7 images.
Continue to use the original product photo as the source of truth for product appearance.
```

### 7. Produce plans and prompts only

```text
Use $ecommerce-detail-images to plan 8 detail images for the attached product.
Only produce the page plan, exact English copy, and complete image-generation prompts.
Do not call an image tool.
Each prompt must stand alone and specify the product constraints, background, lighting, and layout.
```

### 8. Edit one page and keep the others

Continue in the original conversation. In a new one, supply the output directory or the image to edit.

```text
Continue with $ecommerce-detail-images. Edit only image 3:
change the headline to “A handle that matches”, reduce the text size,
and increase the whitespace on the left.
Keep the product and background unchanged.
Do not regenerate the other 7 images. Save a new version.
```

### 9. Chinese or bilingual image copy

```text
Use $ecommerce-detail-images to create 6 detail images from the product photos.
Use a warm minimalist style and 3:4 portrait.
All image copy must be in Simplified Chinese, with no English.
Only describe visible features and the product facts I supplied.
```

For bilingual copy:

```text
Use $ecommerce-detail-images to create 6 bilingual detail images.
Use a Chinese headline and a short English subtitle.
Communicate one main point per image and keep the typography spacious.
```

### 10. Resume an interrupted job

```text
Use $ecommerce-detail-images to continue the unfinished job in the output directory.
Read plan.md, prompts.md, and delivery.md, and check which images actually exist.
Generate only missing pages or pages marked as needing correction.
Keep images that already passed inspection.
```

## Reusable request template

Fill in what you know and remove unused lines:

```text
Use $ecommerce-detail-images to create product detail images.

Product:
Role of each reference image:
Verified benefits and specifications:
Brand / logo:
Number of images:
Aspect ratio:
Style / reference direction:
Palette / background / typography:
Copy language:
Exact required text:
Page contents or priority details:
Product features to preserve:
Elements to avoid:
Full set / sample first / prompts only:
Output directory:
```

## Deliverables

A typical output directory looks like this. Actual extensions follow the image tool's output format.

```text
output/ecommerce-detail-images/product-slug-run-id/
├── plan.md        # Product facts, visual rules, page plan
├── prompts.md     # Complete standalone prompt for each image
├── delivery.md    # Files, inspection status, unresolved issues
└── images/
    ├── 01-hero.png
    ├── 02-detail.png
    ├── ...
    └── 03-detail-v2.png  # An edited version, when applicable
```

Separate image files are the default. Ask explicitly if you want a ZIP. Long-image stitching, store uploads, product publication, and automatic paid-API setup are outside the default workflow. If the host returns only inline media without local files, the Skill shows the actual result and reports that limitation rather than inventing a download path.

## FAQ

**Why does the Skill appear installed but not generate images?**

Check whether the session provides an image generation tool. Skill instructions cannot install a model or grant tool access. Without one, use the prompts-only workflow.

**Is one product photo enough?**

Yes. The page plan will focus on visible features. Unseen internal structures and rear details are not treated as verified facts. Supply more photos when additional angles must be accurate.

**How does it keep the product consistent?**

Every page uses the original product reference, while an inspected hero image guides the shared visual style. This reduces drift but does not eliminate the need to inspect shape, color, branding, and text.

**Will it invent capacity, insulation duration, or certifications?**

No. Supply reliable information if you want those included. Otherwise, visual or lifestyle pages can replace a specification page.

**What if the text is wrong or a detail changes?**

Identify the page and the problem. The Skill uses image editing for a targeted correction and preserves the earlier version. Unresolved pages remain marked as needing correction rather than being reported as finished.

**Do I need to approve every image?**

No. It waits at a sample or planning stage only when you ask it to.

**Can I create sets for several products?**

Yes: group the inputs clearly and request separate references and output directories for each product. Do not mix different models into one consistency specification. Start with one product to establish your preferred style.

**Does it cost money? Do I need an API key?**

The Skill is provided under the MIT License. Rendering may consume the host service's credits or usage allowance. The built-in path needs no separate API key, and tool failure does not automatically authorize a switch to a paid API.

**Can it publish directly to my store?**

The default deliverable is image files, not a published listing. Check product fidelity, copy, and your platform's dimensions before publishing; request any needed adjustments explicitly.

## Updates and removal

For a Git installation with no uncommitted custom changes:

```bash
git -C "$HOME/.agents/skills/ecommerce-detail-images" pull --ff-only
```

PowerShell:

```powershell
git -C (Join-Path $env:USERPROFILE ".agents\skills\ecommerce-detail-images") pull --ff-only
```

If Git reports local changes or divergent branches, preserve your changes first; do not force-reset them. ZIP or installer-based copies may not contain `.git`. Back them up outside Skill discovery directories and update using the original installation method. Do not leave a backup with the same Skill name inside a scanned directory.

To remove the Skill, move the entire `ecommerce-detail-images` folder outside the Skill directory, or delete the installation copy you no longer need. Generated project files remain in their original workspace. Restart Codex if its list does not refresh.

## Repository layout

```text
ecommerce-detail-images/
├── README.md
├── README.en.md
├── LICENSE
├── SKILL.md
├── agents/openai.yaml
├── references/
│   ├── style-guide.md
│   ├── prompt-patterns.md
│   └── generation.md
└── examples/thermos-cup/   # Four tested images used in the README
```

## Feedback and license

Report installation, reference-fidelity, or prompting problems in [Issues](https://github.com/jovanjianga/ecommerce-detail-images/issues). Include your environment, a minimal request, expected behavior, and actual results. Share only reference images that are appropriate for a public issue.

Released under the [MIT License](LICENSE). The repository license does not replace third-party trademark, font, uploaded-asset, or image-service terms.

The workflow was inspired by [Muyang's post about ecommerce image prompts](https://x.com/yyyole/status/2002724036355240392). This project expands the idea into a configurable Skill with inspection and page-level edits; it does not reproduce the post's complete prompt set.
