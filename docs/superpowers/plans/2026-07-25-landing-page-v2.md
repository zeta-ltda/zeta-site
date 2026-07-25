# Zeta Landing Page v2 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Update `index.html` (the Zeta marketing site) with 5 futuristic-scenario panels, a pricing section, an expanded 7-platform download grid, and an updated nav — per `docs/superpowers/specs/2026-07-25-landing-page-v2-design.md`.

**Architecture:** Single static HTML file (`index.html`) with inline `<style>` — no build tools, no JS framework, no test runner. All changes are additive edits within this one file: new CSS blocks inside the existing `<style>` tag, new `<section>` markup inserted between existing sections (identified by their HTML comments, e.g. `<!-- VIDEO -->`, `<!-- TAGLINE -->`), plus edits to the existing nav and download grid markup.

**Tech Stack:** Plain HTML5 + CSS3 (custom properties, flexbox, CSS grid), no dependencies.

## Global Constraints

- No test framework exists in this repo. "Tests" for every task are manual visual verification: open `index.html` in the browser preview tool, check the rendered section at both desktop and mobile widths, and confirm it matches the spec copy exactly (Portuguese text is exact — copy it verbatim from the spec, don't paraphrase).
- Keep the existing dark theme CSS custom properties (`--bg`, `--surface`, `--surface-2`, `--border`, `--border-hover`, `--text`, `--text-muted`, `--accent`, `--accent-light`, `--teal`, `--pink`, `--orange`, `--green`) — do not introduce new color variables; reuse these.
- Follow the existing section pattern: `padding: 80px 0; border-top: 1px solid var(--border);` for new top-level sections, and reduce to `50px 0` inside the existing `@media (max-width: 768px)` block, matching how every other section already does it.
- All new copy is in Portuguese (pt-BR), matching the rest of the site.
- Price is displayed as "R$10/mês" with a small "≈ US$10" note — never convert or show USD as the primary figure.
- Icons for new platforms (Smartwatch, Carro) must be generic/original shapes, not traced brand logos (no Apple Watch or CarPlay logos) — per the spec's trademark-avoidance note.

---

## File Structure

Only one file is touched: `index.html`. Insertion points are anchored to existing HTML comments so line numbers don't need to be exact:

- Nav links: inside `<!-- NAV -->` → `.nav-links` block
- New scenarios section: inserted immediately after `<!-- VIDEO -->` section's closing `</section>`, immediately before `<!-- TAGLINE -->`
- New pricing section: inserted immediately after `<!-- FREE YOURSELF -->` section's closing `</section>`, immediately before `<!-- DOWNLOADS -->`
- Download grid: inside `<!-- DOWNLOADS -->` → `.download-grid` block, plus its mobile breakpoint rules

---

### Task 1: Update nav links

**Files:**
- Modify: `index.html` (`.nav-links` CSS is unchanged; only the `<nav>` markup under `<!-- NAV -->` changes)

**Interfaces:**
- Produces: anchor targets `#precos` (new, used by Task 3) and reuses existing `#download`/`#capacidades` ids — no other task depends on nav internals.

- [ ] **Step 1: Replace the nav links markup**

Find this block (currently right after `<!-- NAV -->`):

```html
<div class="nav-links">
    <a href="#capacidades">Capacidades</a>
    <a href="#agentes">Agentes</a>
    <a href="#download">Download</a>
</div>
```

Replace it with:

```html
<div class="nav-links">
    <a href="#capacidades">Capacidades</a>
    <a href="#download">Como instalar</a>
    <a href="#precos">Preços</a>
</div>
```

- [ ] **Step 2: Verify in browser**

Open `index.html` in the browser preview tool. Confirm the nav now reads "Capacidades · Como instalar · Preços" and that clicking each link smooth-scrolls to the right existing section (Capacidades and Download already exist; Preços will 404-scroll to nowhere until Task 3 adds the `#precos` section — that's expected at this point).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: update nav links for landing page v2 (Como instalar, Preços)"
```

---

### Task 2: Add the 5 futuristic-scenario panels section

**Files:**
- Modify: `index.html` (new CSS block inside `<style>`, new `<section>` markup after the video section)

**Interfaces:**
- Consumes: existing CSS custom properties (`--surface`, `--border`, `--border-hover`, `--text-muted`, `--accent-light`, `--accent`, `--teal`)
- Produces: `.scenarios` section with id `cenarios` (not linked from nav — purely a scroll-through section between video and tagline)

- [ ] **Step 1: Add the CSS block**

Insert this new CSS block right after the existing `.video-frame`/`.play-btn` rules (end of the `/* ── VIDEO ── */` block, before `/* ── TAGLINE SECTION ── */`):

```css
/* ── SCENARIOS ── */
.scenarios {
    padding: 80px 0; border-top: 1px solid var(--border);
}
.scenario-panel {
    display: flex; align-items: center; gap: 48px;
    margin-bottom: 64px;
}
.scenario-panel:last-child { margin-bottom: 0; }
.scenario-panel.reverse { flex-direction: row-reverse; }
.scenario-media, .scenario-text { flex: 1 1 50%; min-width: 0; }
.scenario-media-frame {
    aspect-ratio: 4/3; background: var(--surface); border: 1px solid var(--border);
    border-radius: 16px; display: flex; flex-direction: column; align-items: center;
    justify-content: center; text-align: center; padding: 28px; gap: 10px;
    color: var(--text-muted); transition: border-color 0.3s;
}
.scenario-media-frame:hover { border-color: var(--border-hover); }
.scenario-media-frame .media-type {
    font-size: 0.75rem; font-weight: 700; letter-spacing: 0.04em;
    text-transform: uppercase; color: var(--accent-light);
}
.scenario-media-frame p { font-size: 0.85rem; line-height: 1.5; max-width: 320px; }
.scenario-text h3 {
    font-size: clamp(1.4rem, 3.5vw, 1.9rem); font-weight: 800;
    letter-spacing: -0.02em; margin-bottom: 14px; line-height: 1.2;
}
.scenario-text p { color: var(--text-muted); font-size: 1rem; line-height: 1.6; }
```

- [ ] **Step 2: Add the same CSS block's mobile override**

Inside the existing `@media (max-width: 768px)` block, add these two lines alongside the other section padding reductions (find the line `.tagline-section, .capabilities, .agents, .free-section, .downloads, .video-section { padding: 50px 0; }` and add `.scenarios` to that same selector list, plus a new rule for stacking panels):

Change:
```css
.tagline-section, .capabilities, .agents, .free-section, .downloads, .video-section {
    padding: 50px 0;
}
```
To:
```css
.tagline-section, .capabilities, .agents, .free-section, .downloads, .video-section, .scenarios {
    padding: 50px 0;
}
.scenario-panel, .scenario-panel.reverse {
    flex-direction: column; gap: 24px; margin-bottom: 40px;
}
```

- [ ] **Step 3: Insert the section markup**

Insert this new `<section>` immediately after the `<!-- VIDEO -->` section's closing `</section>` tag, and before the `<!-- TAGLINE -->` comment:

```html
<!-- SCENARIOS -->
<section class="scenarios" id="cenarios">
    <div class="scenario-panel">
        <div class="scenario-media">
            <div class="scenario-media-frame">
                <span class="media-type">🎥 Vídeo · 15-20s</span>
                <p>Carro se aproximando de uma garagem residencial ao entardecer, tela do painel/celular mostra notificação "Zeta: quer que eu abra a garagem?" com confirmação por voz. Iluminação quente, sensação de chegar em casa.</p>
            </div>
        </div>
        <div class="scenario-text">
            <h3>O carro chega. O Zeta já pergunta.</h3>
            <p>O Zeta percebe que você está chegando perto de casa e já pergunta se quer abrir a garagem — sem precisar tocar em nada.</p>
        </div>
    </div>

    <div class="scenario-panel reverse">
        <div class="scenario-media">
            <div class="scenario-media-frame">
                <span class="media-type">📷 Imagem ou vídeo · 10s</span>
                <p>Pulso com smartwatch mostrando notificação "Cliente pediu: criar tela de agendamento" e resposta sendo dada por voz; fundo desfocado de rua ou ambiente externo. Transmitir mobilidade e produtividade em qualquer lugar.</p>
            </div>
        </div>
        <div class="scenario-text">
            <h3>Um cliente pede. Você resolve do pulso.</h3>
            <p>Enquanto você está longe do computador, um cliente pede uma funcionalidade nova — e o Zeta já te avisa e começa a codar, direto do seu relógio.</p>
        </div>
    </div>

    <div class="scenario-panel">
        <div class="scenario-media">
            <div class="scenario-media-frame">
                <span class="media-type">🎥 Vídeo · 10-15s</span>
                <p>Notebook fechado numa mesa à noite, timelapse até o amanhecer, tela acende mostrando resumo "Zeta concluiu 12 tarefas enquanto você dormia". Tom tranquilo, produtividade passiva.</p>
            </div>
        </div>
        <div class="scenario-text">
            <h3>Você dorme. O Zeta trabalha.</h3>
            <p>Enquanto você descansa, o Zeta resolve tarefas pendentes, responde mensagens e organiza sua agenda — e deixa um resumo pronto pra quando você acordar.</p>
        </div>
    </div>

    <div class="scenario-panel reverse">
        <div class="scenario-media">
            <div class="scenario-media-frame">
                <span class="media-type">🎥 Vídeo · 3 cortes, ~12s</span>
                <p>Cena 1: Luiza mexendo um risoto no fogão, pergunta em voz alta "Zeta, qual o próximo passo dessa receita?". Transição (whip-pan) pro andar de cima. Cena 2: alguém no quarto/escritório pergunta "Zeta, onde a Luiza tá?" e pede "manda avisar ela pra ver se tem sal". Transição de volta pra cozinha. Cena 3: Luiza recebe o recado, confere o armário, sorri e confirma "Tem sim!". Tom caseiro, acolhedor, luz quente.</p>
            </div>
        </div>
        <div class="scenario-text">
            <h3>Uma casa. Duas pessoas. Um Zeta cuidando de tudo.</h3>
            <p>Luiza cozinha e pergunta o próximo passo da receita. No andar de cima, alguém pede pro Zeta avisar ela sobre o sal. O Zeta conecta as duas pontas da casa, em tempo real.</p>
        </div>
    </div>

    <div class="scenario-panel">
        <div class="scenario-media">
            <div class="scenario-media-frame">
                <span class="media-type">🎥 Vídeo · 10-15s</span>
                <p>Pessoa andando na calçada, fone de ouvido, digitando no celular. Corta pra notificação recebida com foto do fogão desligado, câmera da cozinha ao fundo. Tom prático, tranquilizador.</p>
            </div>
        </div>
        <div class="scenario-text">
            <h3>Longe de casa, mas de olho em tudo.</h3>
            <p>Andando na rua, com o fone no ouvido, você manda uma mensagem perguntando se deixou o forno desligado — o Zeta confere pela câmera de casa e te manda a foto na hora.</p>
        </div>
    </div>
</section>
```

- [ ] **Step 4: Verify in browser**

Open `index.html` in the browser preview tool at desktop width. Confirm:
- 5 panels render, alternating media-left/text-right and media-right/text-left
- Each media placeholder shows the emoji + media type label, and the descriptive paragraph
- Headlines match the spec text exactly

Resize to mobile (375px width). Confirm all 5 panels stack vertically (media on top, text below), matching how other sections already collapse on mobile.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add 5 futuristic scenario panels to landing page"
```

---

### Task 3: Add the pricing section

**Files:**
- Modify: `index.html` (new CSS block inside `<style>`, new `<section>` markup between Free Yourself and Downloads)

**Interfaces:**
- Consumes: `--surface`, `--border`, `--green`, `--text-muted`, `--accent`, `--accent-light` custom properties; the existing `.check` SVG checkmark pattern from `.free-item .check` (reused visually for the benefits list)
- Produces: `#precos` section id, which Task 1's nav link `href="#precos"` now resolves to. `.pricing-cta` links to `#download` (existing anchor).

- [ ] **Step 1: Add the CSS block**

Insert this new CSS block right **before** the `/* ── DOWNLOADS ── */` comment (i.e., between the end of the `/* ── FREE YOURSELF ── */` rules and the `/* ── DOWNLOADS ── */` comment):

```css
/* ── PRICING ── */
.pricing {
    padding: 80px 0; border-top: 1px solid var(--border); text-align: center;
}
.pricing-card {
    max-width: 420px; margin: 0 auto; background: var(--surface);
    border: 1px solid var(--border); border-radius: 20px; padding: 48px 36px;
}
.pricing-badge {
    display: inline-block; padding: 6px 16px; border-radius: 100px;
    background: rgba(0, 184, 148, 0.12); border: 1px solid rgba(0, 184, 148, 0.3);
    color: var(--green); font-size: 0.8rem; font-weight: 700; margin-bottom: 20px;
}
.pricing-amount {
    font-size: 3rem; font-weight: 900; letter-spacing: -0.02em; line-height: 1;
}
.pricing-amount .period { font-size: 1rem; font-weight: 600; color: var(--text-muted); }
.pricing-usd-note { color: var(--text-muted); font-size: 0.85rem; margin-top: 8px; }
.pricing-cancel { color: var(--text-muted); font-size: 0.9rem; margin: 18px 0 28px; }
.pricing-benefits {
    list-style: none; text-align: left; display: flex; flex-direction: column;
    gap: 14px; margin-bottom: 32px;
}
.pricing-benefits li { display: flex; align-items: center; gap: 10px; font-size: 0.95rem; }
.pricing-benefits li .check {
    flex-shrink: 0; width: 24px; height: 24px; border-radius: 7px;
    background: rgba(0, 184, 148, 0.12); display: flex; align-items: center; justify-content: center;
}
.pricing-benefits li .check svg { stroke: var(--green); }
.pricing-cta {
    display: inline-flex; width: 100%; justify-content: center;
    padding: 14px 32px; border-radius: 12px; font-size: 1rem; font-weight: 600;
    background: linear-gradient(135deg, var(--accent), #8b7cf7); color: #fff;
    text-decoration: none; transition: transform 0.2s, box-shadow 0.2s;
}
.pricing-cta:hover { transform: translateY(-2px); box-shadow: 0 8px 32px var(--accent-glow); }
```

- [ ] **Step 2: Add the mobile override**

Inside the existing `@media (max-width: 768px)` block, add `.pricing` to the shared padding-reduction selector list (the same one edited in Task 2 Step 2):

```css
.tagline-section, .capabilities, .agents, .free-section, .downloads, .video-section, .scenarios, .pricing {
    padding: 50px 0;
}
```

- [ ] **Step 3: Insert the section markup**

Insert this immediately after the `<!-- FREE YOURSELF -->` section's closing `</section>` tag, and before the `<!-- DOWNLOADS -->` comment:

```html
<!-- PRICING -->
<section class="pricing" id="precos">
    <div class="section-header">
        <h2>Um plano. Sem complicação.</h2>
        <p>Acesso completo a todos os agentes do Zeta.</p>
    </div>

    <div class="pricing-card">
        <span class="pricing-badge">1º mês grátis</span>
        <div class="pricing-amount">R$10<span class="period">/mês</span></div>
        <div class="pricing-usd-note">≈ US$10</div>
        <p class="pricing-cancel">Cancele quando quiser, sem multa.</p>

        <ul class="pricing-benefits">
            <li>
                <span class="check"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke-width="3"><polyline points="20 6 9 17 4 12"/></svg></span>
                Todos os agentes especializados inclusos
            </li>
            <li>
                <span class="check"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke-width="3"><polyline points="20 6 9 17 4 12"/></svg></span>
                Uso ilimitado de tarefas
            </li>
            <li>
                <span class="check"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke-width="3"><polyline points="20 6 9 17 4 12"/></svg></span>
                Suporte prioritário
            </li>
        </ul>

        <a href="#download" class="pricing-cta">Começar grátis</a>
    </div>
</section>
```

- [ ] **Step 4: Verify in browser**

Open `index.html` in the browser preview tool. Confirm:
- Clicking "Preços" in the nav (from Task 1) now scrolls to this section
- The card shows "1º mês grátis" badge, "R$10/mês" large, "≈ US$10" small note below, "Cancele quando quiser, sem multa.", 3 benefits with green checkmarks, and a "Começar grátis" button
- Clicking "Começar grátis" scrolls to the Download section

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add pricing section to landing page"
```

---

### Task 4: Expand the download grid to 7 platforms (Smartwatch, Carro)

**Files:**
- Modify: `index.html` (`.download-grid` markup under `<!-- DOWNLOADS -->`, plus its mobile breakpoint CSS)

**Interfaces:**
- Consumes: existing `.download-card`, `.dl-icon`, `.platform-name`, `.file-info`, `.dl-btn` CSS classes (no changes needed to those rules)
- Produces: nothing consumed by later tasks — this is the last content task

- [ ] **Step 1: Add the two new "Em breve" cards**

Find the existing `.download-grid` block under `<!-- DOWNLOADS -->`. Insert these two new cards right after the existing iOS card (last card in the grid), before the closing `</div>` of `.download-grid`:

```html
<a href="#" class="download-card" style="opacity: 0.5; pointer-events: none;">
    <div class="dl-icon">
        <svg viewBox="0 0 24 24"><path d="M15 5V3a1 1 0 00-1-1h-4a1 1 0 00-1 1v2a5 5 0 00-2 4v6a5 5 0 002 4v2a1 1 0 001 1h4a1 1 0 001-1v-2a5 5 0 002-4V9a5 5 0 00-2-4zm-5-2h4v1.09c-.65-.15-1.32-.24-2-.24s-1.35.09-2 .24V3zm4 18h-4v-1.09c.65.15 1.32.24 2 .24s1.35-.09 2-.24V21zm2-6a4 4 0 01-4 4h0a4 4 0 01-4-4V9a4 4 0 014-4h0a4 4 0 014 4v6z"/></svg>
    </div>
    <span class="platform-name">Smartwatch</span>
    <span class="file-info">Em breve</span>
    <span class="dl-btn">Em breve</span>
</a>

<a href="#" class="download-card" style="opacity: 0.5; pointer-events: none;">
    <div class="dl-icon">
        <svg viewBox="0 0 24 24"><path d="M18.92 6.01C18.72 5.42 18.16 5 17.5 5h-11c-.66 0-1.21.42-1.42 1.01L3 12v8a1 1 0 001 1h1a1 1 0 001-1v-1h12v1a1 1 0 001 1h1a1 1 0 001-1v-8l-2.08-5.99zM6.5 16A1.5 1.5 0 118 14.5 1.5 1.5 0 016.5 16zm11 0a1.5 1.5 0 111.5-1.5 1.5 1.5 0 01-1.5 1.5zM5 11l1.5-4.5h11L19 11z"/></svg>
    </div>
    <span class="platform-name">Carro</span>
    <span class="file-info">Em breve</span>
    <span class="dl-btn">Em breve</span>
</a>
```

- [ ] **Step 2: Update the download-grid section header copy**

The existing header above the grid reads "Disponivel para todas as plataformas." — no change needed, it already generalizes correctly to 7 platforms. Skip if unchanged (verify only).

- [ ] **Step 3: Verify the mobile grid breakpoints still look right**

The existing rules are:
```css
@media (max-width: 768px) { .download-grid { grid-template-columns: 1fr 1fr; } }
@media (max-width: 420px) { .download-grid { grid-template-columns: 1fr 1fr; } }
```
With 7 cards this still produces a clean 2-column grid (4 rows, last row with 1 card) — no CSS change needed. Just confirm visually in the next step.

- [ ] **Step 4: Verify in browser**

Open `index.html` in the browser preview tool. Confirm:
- 7 cards render in the grid: macOS, Windows (Em breve), Linux (Em breve), Android, iOS (Em breve), Smartwatch (Em breve), Carro (Em breve)
- The two new cards have the dimmed "Em breve" styling matching Windows/Linux/iOS (0.5 opacity, no click)
- Resize to mobile (375px) and confirm the grid still wraps 2-per-row cleanly with the 7th card alone on its own row

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add smartwatch and carro to download grid"
```

---

### Task 5: Full-page visual verification pass

**Files:**
- None (verification only, no code changes expected unless a bug surfaces)

**Interfaces:**
- Consumes: the fully assembled `index.html` from Tasks 1-4

- [ ] **Step 1: Open the full page in the browser preview tool**

Load `index.html` and scroll through the entire page top to bottom at desktop width (≥1280px).

- [ ] **Step 2: Check the full section order**

Confirm the order matches the spec: Nav → Hero → App preview → Vídeo → 5 Scenario panels → Tagline → Capacidades → Agentes → Livre-se de tudo → Preços → Download (7 cards) → Footer.

- [ ] **Step 3: Check all 3 nav links**

Click "Capacidades", "Como instalar", "Preços" in the nav. Confirm each smooth-scrolls to the correct section (Capacidades card grid, Download grid, Pricing card respectively).

- [ ] **Step 4: Resize to mobile (375px) and re-check the whole page**

Confirm: nav collapses as before (existing `.nav-links { display: none; }` rule already handles this — no task added a nav-links override), all 5 scenario panels stack vertically, pricing card stays centered and readable, download grid stays 2-column with 7 cards.

- [ ] **Step 5: Fix any visual regressions found**

If any section overlaps, overflows, or breaks at mobile width, fix the specific CSS rule causing it (likely a missing `min-width: 0` on a flex child, or a missing padding override) and re-verify. Commit the fix separately if one was needed:

```bash
git add index.html
git commit -m "fix: visual regression from landing page v2 pass"
```

- [ ] **Step 6: Final commit (if no fixes were needed)**

If Step 5 required no changes, there's nothing to commit here — Task 4's commit is the last one. If Step 5 did require a fix, its commit above is the final one.

---

## Self-Review Notes

- **Spec coverage:** Nav (Task 1) ✓, 5 scenario panels with exact headline/body/media-placeholder copy (Task 2) ✓, pricing single-plan card with R$10/≈US$10/1st-month-free/cancel-anytime (Task 3) ✓, 7-platform download grid with generic non-brand icons for Smartwatch/Carro (Task 4) ✓, full visual/mobile verification (Task 5) ✓.
- **Placeholder scan:** No TBD/TODO — all copy text is final, verbatim from the spec. The "media placeholder" text itself is intentional final content (it's a literal instruction string shown on the page), not a plan placeholder.
- **Type consistency:** N/A (no functions/types — static markup only). Section id references are consistent: `#precos` defined in Task 3, consumed by Task 1's nav link and Task 3's own use elsewhere is none; `#download` already existed and is reused by Task 1's nav and Task 3's CTA.
- **Scope:** Single file, single cohesive feature (landing page v2) — no decomposition needed.
