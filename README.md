# seqwalt.github.io

- For initial setup, follow https://jekyllrb.com/docs/installation/ubuntu/, then:
  ```
  cd <this_repo>
  bundle install
  python3 -m venv .venv
  source .venv/bin/activate
  pip3 install jupyter
  deactivate
  ```
  If `mini_racer` fails to build during `bundle install`, you may need to try: https://github.com/rubyjs/mini_racer/issues/243#issuecomment-2285528586
- Run the site locally:
  ```
  cd <this_repo>
  source .venv/bin/activate
  bundle exec jekyll serve
  ```
- Convert Latex to MathML (for inline math in Bootstrap captions), use: https://temml.org/
