# Using Zach Holman's Rakefile
# https://raw.github.com/holman/dotdot_files/master/Rakefile

require 'rake'

desc "Link and run dotdot_files"
task :install do
  if !File.exists?("#{ENV['HOME']}/.oh-my-zsh")
    puts 'Oh My Zsh must be installed first'
    exit
  end

  dot_files = Dir.glob('*/**{.symlink}')

  dot_files.each do |dot_file|
    file = dot_file.split('/').last.split('.symlink').last

    target = "#{ENV["HOME"]}/.#{file}"

    # Symlink zsh files to oh-my-zsh custom dir
    target = "#{ENV["HOME"]}/.oh-my-zsh/custom/#{file}"  if File.extname(file) == ".zsh"

    if File.exists?(target) || File.symlink?(target)
      puts "File #{target} already exists, backup (b) or delete (d)?"

      backup = false
      case STDIN.gets.chomp.downcase
        when 'b' then backup = true
      end

      `mv "#{target}" "#{target}.backup"`  if backup == true
      FileUtils.rm_rf(target)
    end

    `ln -s "$PWD/#{dot_file}" "#{target}"`
  end

  puts "Run shell files? [Y/n]"

  case STDIN.gets.chomp.downcase
    when 'n' then exit
  end

  shell_files = Dir.glob('*/**{.sh}')

  shell_files.each do |file|
    puts "Running shell file #{file}"
    `sh #{file}`
  end
end

task :uninstall do
  dot_files = Dir.glob('*/**{.symlink}')

  dot_files.each do |dot_file|
    file = dot_file.split('/').last.split('.symlink').last

    target = "#{ENV["HOME"]}/.#{file}"
    target = "#{ENV["HOME"]}/.oh-my-zsh/custom/#{file}"  if File.extname(file) == ".zsh"

    if File.symlink?(target)
      puts "Removing #{target}..."
      FileUtils.rm_rf(target)
    end

    # Restore any backups made
    if File.exists?("#{ENV['HOME']}/.#{file}.backup")
      `mv #{ENV['HOME']}/.#{file}.backup #{target}`
    end
  end
end

task :default => 'install'
