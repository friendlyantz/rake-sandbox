task default: %w[greet]

task :greet do
  puts 'Hello, world!'
end

task :rickroll do
  require_relative 'rick_roll_render.rb'
  puts RICKROLL
end

task md_to_html: %W[README.html RICKROLL.html] 

%w[README.md RICKROLL.md].each do |md_file|
  html_file = File.basename(md_file, '.md') + '.html'
  file html_file => md_file do # this defines a file saving rule for HTML files
    if `which pandoc`.empty?
      puts 'pandoc is not installed'
      exit 1
    end
    sh "pandoc -o #{html_file} #{md_file}"
  end
end
