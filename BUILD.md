# Tiny Chess — BUILD

Engine Python 4 KB dari nol. `src.py` = sumber readable (15.199 byte);
`main.py` (3.914 byte) = `makefun compress` dari `src.py`.

## Desain
Board 0x88; negamax alpha-beta + quiescence (stand-pat, delta +200),
transposition table, MVV-LVA + killer + history, null-move (R=2,
d≥3, tidak saat skak, NPM > 900). Eval: material P100 N320 B330 R500
Q900 + PST klasik 7 tabel (endgame NPM ≤ 1500, bishop pair +30),
skor inkremental. Root: iterative deepening, budget node TETAP per
iterasi (tanpa clock/randomness); best dilacak `>` ketat. Driver:
POST /game, /move (UCI); FEN + history dari `moves` env.

## Hasil ukur (practice 6 game: depth-2..nodes-500k)
| Versi | T0 | T1 | T2 | Mean | Waktu |
|---|---|---|---|---|---|
| v1b (+promo) | 1099 | – | – | – | 4m12s |
| **v3 FINAL (null, 60k)** | **1451** | **1451** | **1279** | **~1394** | **~11 mnt** |
| v3+LMR | 1451 | – | 1279 | ~1365 | ~15 mnt |
| v3+100k | 1279 | – | – | – | 19m26s |

LMR di-revert (Elo sama, lebih lambat); 100k di-revert (Elo turun).
Target > 1.400 tercapai di T0/T1; mean ~1394, di atas sunfish (~1.365)
dan floor final (1.154). `main.py` identik dengan `src.py` (T0: 4-0-2).

## Bug yang diperbaiki
1. **PST off-by-one:** `val()` pakai `PST[tipe]` tapi tuple mulai
   indeks 0 = pion (+IndexError `PST[7]`). Fix: sisip tuple nol.
2. **Root-selection:** `sorted(scored)` + tie-break int move pilih
   langkah buruk (h8g8 di Berlin). Fix: lacak best `s > ba` ketat.
3. **Promosi hitam:** `npc = p` jadi RATU PUTIH (+990 phantom).
   Fix: `npc = p * SD`.

## Validasi
Perft lulus (startpos 20/400/8902, kiwipete 48/2039/97862, EP 31,
castling 25, promosi 9). Eval inkremental == penuh (tanpa drift).
Taktik: QxQ → d1d5; mate-in-1 → a1a8. ~61–66k nps; think ~2,1 dtk.
Trial 6 game ~11 menit (final 8 game ≈ 15 menit < wall 1.800 dtk).

## Selanjutnya (parent)
Repo `cholil677/tiny-chess`: upload keempat file; About
https://make.fun/bounty/tiny-chess-01, topics makefun, ai-agents,
bounties, games. Bundle + submit sebelum Sel 25 Nov 2026 00:00 WIB.
Kit lokal level 27, situs level 28 — re-download kit dulu.
