TOMCAT_INSTALL_ROOT = '/Developer/apache-tomcat/Versions/apache-tomcat-5.5.26'
J2EE_CONTAINER_WEBAPPS_DIR = "#{TOMCAT_INSTALL_ROOT}/webapps"
J2EE_CONTAINER_START_COMMAND = "#{TOMCAT_INSTALL_ROOT}/bin/startup.sh"
J2EE_CONTAINER_STOP_COMMAND = "#{TOMCAT_INSTALL_ROOT}/bin/shutdown.sh"
APP_NAME = File.basename( File.dirname(__FILE__) )

task :test do
  puts APP_NAME
end

task :deploy => [:build] do
  sh J2EE_CONTAINER_STOP_COMMAND
  sh "rm -fr #{J2EE_CONTAINER_WEBAPPS_DIR}/#{APP_NAME}"
  sh "cp -r tmp/war #{J2EE_CONTAINER_WEBAPPS_DIR}/#{APP_NAME}"
  sh J2EE_CONTAINER_START_COMMAND
end

task :build => [:clean] do
  sh "warble war"
  Rake::Task[:patch_sinatra].execute
end

task :clean do
  sh "rm -fr #{APP_NAME}.war"
  sh "rm -fr tmp"
end

task :patch_sinatra do
  #path = "tmp/war/WEB-INF/gems/gems/sinatra-0.9.2/lib/sinatra.rb"
  Dir["*/**/sinatra.rb"].each do |path|
    lines = File.open(path, "r").readlines.collect{|l| (l.index("use_in_file_templates") != nil) ? "# #{l}" : l }
    File.open(path, "w") { |f| f.write(lines.join) }
  end
end