# Spotify Recent Tracks - Clean Table

A simple browser tool that transforms the **StatsForSpotify** "Recent Tracks" page into a clean, minimal table without album covers.

## ✨ Features

- Displays only **Track**, **Artist(s)**, and **Played at**
- Clean white background with black text
- Minimalist design (no heavy borders)
- Overlay on top of the original page
- Easy one-click close button
- No installation required

## 📸 Preview

![warning](https://raw.githubusercontent.com/redmail99/statsforspotify-table/refs/heads/main/warning.png)

![Clean Spotify Recent Tracks Table](https://raw.githubusercontent.com/redmail99/statsforspotify-table/refs/heads/main/Preview.png)


## 🚀 How to Use

1. Go to:  
   [https://www.statsforspotify.com/track/recent](https://www.statsforspotify.com/track/recent)

2. Log in with your Spotify account.

3. Press **F12** to open Developer Tools and go to the **Console** tab.

4. **Allow pasting** (important):  
   When you try to paste the code, the browser may block it. Just type `allow pasting` in the console and press **Enter**.

5. Copy and paste the entire script below, then press **Enter**:

```javascript
// === CLEAN RECENT TRACKS TABLE ===
(function() {
    const items = document.querySelectorAll('div[class*="track-row"], div[class*="item"], div[class*="list-item"], li, tr');

    let data = [];

    items.forEach(item => {
        const trackEl = item.querySelector('h3, h4, strong, .title, [class*="title"], [class*="name"]');
        const artistEl = item.querySelector('p, span, .artist, [class*="artist"], small');
        const timeEl = item.querySelector('time, .time, [class*="time"], [class*="played"], [class*="date"]');

        const track = trackEl ? trackEl.textContent.trim() : '';
        const artist = artistEl ? artistEl.textContent.trim() : '';
        const playedAt = timeEl ? timeEl.textContent.trim() : '—';

        if (track && artist) {
            data.push({ Track: track, Artist: artist, 'Played At': playedAt });
        }
    });

    if (data.length === 0) {
        alert("No tracks found. Try inspecting a row and let me know if selectors need updating.");
        return;
    }

    const table = document.createElement('table');
    table.style = `
        width: 100%; 
        border-collapse: collapse; 
        margin: 20px 0; 
        font-family: Arial, sans-serif;
        background: white;
        color: #111;
    `;

    let html = `
        <thead>
            <tr style="background:#f8f8f8; border-bottom: 2px solid #ddd;">
                <th style="padding:14px 12px; text-align:left; font-weight:600;">Track</th>
                <th style="padding:14px 12px; text-align:left; font-weight:600;">Artist(s)</th>
                <th style="padding:14px 12px; text-align:left; font-weight:600;">Played at</th>
            </tr>
        </thead>
        <tbody>
    `;

    data.forEach(row => {
        html += `
            <tr>
                <td style="padding:14px 12px; border-bottom: 1px solid #eee;">${row.Track}</td>
                <td style="padding:14px 12px; border-bottom: 1px solid #eee;">${row.Artist}</td>
                <td style="padding:14px 12px; border-bottom: 1px solid #eee;">${row['Played At']}</td>
            </tr>
        `;
    });

    html += '</tbody></table>';

    const container = document.createElement('div');
    container.style = `
        position: fixed; 
        top: 20px; 
        left: 20px; 
        right: 20px; 
        max-height: 85vh; 
        overflow: auto; 
        z-index: 99999; 
        background: #ffffff; 
        padding: 20px; 
        border-radius: 8px;
        border: 1px solid #ddd;
        box-shadow: 0 4px 20px rgba(0,0,0,0.12);
    `;

    const title = document.createElement('h2');
    title.textContent = '📋 Recent Tracks - Clean Table';
    title.style = 'margin: 0 0 15px 0; color: #1a1a1a; font-size: 22px;';
    container.appendChild(title);
    container.appendChild(table);

    const closeBtn = document.createElement('button');
    closeBtn.textContent = '✕';
    closeBtn.style = `
        position: absolute; top: 15px; right: 15px; 
        padding: 6px 12px; background: #e74c3c; color: white; 
        border: none; border-radius: 4px; cursor: pointer; font-size: 16px;
    `;
    closeBtn.onclick = () => container.remove();
    container.appendChild(closeBtn);

    document.body.appendChild(container);

    console.log(`✅ Clean table created with ${data.length} tracks`);
})();

```

## ☕ Support

* If this tool was useful to you, you can support me here:
   [☕ Buy me a coffee on Ko-fi](https://ko-fi.com/redsj)
