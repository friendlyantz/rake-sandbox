* rake-sandbox
{:toc}

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

## using Rules

unlike previous commit this rule defines a saving
RULE for files with .html extension with prerequisite 'md' file, so the
rake initially looks for a rule with exact match of `README.html`, doesn't
find and then goes to generic '.html' rule
Previously with used `file` task, with explicit name of the file
```ruby
rule '.html' => '.md' do |t| 
  if `which pandoc`.empty?
    puts 'pandoc is not installed'
    exit 1
  end
  sh "pandoc -o #{t.name} #{t.source}"
end
```

## Listing files

```ruby
files = Rake::FileList['**/*.md']
files.exclude('**/~*') # exclude files with ~ in the name
files.exclude do |file|
  `git ls-files #{file}`.empty? # exclude files that are not tracked by git
end
files.exclude /^ignoredir/ # using REGEX
```

OR create instance of a file list and pass a block 

```ruby
files = Rake::FileList.new('**/*.md') do |fl|
  fl.exclude('**/~*') # exclude files with ~ in the name
  fl.exclude do |file|
    `git ls-files #{file}`.empty? # exclude files that are not tracked by git
  end
  fl.exclude(/^ignoredir/) # using REGEX
  fl.exclude(/README/) # using REGEX
end
```
### listing files with new file ext

```ruby
files.ext('html')
```

# Debugging

```ruby
Rake.application.options.trace_rules = true
```

then run
```sh
rake --trace
```

# Resources

https://graceful.dev/courses/the-freebies/modules/rake-and-project-automation/topic/episode-129-rake/
