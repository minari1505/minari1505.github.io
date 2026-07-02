source "https://rubygems.org"

# GitHub Pages 호환 빌드
gem "github-pages", group: :jekyll_plugins

# Apple system Ruby 2.6 cannot use the ffi 1.17+ darwin gems.
gem "ffi", "< 1.17"

# 테마에서 사용하는 플러그인
group :jekyll_plugins do
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-gist"
  gem "jekyll-feed"
  gem "jekyll-include-cache"
end

# Windows / JRuby 호환
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
