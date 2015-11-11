require 'fileutils'

resource = "/home/blei/stuff"

pwd = Dir.pwd

task :collect => "collect:all"
namespace :collect do
  task :beans do
    Dir.chdir(pwd)
    ruby "collector/jobs.rb"
    ruby "collector/skills.rb"
    ruby "collector/levels.rb"
#    FileUtils.cp("#{resource}/resource/version.cfg", Dir.pwd + "/src/main/resources/version.cfg")
  end
  
  task :json do
    Dir.chdir(pwd)
    ruby "collector/skilltrees.rb"
  end
  
  # Just the skillicons
  task :icons do
    Dir.chdir(pwd)
    images_path = Dir.pwd + "/src/main/webapp/resources/icons"
    puts "Clearing #{images_path}..."
    if not File.directory?(images_path)
      FileUtils.mkpath images_path
    elsif
      Dir[images_path + "/*.png"].each do |png|
        FileUtils.rm(png)
      end
    end
      
    Dir.chdir(resource + "/resource/ui/mainbar")
    puts "Converting .dds files to .png..."
    sh "mogrify -verbose -format png skillicon*.dds"
    puts
    Dir["*.png"].each do |png|
      FileUtils.mv(png, images_path + "/" + png)
    end
    
    Dir.chdir(images_path)
    puts "compressing png files..."
    Dir["*.png"].each do |png|
#      sh "mogrify -verbose -crop 500x#{rows}+0+0 +repage +repage #{png}"
      sh "pngcrush -ow #{png}"
    end
  end
  
  task :all => [:icons, :beans, :json]
end

task :clean do
  sh "mvn clean"
end

task :default do
  sh "mvn install"
  sh "foreman start"
end
