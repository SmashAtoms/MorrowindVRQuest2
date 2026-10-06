# OpenMW Quest 0.50 port: preservation and provenance

## Preserved implementation

The original Quest 0.51 implementation remains on `main` at
`6f43519208706ad9ae9f6a066404a4c72c9c38dd`. This branch is
`quest-vr-0.50-stable`. The 0.51 APK remains in `apk/`, and its README
is retained as `README_QUEST_051.md`. Original code, build scripts,
configuration and documentation remain retrievable from the preserved commit.

## Baseline import

Official OpenMW 0.50.0: `47d78e004bc182def2904986f8bb54aea1f4b3ae`.
OpenMW-VR 0.50.1: `a7c2f31d0284508ddae8b8634bdf48c2cc75caef` from
`https://gitlab.com/madsbuvi/openmw`, branch `openmw-vr-0.50.1`.
Git merge-base confirms the official release is an ancestor of this VR branch.
The engine tree is imported from that VR baseline, not renamed from 0.51.
Quest-specific changes are ported in a subsequent commit.

The existing 0.51 VR source merged official `openmw-51-rc2`
(`673981c7f552089f1370d97fdac95e7221d4debc`) in commit
`c87d820cdcfdee4aa6406710587cdd536b087eb8`.
This gives separate official-engine, VR and Quest diffs, recorded in
`docs/porting-baselines.json` and the four `docs/*-files.csv` inventories.

## Save preservation

Before source changes, seven saves and nine configuration/log files were
copied to a new `/sdcard/saves_backup_051/` timestamped snapshot and a PC
backup outside this repository. SHA-256 hashes matched the source before
and after the copy, the headset backup, and the PC backup. Personal saves
and backup contents are never committed to this public repository.

0.50 writes format 34; the existing 0.51 Quest source writes format 37.
No save header is changed and no save-version check is bypassed.

## Validation status

This is a development branch. A 0.50 APK has not yet been built or tested.
The word "stable" names the project target, not a verified stability claim.
