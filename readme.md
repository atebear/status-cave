# Status.Cave (`status-cave`)

A lightweight, self-hosted, single-user alternative to Status.cafe built entirely using **Hugo**, **Decap CMS**, and **Netlify Identity**. 

**Status.Cave** operates as a headless CMS. It generates a raw JSON feed of your status updates that you can easily fetch and embed seamlessly into any personal website or indie-web layout.

## 🛠️ The Architecture

1. **The Admin Interface:** Powered by Netlify Identity and Decap CMS. Logging into the site gives you a clean dashboard to write updates and set your mood.
2. **The Database:** Zero traditional databases. Every time you save a status, a flat `.md` file is committed directly to this GitHub repository.
3. **The Deployment:** Netlify automatically builds the project, rendering an updated `index.json` static file in seconds.

## 🏗️ Project Structure

```text
status-cave/
├── hugo.toml                 <-- Global settings & JSON output rules
├── content/
│   └── status/               <-- Your saved updates (.md files)
└── themes/
    └── status-cave-theme/    <-- The custom theme templates
        ├── theme.toml
        ├── layouts/
        │   ├── index.html    <-- Private login panel
        │   └── index.json    <-- The JSON API generation template
        └── static/
            └── admin/
                ├── index.html <-- Decap CMS panel
                └── config.yml <-- CMS structure layout
```

## 🌐 How to Embed into Your Main Indie Site

Because this project outputs a clean JSON file, you don't have to use clunky iframes. Use this vanilla JavaScript snippet on your main website to fetch and render your timeline natively:

```html
<!-- Place this container where you want your statuses to appear -->
<div id="status-cave-timeline">
  <p>Peering into the cave...</p>
</div>

<script>
  // Replace with your actual live Netlify URL
  const CAVE_URL = 'https://YOUR-NETLIFY-SITE.netlify.app/index.json';

  async function fetchStatuses() {
    try {
      const res = await fetch(CAVE_URL);
      const data = await res.json();
      const container = document.getElementById('status-cave-timeline');
      
      if(data.length === 0) {
        container.innerHTML = '<p>The cave is currently empty.</p>';
        return;
      }

      container.innerHTML = data.map(status => `
        <div class="status-item" style="margin-bottom: 1rem; border-left: 2px solid #555; padding-left: 8px;">
          <small style="color: #888;">${status.date} ${status.mood ? `— ${status.mood}` : ''}</small>
          <div class="status-text" style="margin-top: 4px;">${status.text}</div>
        </div>
      `).join('');
    } catch (err) {
      console.error(err);
      document.getElementById('status-cave-timeline').innerHTML = '<p>Failed to retrieve cave updates.</p>';
    }
  }
  fetchStatuses();
</script>
```

## 🚀 Deployment Checklist

1. Push this setup to your GitHub repository.
2. Link the repository to a new site on **Netlify**.
3. In your Netlify Site Settings:
* Go to **Identity** and click **Enable Identity**.
* Scroll down to **Services** -> **Git Gateway** and click **Enable Git Gateway** (this allows your login panel to securely commit files back to your GitHub repository).
4. Go to your live URL, log in via the dashboard widget, navigate to `/admin/`, and post away!
