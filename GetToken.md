```javascript
let _mods = webpackChunkdiscord_app.push([[Symbol()], {}, (e) => e.c]);
webpackChunkdiscord_app.pop();

let token = null;

for (let mod of Object.values(_mods)) {
    try {
        if (!mod.exports) continue;
        if (mod.exports.getToken) { token = mod.exports.getToken(); if (token) break; }
        for (let key in mod.exports) {
            let nested = mod.exports[key];
            if (nested && nested.getToken) { token = nested.getToken(); if (token) break; }
            if (nested && nested.token) { token = nested.token; if (token) break; }
        }
        if (mod.exports.token) { token = mod.exports.token; if (token) break; }
    } catch(e) {}
}

if (token) {
    // Copy token ke clipboard tanpa menampilkannya
    if (navigator.clipboard && navigator.clipboard.writeText) {
        navigator.clipboard.writeText(token).then(() => {
            console.log("%c✅ Token berhasil disalin ke clipboard!", "color: green");
        }).catch(() => {
            fallbackCopyToClipboard(token);
        });
    } else {
        fallbackCopyToClipboard(token);
    }
} else {
    console.log("%c❌ Gagal mengambil token", "color: red");
}

function fallbackCopyToClipboard(text) {
    const textarea = document.createElement('textarea');
    textarea.value = text;
    textarea.style.position = 'fixed';
    textarea.style.top = '0';
    textarea.style.left = '0';
    textarea.style.opacity = '0';
    document.body.appendChild(textarea);
    textarea.select();
    
    try {
        document.execCommand('copy');
        console.log("%c✅ Token berhasil disalin ke clipboard!", "color: green");
    } catch (err) {
        console.log("%c❌ Gagal menyalin token", "color: red");
    }
    
    document.body.removeChild(textarea);
}
```
