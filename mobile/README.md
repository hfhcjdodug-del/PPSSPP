# Quake II Mobile (2v2 online) - working notes

Goal: Quake II for Android and iOS, 2 teams of 2 players, real online play.

## Why this does not build on `src/ps2`
- `src/ps2/net` is an in-process loopback only (no sockets).
- `src/ps2/renderer` (22k lines) is written for the PS2 GS and VU1; it cannot run on a phone.
- So the mobile port starts from Yamagi Quake II (GPLv2, same id 3.19 codebase), vendored in
  [yquake2/](yquake2/) at the commit in `yquake2/UPSTREAM_COMMIT.txt`. The PS2 port stays untouched.

## Status (honest)
| Phase | What | State |
| --- | --- | --- |
| 0 | Dedicated server `q2ded` + `game.so` build on Linux | **Builds** (gcc 13). Not yet run with a map: needs `pak0.pak` |
| 1 | 2v2 mode in game code (teams, max 4, scoring, end of match) | Not started |
| 2 | Lobby / matchmaking service | Not started |
| 3 | Android client: SDL2 + GLES3, touch controls | Not started, no NDK/device in the dev sandbox |
| 4 | iOS client, performance | Not started, needs macOS/Xcode |

Nothing here has been tested on a phone.

## Build the dedicated server (Linux)
```sh
cd mobile/yquake2
mkdir -p release/baseq2
make -j2 release/q2ded release/baseq2/game.so WITH_CURL=no WITH_OPENAL=no WITH_SDL3=no
# q2ded refuses to run as root; use a normal user, with pak0.pak in release/baseq2/
./release/q2ded +set dedicated 1 +map q2dm1
```

## Known limits
- Quake II has no weapon reload; a reload button needs new weapon logic.
- `pak0.pak` is not redistributable; players supply their own copy.
