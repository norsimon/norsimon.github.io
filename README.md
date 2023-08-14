# Running Jekyll and updating files
*copied from Jekyll_setup.org*

- To start local server, execute in terminal
  : bundle exec jekyll serve
  + also exists as alias "serveme" in zshell on 13"
- Then navigate to
  : http://localhost:4000
- Likely have to first do
  : export PATH=/usr/local/opt/ruby/bin:$PATH
- Pages to edit in emacs (default for files of type .md)
  + /_pages/index.md
  + /_pages/about.md
  + /_pages/posts.md
  + /_pages/research.md
- Setup to edit in Xcode or Emacs (default for files of type .yml)
  + _config.yml
  + /_data/navigation.yml
  + Beware: Xcode can introduce, but not show, "control character". Open in emacs to reveal.
- Commit, push, and pull to GitHub using Github Desktop
