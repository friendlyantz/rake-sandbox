task default: %w[greet]

task :greet do
  puts 'Hello, world!'
end

task :rickroll do
  require_relative 'rick_roll_render'
  puts RICKROLL
end

task md_to_html: %w[README.html RICKROLL.html]

rule '.html' => '.md' do |t| # unlike previous commit this rule defines a saving
  # RULE for files with .html extension with prerequisite 'md' file, so the
  # rake initially looks for a rule with exact match of `README.html`, doesn't
  # find and then goes to generic '.html' rule
  # Previously with used `file` task, with explicit name of the file
  if `which pandoc`.empty?
    puts 'pandoc is not installed'
    exit 1
  end
  sh "pandoc -o #{t.name} #{t.source}"
end
