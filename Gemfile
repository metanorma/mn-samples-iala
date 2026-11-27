source "https://rubygems.org"

gem "metanorma-cli", github: "metanorma/metanorma-cli", branch: "main"
gem "metanorma-taste", "~> 1.1"
gem "metanorma-iho", "~> 1.4"
gem "uniword", "~> 1.5"
gem "debug"

# LADL parity harness: label the compiled presentation models through
# the engine and diff against the baked numbering (bin/iala_parity).
# The labeller seam merged (metanorma-document#179): git main until the
# next release carries it, then "~> 0.5.16".
gem "metanorma-document", github: "metanorma/metanorma-document",
  branch: "main"
# TEMPORARY pin: {key} vocabulary tokens + the MN 112 evaluator are on
# main past the 0.1.1 release — flip to the next release when shipped.
gem "ladl", github: "metanorma/ladl", branch: "main"
