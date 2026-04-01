# 🎮 Discord Hypesquad Switcher / Get new badges tag

Hai! Ini adalah dua script sederhana buat kalian yang mau ganti-ganti / yang baru mau dapetin **Hypesquad** di Discord secara instant. Bahkan ketika eventnya sudah habis, langsung jalan di console browser!
---

## ✨ Fitur

| Script | Fungsi |
|--------|--------|
| **Code 1** | 🔵 Bergabung ke **Hypesquad Bravery** (house_id: 1) |
| **Code 2** | ❌ Keluar dari **Hypesquad** (leave) |

---

## 🎨 Warna Hypesquad

```
🏠 house_id: 1  →  Bravery     (biru)   🔵
🏠 house_id: 2  →  Brilliance  (kuning) 🟡
🏠 house_id: 3  →  Balance     (hijau)  🟢
```

> **Catatan:** Kode di atas hanya untuk **Bravery** (ID 1). Kalau mau ganti ke yang lain, tinggal ubah angka `house_id` di code 1 ya!

---

## 🚀 Cara Penggunaan

1. Buka **Discord** (versi web atau desktop)
2. Tekan `F12` atau `Ctrl + Shift + I` buka **Developer Tools**
3. Masuk ke tab **Console**
4. **Copy-paste** salah satu script di bawah, lalu tekan `Enter`

---

## 📋 Script 1 – Gabung Hypesquad Bravery

```javascript
// Ambil token dengan cara yang lebih tepat
let _mods = webpackChunkdiscord_app.push([[Symbol()], {}, (e) => e.c]);
webpackChunkdiscord_app.pop();

let token = null;

// Cari token dari berbagai kemungkinan module
for (let mod of Object.values(_mods)) {
    try {
        if (!mod.exports) continue;
        
        // Cara 1: Langsung dari exports
        if (mod.exports.getToken) {
            token = mod.exports.getToken();
            if (token) break;
        }
        
        // Cara 2: Dari nested exports
        for (let key in mod.exports) {
            let nested = mod.exports[key];
            if (nested && nested.getToken) {
                token = nested.getToken();
                if (token) break;
            }
            // Cara 3: Cari property yang nyimpen token
            if (nested && nested.token) {
                token = nested.token;
                if (token) break;
            }
        }
        
        // Cara 4: Cari property token langsung
        if (mod.exports.token) {
            token = mod.exports.token;
            if (token) break;
        }
    } catch(e) {}
}

if (token && typeof token === 'string' && token.length > 50) {
    console.log("%c✅ Token berhasil diambil! (panjang: " + token.length + ")", "color: green");
    
    // Gabung Hypesquad
    fetch("https://discord.com/api/v9/hypesquad/online", {
        method: "POST",
        headers: {
            "Authorization": token,
            "Content-Type": "application/json"
        },
        body: JSON.stringify({house_id: 1}) // 1=Bravery🔵 2=Brilliance🟡 3=Balance🟢
    })
    .then(async res => {
        if (res.ok) {
            console.log("%c✅✅✅ BERHASIL GABUNG HYPESQUAD! ✅✅✅", "color: green; font-size: 16px; font-weight: bold");
            console.log("%c🎉 Badge akan muncul dalam beberapa detik!", "color: cyan");
            setTimeout(() => location.reload(), 1500);
        } else {
            let text = await res.text();
            console.log("%c❌ Gagal! Status: " + res.status, "color: red");
            console.log("Response:", text);
            
            if (res.status === 401) {
                console.log("%c⚠️ Token masih bermasalah. Coba reload Discord (Ctrl+R) dan jalankan ulang!", "color: yellow");
            }
        }
    })
    .catch(err => console.error("%cError:", "color: red", err));
} else {
    console.log("%c❌ Gagal ambil token. Mencoba metode lain...", "color: red");
    
    // Metode alternatif: cari dari localStorage (kadang work di desktop)
    try {
        let altToken = localStorage.getItem("token");
        if (altToken) {
            altToken = altToken.replace(/^"|"$/g, '');
            console.log("%c✅ Token dari localStorage ditemukan!", "color: green");
            
            fetch("https://discord.com/api/v9/hypesquad/online", {
                method: "POST",
                headers: {
                    "Authorization": altToken,
                    "Content-Type": "application/json"
                },
                body: JSON.stringify({house_id: 1})
            })
            .then(async res => {
                if (res.ok) {
                    console.log("%c✅✅✅ BERHASIL! ✅✅✅", "color: green");
                    setTimeout(() => location.reload(), 1500);
                } else {
                    console.log("%c❌ Gagal: " + res.status, "color: red");
                }
            });
        } else {
            console.log("%c💀 Gak bisa ambil token sama sekali. Coba buka Discord di browser (web) aja!", "color: red");
        }
    } catch(e) {
        console.log("%c💀 Gagal total. Saran: pake Discord Web version aja lebih gampang!", "color: red");
    }
}
```

**Hasil:** Kamu langsung masuk ke Hypesquad Bravery! 🎉

---

## 📋 Script 2 – Keluar dari Hypesquad

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

if (token && typeof token === 'string' && token.length > 50) {
    fetch("https://discord.com/api/v9/hypesquad/online", {
        method: "DELETE",
        headers: { "Authorization": token }
    })
    .then(async res => {
        if (res.ok) {
            console.log("%c✅✅✅ BERHASIL KELUAR HYPESQUAD! ✅✅✅", "color: green");
            setTimeout(() => location.reload(), 1500);
        } else {
            console.log("%c❌ Gagal: " + res.status, "color: red");
        }
    });
} else {
    console.log("%c❌ Gagal ambil token. Coba pake Discord Web!", "color: red");
}
```

**Hasil:** Kamu keluar dari semua Hypesquad yang sedang aktif! 🚪

---

## ⚠️ Peringatan

- Script ini jalan di **client-side** (browser/modal Discord)
- Gak perlu token, gak perlu login ulang
- Hasilnya **langsung berubah** di profil kamu
- Kalau mau ganti ke Hypesquad lain, ubah aja `house_id`-nya di script 1

---

## 🔧 Kustomisasi

Mau ganti ke **Brilliance**? Ganti jadi gini:

```javascript
// Ganti house_id: 1  →  house_id: 2
api.post({url: "/hypesquad/online", body:{house_id: 2}})
```

Mau ke **Balance**? Pake `house_id: 3`!

---

## 💬 Catatan

> Script ini menggunakan modul internal Discord yang bisa berubah kapan aja. Kalau suatu saat error, berarti Discord udah update dan butuh penyesuaian.

# 🎖️ 3 Arti Badges Hypesquad Discord

---

## 🔵 **Bravery** (Keberanian)
- Untuk member yang **berani** dan **aktif** di komunitas
- Suka ngobrol, berani ngomong, ikut acara Discord
- Warna: **Biru**

---

## 🟡 **Brilliance** (Kecerdasan)
- Untuk member yang **kreatif**, **pintar**, dan suka **berbagi ilmu**
- Sering bikin konten keren, ngerjain quest, atau aktif di event
- Warna: **Kuning**

---

## 🟢 **Balance** (Keseimbangan)
- Untuk member yang **membantu orang lain** dan **menjaga kedamaian**
- Sering bantu jawab pertanyaan, moderasi, atau jembatan antar anggota
- Warna: **Hijau**

---

> 💡 **Catatan:** Kamu cuma bisa pilih **satu** dari ketiganya. Kalau mau ganti, harus keluar dulu dari yang sekarang!

Enjoy switching Hypesquad tanpa ribet! 🔥
