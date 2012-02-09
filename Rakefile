# Using Zach Holman's Rakefile
# https://raw.github.com/holman/dotdot_files/master/Rakefile

require 'rake'

desc "Link and run dotdot_files"
task :install do
  # TODO check for deps
  dot_files = Dir.glob('*/**{.symlink}')

  dot_files.each do |dot_file|
    file = dot_file.split('/').last.split('.symlink').last
    target = "#{ENV["HOME"]}/.#{file}"

    if File.exists?(target) || File.symlink?(target)
      puts "File #{target} already exists, backing up and removing old file"
      `mv "#{target}" "#{target}.backup"`
      FileUtils.rm_rf(target)
      # TODO ask to replace/skip existing dot_files
    else
      `ln -s "$PWD/#{dot_file}" "#{target}"`
    end
  end

  shell_files = Dir.glob('*/**{.sh}')

  shell_files.each do |file|
    puts "Running shell file #{file}"
    `sh #{file}`
  end
end

task :uninstall do
end

task :default => 'install'
