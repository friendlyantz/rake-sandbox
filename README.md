# rake-sandbox
# CLI

## Incline execution
```sh
ruby -e puts 'Hello, world!'
rake -e puts 'Hello, world!'
```

# File writing

```ruby
%W[ch1.md ch2.md ch3.md].each do |md_file|
  html_file = File.basename(md_file, ".md") + ".html"
  file html_file => md_file do
    sh "pandoc -o #{html_file} #{md_file}"
  end
end
```

I had to call with `.html`, not `.md`, which was a bit confusing.
Now rake handles file writing and trackes if there are any updates in `.md` file worth calling a conversion operation
```sh
❯ rake md_to_html README.html
pandoc -o README.html README.md
❯ rake md_to_html README.html
❯ rake md_to_html README.html
```

# Resources

https://graceful.dev/courses/the-freebies/modules/rake-and-project-automation/topic/episode-129-rake/
