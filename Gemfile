source "https://rubygems.org"

gem "metanorma-cli", github: "metanorma/metanorma-cli", branch: "main"
gem "metanorma-taste", "~> 1.1"
gem "metanorma-iho", "~> 1.4"
gem "uniword", "~> 1.5"
gem "debug"

# LADL parity harness: label the compiled presentation models through
# the engine and diff against the baked numbering (bin/iala_parity).
# TEMPORARY pin: the labeller seam rides metanorma-document#179 —
# flip to released when merged.
gem "metanorma-document", github: "metanorma/metanorma-document",
  branch: "feat/ladl-labeller-seam"
# TEMPORARY pin: {key} vocabulary tokens ship on main (ladl#7) —
# flip to "~> 0.2" when released.
gem "ladl", github: "metanorma/ladl", branch: "main"
