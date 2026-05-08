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

3. **Open the Console** (one of these ways):  
   - Press **F12** on your keyboard, then go to the **Console** tab.  
   - Or right-click anywhere on the page → click **Inspect** → go to the **Console** tab.
     
4. **Allow pasting** (important):  
   When you try to paste the code, the browser may block it. Just type `allow pasting` in the console and press **Enter**.

5. Copy and paste the entire script below, then press **Enter**:

```javascript
// === Simple table ===
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
        alert("No songs.");
        return;
    }

    const table = document.createElement('table');
    table.style = `
        width: 80%; 
        border-collapse: collapse; 
        margin: 20px 0; 
        font-family: Arial, sans-serif;
        background: #ffffff;
        color: #000000;
    `;

    let html = `
        <thead>
            <tr style="background:#ffffff;">
                <th style="padding:12px; text-align:left;">Track</th>
                <th style="padding:12px; text-align:left;">Artist(s)</th>
                <th style="padding:12px; text-align:left;">Played at</th>
            </tr>
        </thead>
        <tbody>
    `;

    data.forEach(row => {
        html += `
            <tr">
                <td style="padding:10px 12px;">${row.Track}</td>
                <td style="padding:10px 12px;">${row.Artist}</td>
                <td style="padding:10px 12px;">${row['Played At']}</td>
            </tr>
        `;
    });

    html += '</tbody></table>';

    table.innerHTML = html;

    const container = document.createElement('div');
    container.style = `
        position: fixed; 
        width: 90%; 
        top: 20px; 
        left: 20px; 
        right: 20px; 
        max-height: 100%; 
        overflow: auto; 
        z-index: 99999; 
        background: #ffffff; 
        padding: 15px; 
        border-radius: 12px;
        border: 2px solid;
    `;

    const title = document.createElement('h2');
    title.textContent = '📋 Song list';
    title.style = 'margin: 0 0 15px 0; color: #000000;';
    container.appendChild(title);
    container.appendChild(table);

    const closeBtn = document.createElement('button');
    closeBtn.textContent = '✕ Close';
    closeBtn.style = `
        position: absolute; top: 10px; right: 15px; 
        padding: 8px 14px; background: #f00; color: white; 
        border: none; border-radius: 6px; cursor: pointer;
    `;
    closeBtn.onclick = () => container.remove();
    container.appendChild(closeBtn);

    document.body.appendChild(container);

    console.log(`✅ Table data: ${data.length} songs`);
})();

```

## ☕ Support

* If this tool was useful to you, you can support me here:
   [☕ Buy me a coffee on Ko-fi](https://ko-fi.com/redsj)
