task default: %w[greet]

task :greet do
  puts 'Hello, world!'
end

task :rickroll do
  require_relative 'rick_roll_render.rb'
  puts RICKROLL
end

task :md_to_html do
  if `which pandoc`.empty?
    puts 'pandoc is not installed'
    exit 1
  end

  %w[README.md RICKROLL.md].each do |md_file|
    html_file = File.basename(md_file, '.md') + '.html'
    system("pandoc -o #{html_file} #{md_file}")
  end
end
