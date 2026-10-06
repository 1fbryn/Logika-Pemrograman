# DEAD ZONE

**Dead Zone** adalah game **Zombie Survival Roguelike** berbasis web
yang dibuat dalam satu file HTML. Pemain harus bertahan hidup dari
gelombang zombie yang terus meningkat, mengalahkan musuh, mendapatkan
XP, memilih upgrade, mengumpulkan Scrap, dan menghadapi Elite serta
Boss.

Game menggunakan **HTML5 Canvas**, **JavaScript**, dan **Web Audio API**
tanpa framework atau dependency eksternal. Seluruh game dapat dijalankan
langsung dari file HTML di browser.

> **Genre:** Zombie Survival / Roguelike / Arena Shooter\
> **Platform:** Web Browser\
> **Mode:** Normal, Hard, Nightmare\
> **Arsitektur:** Single-file HTML\
> **Arena:** 3400 × 3400 px

## Fitur Utama

-   Arena shooter dengan perspektif top-down.
-   Gerakan menggunakan keyboard dan membidik menggunakan mouse.
-   Sistem wave dengan jumlah dan komposisi musuh yang meningkat.
-   6 jenis senjata.
-   8 tipe musuh, termasuk Elite dan Boss.
-   Sistem XP dan level-up.
-   Pilihan upgrade berdasarkan rarity.
-   Power-up yang dapat ditemukan di arena.
-   Meta upgrade menggunakan Scrap.
-   Achievement yang tersimpan melalui `localStorage`.
-   Minimap.
-   Boss dengan beberapa fase dan pola serangan.
-   Efek visual seperti particle, damage number, screen shake, vignette,
    dan muzzle flash.
-   Sound effect sintetis menggunakan Web Audio API.
-   Pause dan mute.
-   Developer panel untuk testing/debugging.

## Cara Menjalankan

Tidak diperlukan instalasi atau build process.

### Opsi 1 --- Buka langsung

1.  Simpan file `The games.html`.
2.  Buka file tersebut menggunakan browser modern seperti Chrome, Edge,
    atau Firefox.
3.  Game akan langsung menampilkan menu utama.

### Opsi 2 --- Menggunakan Local Server

Jika ingin menjalankan melalui local server:

``` bash
python -m http.server 8000
```

Kemudian buka:

``` text
http://localhost:8000/The%20games.html
```

Game tidak membutuhkan backend maupun package tambahan.

## Kontrol

  Input          Fungsi
  -------------- -------------------------
  `W A S D`      Bergerak
  `Arrow Keys`   Bergerak
  Mouse          Membidik
  `Left Click`   Menembak
  `R`            Reload
  `1` -- `6`     Berganti senjata
  `ESC`          Pause / Resume
  `F2`           Membuka Developer Panel

> Tombol mouse kanan dinonaktifkan untuk mencegah context menu browser
> saat bermain.

## Game Modes

Game menyediakan tiga tingkat kesulitan:

### Normal

Mode standar dengan statistik awal pemain normal.

### Hard

HP awal dan Max HP pemain dikurangi menjadi sekitar **85%** dari nilai
normal.

### Nightmare

HP awal dan Max HP pemain dikurangi menjadi sekitar **70%** dari nilai
normal.

## Senjata

Terdapat enam senjata yang dapat digunakan dan diganti menggunakan
tombol `1–6`.

  ------------------------------------------------------------------------------
  Weapon        Damage  Fire Rate   Magazine     Reload   Projectile Special
  --------- ---------- ---------- ---------- ---------- ------------ -----------
  Pistol            32        2.5         12       1.2s            1 Balanced

  SMG               17        8.5         32       1.5s            1 High fire
                                                                     rate

  Shotgun           24        1.0          6       2.0s            7 Spread shot

  Assault           23        5.0         28       1.8s            1 Balanced
  Rifle                                                              automatic
                                                                     weapon

  Sniper           130        0.7          5       2.5s            1 High
  Rifle                                                              damage,
                                                                     crit,
                                                                     pierce

  Plasma            48        3.0         16       2.0s            1 Crit and
  Gun                                                                pierce
  ------------------------------------------------------------------------------

Weapon juga memiliki atribut seperti bullet speed, spread, critical
chance, recoil, dan pierce.

## Musuh

Musuh memiliki karakteristik dan perilaku yang berbeda.

  ------------------------------------------------------------------------------
  Enemy          HP Dasar        Speed       Damage           XP Karakteristik
  ---------- ------------ ------------ ------------ ------------ ---------------
  Normal               45           62           10           10 Musuh standar

  Runner               30          115            8           12 Cepat dan
                                                                 agresif

  Tank                185           36           23           32 HP tinggi dan
                                                                 berbadan besar

  Spitter              72           46           13           20 Menyerang dari
                                                                 jarak jauh

  Exploder             58           88           45           16 Meledak ketika
                                                                 mendekati
                                                                 pemain

  Screamer             62           52            5           25 Memberikan buff
                                                                 kepada musuh di
                                                                 sekitar

  Elite               320           82           20           85 Musuh kuat
                                                                 dengan reward
                                                                 lebih besar

  Boss               2000           52           34          550 Musuh spesial
                                                                 dengan beberapa
                                                                 fase
  ------------------------------------------------------------------------------

Statistik musuh meningkat seiring bertambahnya wave.

## Wave System

Game menggunakan sistem wave. Sepuluh konfigurasi wave pertama memiliki
komposisi musuh yang telah ditentukan.

-   Wave awal berisi Normal Zombie.
-   Runner, Tank, dan Spitter mulai muncul pada wave berikutnya.
-   Screamer dan Exploder muncul pada wave yang lebih tinggi.
-   Elite muncul secara khusus pada wave kelipatan 5.
-   Boss muncul pada wave kelipatan 10.

Setelah wave ke-10, komposisi musuh dibuat secara dinamis dan jumlah
musuh terus meningkat.

Setiap wave memiliki interval spawn dan jumlah musuh yang berbeda.
Kesulitan juga meningkat karena HP, damage, dan sebagian speed musuh
diskalakan berdasarkan nomor wave.

## Boss

Boss memiliki:

-   HP dasar 2000.
-   Sistem **Phase 1** dan **Phase 2**.
-   Memasuki kondisi **Enraged** ketika HP berada di bawah 50%.
-   Kecepatan meningkat saat enraged.
-   Tiga pola perilaku/serangan yang berganti secara berkala.
-   Serangan projectile berbentuk radial.
-   HP bar khusus di bagian atas layar.
-   Reward XP dan power-up dalam jumlah besar ketika dikalahkan.

Boss muncul secara otomatis pada wave kelipatan 10.

## XP & Leveling

Musuh yang dikalahkan memberikan XP.

Saat XP mencapai batas level:

1.  Pemain naik level.
2.  XP yang melebihi batas dibawa ke proses berikutnya.
3.  Batas XP berikutnya meningkat.
4.  Game menampilkan empat pilihan upgrade.
5.  Pemain memilih satu upgrade sebelum kembali bermain.

XP requirement meningkat secara progresif menggunakan formula:

``` text
XP berikutnya = floor(XP sebelumnya × 1.26 + 35)
```

## Upgrade

Upgrade saat level-up dibagi menjadi empat rarity:

-   Common
-   Rare
-   Epic
-   Legendary

### Common

-   +15% Damage
-   +10% Speed
-   +22 Max HP
-   +13% Fire Rate
-   +3 HP/s Regen
-   +15% Pickup Radius
-   +5% Critical Chance

### Rare

-   +28% Damage
-   +1 Projectile
-   +1 Pierce
-   +4% Lifesteal
-   +45% Critical Damage
-   +18 Armor
-   +22% Bullet Speed

### Epic

-   **Critical Eye** --- +15% Crit dan +65% Crit Damage
-   **Berserker** --- +38% Damage dan +22% Speed
-   **Barrage** --- +2 Extra Projectiles
-   **Vampire** --- +8% Lifesteal dan +6 HP/s Regen
-   **Juggernaut** --- +35 HP, +22 Armor, +4 Regen

### Legendary

-   **Glass Cannon** --- +55% Damage tetapi -32 Max HP
-   **Overclock** --- +45% Fire Rate
-   **Rampage** --- +3 Projectiles dan +32% Damage
-   **Iron Fortress** --- +65 HP, +42 Armor, +9 Regen
-   **Phantom Rounds** --- +3 Pierce dan +25% Bullet Speed

Peluang rarity meningkat untuk Epic dan Legendary ketika level pemain
semakin tinggi.

## Power-ups

Power-up dapat diperoleh dari drop tertentu, terutama dari Elite dan
Boss.

  Power-up         Durasi
  ------------ ----------
  Berserk        10 detik
  Rapid Fire      9 detik
  Shield         15 detik
  Magnet         13 detik
  Double XP      15 detik
  Regen          10 detik

Power-up memberikan efek sementara yang membantu pemain bertahan atau
meningkatkan kemampuan menyerang.

## Drops

Musuh dapat menjatuhkan beberapa jenis item:

-   **XP** --- menambah XP.
-   **HP** --- memulihkan health.
-   **Ammo** --- mengisi sebagian amunisi.
-   **Power-up** --- memberikan buff sementara.

Item dapat tertarik menuju pemain ketika efek Magnet aktif.

## Scrap & Meta Upgrades

Selain progression selama satu permainan, Dead Zone memiliki progression
permanen menggunakan **Scrap**.

Scrap dapat diperoleh melalui:

-   Mengalahkan musuh.
-   Menyelesaikan achievement.
-   Mendapatkan Scrap setelah game over berdasarkan performa permainan.

Meta upgrade yang tersedia:

  Upgrade          Efek                              Maks. Level
  ---------------- ------------------------------- -------------
  Starting HP      +10 Max HP per level                       10
  Base Damage      +10% Damage per level                      10
  Starting Speed   +10 Speed per level                        10
  Crit Chance      +2% Critical Chance per level               5

Biaya setiap upgrade meningkat berdasarkan level upgrade sebelumnya.

Progression permanen disimpan menggunakan `localStorage`.

## Achievement

Game memiliki achievement yang memberikan Scrap ketika pertama kali
berhasil dicapai.

  Achievement    Syarat                   Reward
  -------------- ------------------- -----------
  Survivor       100 kills              50 Scrap
  Exterminator   1000 kills            200 Scrap
  Veteran        Mencapai level 10     100 Scrap
  Endurance      Bertahan 10 menit     150 Scrap
  Wave Runner    Mencapai wave 15      150 Scrap
  Boss Slayer    Mengalahkan Boss      100 Scrap

Achievement yang telah terbuka disimpan di browser menggunakan
`localStorage`.

## Arena & Visual

Arena memiliki ukuran **3400 × 3400 px** dan dibuat secara prosedural
menggunakan sejumlah obstacle.

Visual game dirender menggunakan HTML5 Canvas dan mencakup:

-   Grid arena.
-   Obstacle.
-   Player dan weapon direction.
-   Zombie dan variasi bentuk musuh.
-   Projectile dan bullet trail.
-   Particle effects.
-   Damage numbers.
-   Muzzle flash.
-   Screen shake.
-   Vignette.
-   Low-health red flash.
-   Boss HP bar.
-   Minimap.

## Audio

Audio dibuat secara real-time menggunakan **Web Audio API**.

Sound effect tersedia untuk beberapa event seperti:

-   Shooting.
-   Shooting dengan senjata tertentu.
-   Hit.
-   Reload.
-   Level up.
-   Pickup.
-   Death.
-   Boss appearance.
-   Explosion.

Audio dapat dimatikan melalui tombol **Toggle Mute** pada pause menu.

## Developer Panel

Developer Panel dapat dibuka menggunakan `F2`.

Fitur debugging yang tersedia:

-   Add XP / Level Up
-   Full Heal
-   +200 Scrap
-   Spawn Boss
-   Spawn Elite
-   +5 Waves
-   God Mode

Panel ini ditujukan untuk testing dan debugging, bukan gameplay normal.

## Teknologi

Project ini menggunakan:

-   **HTML5**
-   **CSS3**
-   **JavaScript**
-   **HTML5 Canvas 2D API**
-   **Web Audio API**
-   **Browser Local Storage**

Tidak terdapat framework JavaScript atau library eksternal yang
diperlukan.

## Struktur Project

Karena game dibuat sebagai single-file application, struktur dasarnya
sangat sederhana:

``` text
.
└── The games.html
```

Di dalam file HTML terdapat tiga bagian utama:

``` text
The games.html
├── HTML
│   ├── Main Menu
│   ├── Pause Screen
│   ├── Game Over Screen
│   ├── Level Up Screen
│   ├── Meta Upgrade Shop
│   └── Developer Panel
│
├── CSS
│   └── UI styling & visual effects
│
└── JavaScript
    ├── Game State
    ├── Input
    ├── Audio
    ├── Weapons
    ├── Player
    ├── Enemies
    ├── Bullets
    ├── Drops
    ├── XP & Leveling
    ├── Upgrades
    ├── Waves
    ├── Particles
    ├── Arena
    ├── Camera
    ├── Rendering
    ├── Achievements
    ├── Meta Upgrades
    └── Game Loop
```

## Game Loop

Game berjalan menggunakan `requestAnimationFrame`.

Secara umum setiap frame menjalankan:

``` text
Input
  ↓
Player Update
  ↓
Enemy Update
  ↓
Bullet Update
  ↓
Particle / Damage Number Update
  ↓
Wave Update
  ↓
Camera Update
  ↓
Achievement Check
  ↓
Render
```

Game membatasi delta time maksimum untuk menjaga stabilitas simulasi
ketika frame rate turun.

## Persistence

Progress tertentu disimpan menggunakan browser `localStorage`, terutama:

-   Scrap.
-   Meta upgrade level.
-   Best wave.
-   Best level.
-   Achievement yang telah terbuka.

Artinya, progress dapat tetap tersedia setelah halaman ditutup selama
data browser tidak dihapus.

## Development Notes

Game dirancang sebagai **single-file browser game**, sehingga cocok
untuk:

-   Prototype game.
-   Eksperimen JavaScript Canvas.
-   Pembelajaran game loop.
-   Eksperimen procedural spawning.
-   Eksperimen progression dan roguelike mechanics.
-   Demo game tanpa backend.

Karena seluruh sistem berada dalam satu file, project relatif mudah
dipindahkan dan dimainkan, tetapi akan menjadi lebih sulit dipelihara
apabila kode terus berkembang. Untuk pengembangan lebih lanjut, sistem
dapat dipisahkan menjadi beberapa module seperti `player.js`,
`weapons.js`, `enemies.js`, `waves.js`, dan `render.js`.

## License

Belum terdapat informasi lisensi khusus di dalam source code ini.

Jika project akan dipublikasikan ke repository atau didistribusikan,
tambahkan file `LICENSE` dengan lisensi yang sesuai.
