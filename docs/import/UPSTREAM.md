# Independent Amethyst source import

Imported on 2026-10-25 from https://github.com/AngelAuraMC/Amethyst-Android.
Upstream default branch: v3_openjdk.
Exact baseline: 330c6eae3164df64bdc4828e946a9e62cc5169e4.

All 111 advertised upstream branches are imported, retaining their full history.
The integration branch v3_openjdk merges the original destination main history.
Its initial GPL license is retained separately here; the upstream LICENSE and
bundled attribution files remain unchanged. The other 110 branch tips are exact
copies of upstream. The tag verification workflow imports and checks all 140
original tag objects without rewriting them.

Submodule URLs and gitlinks remain unchanged. HTTPS translation of the
androidnsbypass SSH URL is only a checkout setting. No LFS pointers were found
by git lfs ls-files --all during local source inspection.

origin is Russianranger/amethyst_cursorfix; upstream is fetch-only.
The initial Actions import transferred the objects but its branch push was
rejected because GITHUB_TOKEN lacks workflow-write permission. The authorized
GitHub connector created the imported branch refs while Actions was disabled,
including the merge commit 65ed2acc704192085a9637d431570c0bdcd10f17.
Tag synchronization uses the normal repository token and does not trigger
per-tag builds. Upstream's release workflow is gated to its original repository;
the separate destination debug workflow requires no upstream release secrets.
