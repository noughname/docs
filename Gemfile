source "https://rubygems.org"

# GitHub Pages gem includes Jekyll and most plugins
gem "github-pages", "~> 228", group: :jekyll_plugins

# Just the Docs theme
gem "just-the-docs", "~> 0.7.0"

# Platform-specific dependencies
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1", :platforms => [:mingw, :x64_mingw, :mswin]
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]

# Performance booster for watching directories
gem "listen", "~> 3.8"
