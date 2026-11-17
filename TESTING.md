# Jurassic Park 2 level-select hack — release test

**Result: PASS. v0.9 needs no ROM changes from this audit.**

Tested with the official **SameBoy 1.0.4 core**, matching the installed app version, with its installed boot ROMs. Both **Game Boy (DMG-B)** and **Game Boy Color (CGB-E)** models passed. Earlier PyBoy checks remain supplementary.

## Coverage

| Check | Result |
|---|---|
| Four languages × 15 stage launches × two hardware models | All 120 passed |
| Stage selector wraps both directions; lives selector wraps both directions | Passed |
| Three full stage/lives carousel cycles and 100 row changes per language/model | Passed |
| Long idle at menu | 1,800 frames per language/model; no corruption |
| Menu graphics timing | Zero writes during blocked VRAM periods |
| Monitor frame and stripe preservation | 224 menu snapshots passed |
| Actual lives: 3, 5, 7, 10, 20, 30, 40, 50, 60, 70, 80, 90, 99 | All values fully exhausted and restored on continue on both models |
| Actual enemy damage/death, without death assistance | Nine deaths and all three continues exhausted per model |
| Continue via A; game over; fresh new game | Passed on both models |
| Pause and resume; A remains jump during gameplay | Passed on both models |
| Soft reset and save/load state | Passed; menu snapshots repeatedly restored |
| Extra-life award | 90→91; 99 stays 99; selected starting amount remains saved |
| Copyright screen | Waits normally without input; A and Start skip; held buttons do not cascade through language selection |
| A and Start in the opening screens, selector, briefings, and continue screen | Passed |
| Whole native stage order, briefings, helicopter ending, and scrolling credits | All script states 1–38 reached in order on both models |
| Return from credits to a new selector; selected language persists | Passed |
| Identical-input comparison against original ROM | 27,000 gameplay frames across 15 stages and both models; framebuffer, gameplay RAM, audio registers, and PCM samples match |
| IPS replay and ROM checksums | Exact byte-for-byte reconstruction; valid checksums |
| Original graphical/script banks and native dispatcher | Preserved |

At least **841,901 emulated frames** were checked, excluding additional startup, game-over, award, and calibration runs.

## Important scope details

The progression/ending checks use the game's **native stage-completion animation and dispatcher**, triggered with a controlled completion flag. They verify the complete chain, cinematics, ending, and restart flow; they are **not an unassisted human completion of the game**. Combat, ordinary enemy damage, deaths, and continues were exercised separately.

Exhaustive life-count checks trigger the native death animation repeatedly without directly editing lives or continue counters. This tests every decrement, decimal boundary, persistent selected amount, and final continue restoration. Natural enemy deaths independently confirmed the ordinary damage path.

The original-versus-hack comparisons use a shared initialized native gameplay checkpoint per stage and identical controller inputs. Host audio output/filter state was also matched: a first comparison with unmatched emulator output state produced a false mismatch, and the corrected comparison passed on both models. No release-ROM fix was required.

A deliberately broken test ROM that restored 3 lives instead of the selected 5 was rejected by the same test (`FAIL BCD decrement/restore 3 5 frame=7431`). The temporary broken ROM was removed; only its test log remains.

These are emulated tests, not physical cartridge testing or an analog speaker listening test. The user's open SameBoy session and preferences were not changed.

## Artifact identity

- Release SHA-256: `e3f5871e4edf4c2cd888d30cf4fec4be5b08ea7da2cb495ed32b757300095cb7`
- Original ROM SHA-256: `d1f49235704e86070bcd04569be7d2d58643cc4021b1f7bc9eb5133e27457329`
- IPS patch reapplied to the original and matched the released ROM exactly.

Detailed machine-readable results are in `Final verification.json`; screenshots and per-test logs are alongside this report.
