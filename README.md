# Dashboard.css

A classless CSS framework for dashboards. Drop in `dashboard.css` and write plain semantic HTML — no class names, no IDs, no inline styles needed.

```html
<link rel="stylesheet" href="dashboard.css" />
```

Variants are controlled via `data-variant` attributes. The only toggle states are two CSS classes applied to `<body>` or `<nav>`: `.toggle-sidebar` and `.toggle-navbar`.

---

## Layout

The page structure is always: `<aside>` + `<main>`. The sidebar is fixed on the left; main fills the rest.

```html
<body>
  <aside>…</aside>
  <main>…</main>
</body>
```

---

## Sidebar

The sidebar is built from a brand link, a `<ul>` nav list, and an optional close `<button>`.

```html
<aside>
  <!-- Brand -->
  <a href="#">My App</a>

  <!-- Nav -->
  <ul>
    <li><small>Section label</small></li>
    <li><a href="#" aria-current>Home</a></li>
    <li><a href="#">Analytics</a></li>

    <!-- Item with subitems — hover to expand -->
    <li>
      <span>Settings</span>
      <ul>
        <li><a href="#">Profile</a></li>
        <li><a href="#">Billing</a></li>
      </ul>
    </li>
  </ul>

  <!-- Close button -->
  <button onclick="document.body.classList.toggle('toggle-sidebar')">✕</button>
</aside>
```

- Use `<small>` inside a `<li>` for section group labels.
- Use `<span>` instead of `<a>` for a parent item that has subitems.
- Add `aria-current` to the active link.
- Subitems reveal on hover via CSS — no JS needed.
- Toggle the sidebar by adding `.toggle-sidebar` to `<body>`.

---

## Navbar

```html
<nav>
  <!-- Logo / brand -->
  <a href="#">My App</a>

  <!-- Hamburger (mobile only) -->
  <button aria-label="Toggle menu"
    onclick="this.parentElement.classList.toggle('toggle-navbar')">☰</button>

  <!-- Links -->
  <ul>
    <li><a href="#">Home</a></li>

    <!-- Dropdown on hover -->
    <li>
      <a href="#">Products ▾</a>
      <ul>
        <li><a href="#">Analytics</a></li>
        <li><a href="#">Automation</a></li>
      </ul>
    </li>

    <!-- Last item becomes a CTA button -->
    <li><a href="#">Get started →</a></li>
  </ul>
</nav>
```

- The last `<li>` in the nav list is automatically styled as a primary CTA button.
- Dropdowns open on hover via CSS.
- On mobile the `<ul>` hides and the hamburger appears. Clicking it toggles `.toggle-navbar` on `<nav>`.

---

## Page header

Place a `<header>` as a direct child of `<main>` for the page title row.

```html
<main>
  <header>
    <h1>Overview</h1>
    <button aria-label="Toggle menu" onclick="this.parentElement.classList.toggle('toggle-navbar')">☰</button>
  </header>
</main>
```

---

## Stat cards

A `<div>` containing only `<article>` elements becomes a responsive auto-fill grid.

```html
<div>
  <article>
    <h3>Total Users</h3>
    <strong>24,521</strong>
    <small data-variant="success">↑ 12% this month</small>
  </article>

  <article>
    <h3>Churn Rate</h3>
    <strong>2.4%</strong>
    <small data-variant="danger">↑ 0.3% vs last month</small>
  </article>
</div>
```

`data-variant` on `<small>` accepts: `success`, `danger`, `warning`.

---

## Card / Article

A general-purpose card. Use `<header>`, `<section>`, and `<footer>` inside `<article>`.

```html
<article>
  <header>
    <h3>Card title</h3>
    <button type="reset">Action</button>
  </header>

  <section>
    <p>Card content goes here.</p>
  </section>

  <footer>
    <button>Save</button>
    <button type="reset">Cancel</button>
  </footer>
</article>
```

---

## Table

Wrap `<table>` in a `<section>`. Add a `<header>` for controls and a `<footer>` for pagination. The `<div>` wrapper makes the table scroll horizontally on small screens.

```html
<section>
  <!-- Controls -->
  <header>
    <h3>Users</h3>
    <input type="search" placeholder="Search…" oninput="
      const q = this.value.toLowerCase();
      this.closest('section').querySelectorAll('tbody tr').forEach(r =>
        r.hidden = !r.textContent.toLowerCase().includes(q)
      );
    " />
    <select onchange="
      const v = this.value;
      this.closest('section').querySelectorAll('tbody tr').forEach(r =>
        r.hidden = v ? r.querySelector('mark')?.textContent.trim() !== v : false
      );
    ">
      <option value="">All statuses</option>
      <option>Active</option>
      <option>Inactive</option>
    </select>
    <button>+ Add</button>
  </header>

  <!-- Scrollable wrapper -->
  <div>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Status</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Alice</td>
          <td><mark data-variant="success" data-shape="pill">Active</mark></td>
          <td>
            <button type="reset">Edit</button>
            <button data-variant="danger">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- Pagination -->
  <footer>
    <span>Showing 5 of 24</span>
    <nav>
      <button disabled>‹</button>
      <button aria-current onclick="
        this.closest('nav').querySelectorAll('button').forEach(b => b.removeAttribute('aria-current'));
        this.setAttribute('aria-current','true');
      ">1</button>
      <button onclick="
        this.closest('nav').querySelectorAll('button').forEach(b => b.removeAttribute('aria-current'));
        this.setAttribute('aria-current','true');
      ">2</button>
      <button>›</button>
    </nav>
  </footer>
</section>
```

- Buttons inside `<td>` are automatically smaller.
- Use `data-variant="danger"` on a `<button>` inside `<td>` for a destructive outline button.
- Active pagination button: add `aria-current`.

---

## Buttons

```html
<!-- Primary (default) -->
<button>Save</button>

<!-- Secondary / outlined -->
<button type="reset">Cancel</button>

<!-- Neutral / grey -->
<button type="button">Options</button>

<!-- Variants -->
<button data-variant="danger">Delete</button>
<button data-variant="success">Publish</button>
<button data-variant="ghost">Skip</button>

<!-- Disabled -->
<button disabled>Unavailable</button>
```

| Element | Style |
|---|---|
| `<button>` | Primary blue |
| `<button type="reset">` | Secondary outlined |
| `<button type="button">` | Neutral grey |
| `data-variant="danger"` | Red |
| `data-variant="success"` | Green |
| `data-variant="ghost"` | Transparent |

---

## Badges / Mark

```html
<!-- Default (warning yellow) -->
<mark>Pending</mark>

<!-- Variants -->
<mark data-variant="success">Active</mark>
<mark data-variant="danger">Inactive</mark>
<mark data-variant="warning">Pending</mark>
<mark data-variant="info">Info</mark>
<mark data-variant="accent">New</mark>

<!-- Pill shape -->
<mark data-variant="success" data-shape="pill">Active</mark>
```

---

## Form

```html
<form>
  <!-- Side-by-side fields: wrap labels in a div -->
  <div>
    <label>First name <input type="text" placeholder="Alice" /></label>
    <label>Last name  <input type="text" placeholder="Smith" /></label>
  </div>

  <label>Email <input type="email" placeholder="you@example.com" /></label>

  <label>Role
    <select>
      <option>Admin</option>
      <option>Editor</option>
    </select>
  </label>

  <label>Bio <textarea placeholder="Short bio…"></textarea></label>

  <!-- Grouped checkboxes / radios -->
  <fieldset>
    <legend>Notifications</legend>
    <label><input type="checkbox" checked /> Email alerts</label>
    <label><input type="checkbox" /> SMS alerts</label>
    <label><input type="radio" name="freq" checked /> Daily</label>
    <label><input type="radio" name="freq" /> Weekly</label>
  </fieldset>

  <label>Volume <input type="range" min="0" max="100" value="50" /></label>

  <label>Color <input type="color" value="#1d6bf3" /></label>

  <div>
    <button type="submit">Save</button>
    <button type="reset">Reset</button>
  </div>
</form>
```

- Wrap `<label>` elements in a `<div>` to place them side by side.
- Always nest the `<input>` inside its `<label>` — no `for`/`id` needed.
- For checkboxes and radios, nesting inside `<label>` makes the whole row clickable.

---

## Blockquote / Callout

```html
<!-- Default (info blue) -->
<blockquote>
  <p>This is a note.</p>
  <footer>— Source</footer>
</blockquote>

<!-- Variants -->
<blockquote data-variant="success">
  <p><strong>Success:</strong> Deployment complete.</p>
</blockquote>

<blockquote data-variant="warning">
  <p><strong>Warning:</strong> This cannot be undone.</p>
</blockquote>

<blockquote data-variant="danger">
  <p><strong>Error:</strong> Connection failed.</p>
</blockquote>
```

---

## Details / Accordion

```html
<details>
  <summary>Click to expand</summary>
  <p>Hidden content goes here.</p>
</details>

<!-- Open by default -->
<details open>
  <summary>Already open</summary>
  <p>This is visible on load.</p>
</details>
```

---

## Figure

```html
<figure>
  <img src="chart.png" alt="Monthly revenue chart" />
  <figcaption>Fig. 1 — Monthly revenue for Q1 2024.</figcaption>
</figure>
```

Works with `<img>`, `<svg>`, `<video>`, or `<iframe>`.

---

## Progress & Meter

```html
<!-- Progress bar -->
<label>Storage used
  <progress value="68" max="100"></progress>
</label>

<!-- Meter (color changes based on value vs low/high/optimum) -->
<label>Score
  <meter value="0.75" min="0" max="1" low="0.3" high="0.7" optimum="1"></meter>
</label>
```

The `<meter>` element turns green (optimum), yellow (suboptimum), or red (bad) automatically based on its `low`, `high`, and `optimum` attributes.

---

## Code

```html
<!-- Inline -->
<p>Use the <code>data-variant</code> attribute.</p>

<!-- Block -->
<pre><code>aside > ul li:hover > ul {
  max-height: 300px;
}</code></pre>
```

---

## Definition List

```html
<dl>
  <dt>Version</dt>
  <dd>1.0.0</dd>

  <dt>License</dt>
  <dd>MIT</dd>
</dl>
```

---

## Keyboard shortcut

```html
<p>Press <kbd>⌘</kbd> + <kbd>K</kbd> to open search.</p>
```

---

## Abbreviation

```html
<p><abbr title="Cascading Style Sheets">CSS</abbr> is awesome.</p>
```

Renders with a dotted underline and a help cursor showing the full title on hover.

---

## Sidebar toggle

Add or remove `.toggle-sidebar` on `<body>` to collapse/expand the sidebar.

```html
<button onclick="document.body.classList.toggle('toggle-sidebar')">
  Toggle sidebar
</button>
```

On mobile the sidebar starts collapsed. On desktop it starts open.

---

## CSS variables

Override any of these in your own stylesheet to theme the framework:

```css
:root {
  --bg:        #f7f7f5;   /* Page background */
  --surface:   #ffffff;   /* Card / sidebar background */
  --border:    #e4e4e0;   /* Borders */
  --text:      #1a1a18;   /* Primary text */
  --muted:     #888880;   /* Secondary text */

  --accent:       #1d6bf3; /* Primary colour */
  --accent-lt:    #eef3fd; /* Light accent tint */
  --accent-hover: #1559d4; /* Hover state */

  --success:  #2da44e;  --success-lt: #dcfce7;
  --warning:  #d97706;  --warning-lt: #fef3c7;
  --danger:   #e03e3e;  --danger-lt:  #fee2e2;
  --info:     #0891b2;  --info-lt:    #e0f2fe;

  --sidebar-width: 260px;
  --nav-height:    56px;
  --gap:           1.25rem;
  --radius:        6px;
}
```