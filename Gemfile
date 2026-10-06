source "https://rubygems.org"
git_source(:github) { |repo| "https://github.com/#{repo}" }

gem "uniword", "~> 1.5"

# --- relaton-v3 / pubid-v2 migration set --------------------------------
# All TEMPORARY git pins; flip each to a rubygems constraint as it releases.
# pubid#380 restored the 2.0.0.pre.alpha series metanorma-document pins.
gem "pubid", github: "pubid/pubid", branch: "main"
# relaton v3 monogem (Relaton::Bib consolidated; relaton-bib standalone retired)
# git-main relaton adds lutaml-store ~> 0.3, which conflicts with the
# glossarist ~> 2.13 required by metanorma-document; the released
# 3.0.0.pre.alpha line has no lutaml-store dep (harness resolution)
# gem "relaton", github: "relaton/relaton", branch: "main"
# relaton-cli v3 line requires the relaton v3 monogem (PR #133); the v3
# branch still self-declares 2.1.2, so pin the rubygems pre-release that
# satisfies standoc's >= 3.0.0.pre.alpha.3, < 3.1.0
gem "relaton-cli", "3.0.0.pre.alpha.4"
# 0.5.x: document model + removal of the stale Moxml children monkeypatch (metanorma#603)
gem "metanorma-document", github: "metanorma/metanorma-document", branch: "main"
# flavor table (metanorma-core#18), merged but unreleased
gem "metanorma-core", github: "metanorma/metanorma-core", branch: "main"
# RootAttributes + document ~> 0.5 expectations; same version number as the
# release but ahead of it. metanorma-mirror is its unreleased dependency.
gem "metanorma-mirror", github: "metanorma/metanorma-mirror", branch: "main"
gem "metanorma-standoc", github: "metanorma/metanorma-standoc", branch: "main"
# standoc main calls Metanorma::Utils::GcBudget, unreleased in the
# rubygems 2.0.7 line
gem "metanorma-utils", github: "metanorma/metanorma-utils", branch: "main"
# move-generic-document line (formats-table fix via metanorma-generic#128, merged)
gem "metanorma-generic", github: "metanorma/metanorma-generic", branch: "feat/move-generic-document"
# flavor-table compile line (metanorma#591/#602)
gem "metanorma", github: "metanorma/metanorma", branch: "main"
# isodoc github main widens relaton-render to "< 5"; the rubygems 3.7.3
# line caps it at ~> 1.3.0 and cannot coexist with relaton-render main
gem "isodoc", github: "metanorma/isodoc", branch: "main"
gem "relaton-render", github: "relaton/relaton-render", branch: "main"
# -------------------------------------------------------------------------

# iala taste with base generic (the feat/flavor-registry-integration tip
# flipped the base to iho, which has no guideline doctype); this ref is the
# generic-base iala addition on that branch. Flip to github main once the
# taste merges.
gem "metanorma-taste", github: "metanorma/metanorma-taste", ref: "0305fbf61dd51d8541a1a161b7beebb4e76a41bf"
# metanorma-cli: flavor deps are dev-only until the v3 wave releases
# (metanorma-cli#453, merged into the #452 integration line)
gem "metanorma-cli", github: "metanorma/metanorma-cli", branch: "feat/flavors-table"
gem "sassc-embedded", "~> 1"
