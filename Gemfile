source "https://rubygems.org"
git_source(:github) { |repo| "https://github.com/#{repo}" }

gem "uniword", "~> 1.5"

# --- relaton-v3 / pubid-v2 migration set --------------------------------
# All TEMPORARY git pins; flip each to a rubygems constraint as it releases.
# pubid#380 restored the 2.0.0.pre.alpha series metanorma-document pins.
gem "pubid", github: "pubid/pubid", branch: "main"
# relaton v3 monogem (Relaton::Bib consolidated; relaton-bib standalone retired)
gem "relaton", github: "relaton/relaton", branch: "main"
# relaton-cli#131 widens relaton >= 2.1.0 so v3 can resolve
gem "relaton-cli", github: "relaton/relaton-cli", branch: "feat/allow-relaton-v3-v2line"
# 0.5.x: document model + removal of the stale Moxml children monkeypatch (metanorma#603)
gem "metanorma-document", github: "metanorma/metanorma-document", branch: "main"
# flavor table (metanorma-core#18), merged but unreleased
gem "metanorma-core", github: "metanorma/metanorma-core", branch: "main"
# RootAttributes + document ~> 0.5 expectations; same version number as the
# release but ahead of it. metanorma-mirror is its unreleased dependency.
gem "metanorma-mirror", github: "metanorma/metanorma-mirror", branch: "main"
gem "metanorma-standoc", github: "metanorma/metanorma-standoc", branch: "main"
# move-generic-document line (formats-table fix via metanorma-generic#128, merged)
gem "metanorma-generic", github: "metanorma/metanorma-generic", branch: "feat/move-generic-document"
# flavor-table compile line (metanorma#591/#602)
gem "metanorma", github: "metanorma/metanorma", branch: "main"
# -------------------------------------------------------------------------

# iala taste (metanorma-taste#217, merged into feat/flavor-registry-integration)
gem "metanorma-taste", github: "metanorma/metanorma-taste", branch: "feat/flavor-registry-integration"
# metanorma-cli: flavor deps are dev-only until the v3 wave releases
# (metanorma-cli#453, merged into the #452 integration line)
gem "metanorma-cli", github: "metanorma/metanorma-cli", branch: "feat/flavors-table"
gem "sassc-embedded", "~> 1"
