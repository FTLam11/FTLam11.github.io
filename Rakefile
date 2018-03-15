task :default do
  puts "lol"
end

desc "Generate post"
task :new_post, [:name, :tag] do |t, args|
  args.with_defaults(name: "new_post", tag: "ruby")
  date = Time.now.strftime('%F')
  file_name = date + '-' + "#{args.name.downcase.gsub(/\s/, '_')}" + '.md'
  output_path = File.join('_posts', file_name)
  cp('legacy/template.markdown', output_path, verbose: true)
  old_text = File.read(output_path)
  new_text = old_text.gsub('__TITLE__', args.name).gsub('__DATE__', date).gsub('__TAG__', args.tag)
  File.open(output_path, "w") { |f| f.puts new_text }
end
