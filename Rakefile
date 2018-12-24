# coding: utf-8
task :default => :preview

# CONFIGURATION VARIABLES (on top of those defined by Jekyll in _config(_deploy).yml)
#
# PREVIEWING
# If your project is based on compass and you want compass to be invoked
# by the script, set the $compass variable to true
#
# $compass = false # default
# $compass = true  # if you wish to run compass as well
#
# Notice that Jekyll 2.0 supports sass natively, so you might want to have a look
# at the functions provided by Jekyll instead of using the functions provided here.
#
# MANAGING POSTS
# Set the extension for new posts (defaults to .textile, if not set)
#
# $post_ext = ".textile"  # default
# $post_ext = ".md"       # if you prefer markdown
#
# Set the location of new posts (defaults to "_posts/", if not set).
# Please, terminate with a slash:
#
# $post_dir = "_posts/"
#
# MANAGING MULTI-USER WORK
# If you are using git to manage the sources, you might want to check the repository
# is up-to-date with the remote branch, before deploying.  In fact---when this is not the
# case---you end up deploying a previous version of your website.
#
# The following variable determines whether you want to check the git repository is
# up-to-date with the remote branch and, if not, issue a warning.
#
# $git_check = true
#
# It is safe to leave the variable set to true, even if you do not manage your sources
# with git.
#
# The following variable controls whether we push to the remote branch after deployment,
# committing all uncommitted changes
#
# $git_autopush = false
#
# If set to true, the sources have to be managed by git or an error message will be issued.
#
# ... or load them from the configuration file, e.g.:
# 
load '_rake-configuration.rb' if File.exist?('_rake-configuration.rb')
load '_rake_configuration.rb' if File.exist?('_rake_configuration.rb')

# ... we are a bit redundant and allow two different file names


#
# --- NO NEED TO TOUCH ANYTHING BELOW THIS LINE ---
#

# Specify default values for variables NOT set by the user

$post_ext ||= ".textile"
$post_dir ||= "_posts/"
$git_check ||= true
$git_autopush ||= false

#
# Tasks start here
#

desc 'Clean up generated site'
task :clean do
  cleanup
end


desc 'Preview on local machine (server with --auto)'
task :preview => :clean do
  compass('compile') # so that we are sure sass has been compiled before we run the server
  compass('watch &')
  jekyll('serve --watch --livereload')
end
task :serve => :preview


desc 'Build for deployment (but do not deploy)'
task :build, [:deployment_configuration] => :clean do |t, args|
  args.with_defaults(:deployment_configuration => 'deploy')
  config_file = "_config_#{args[:deployment_configuration]}.yml"

  if rake_running then
    puts "\n\nWarning! An instance of rake seems to be running (it might not be *this* Rakefile, however).\n"
    puts "Building while running other tasks (e.g., preview), might create a website with broken links.\n\n"
    puts "Are you sure you want to continue? [Y|n]"

    ans = STDIN.gets.chomp
    exit if ans != 'Y' 
  end

  compass('compile')
  jekyll("build --config _config.yml,#{config_file}")
end


desc 'Build and deploy to remote server'
task :deploy, [:deployment_configuration] => :build do |t, args|
  args.with_defaults(:deployment_configuration => 'deploy')
  config_file = "_config_#{args[:deployment_configuration]}.yml"

  text = File.read("_config_#{args[:deployment_configuration]}.yml")
  matchdata = text.match(/^deploy_dir: (.*)$/)
  if matchdata

    if git_requires_attention("master") then
      puts "\n\nWarning! It seems that the local repository is not in sync with the remote.\n"
      puts "This could be ok if the local version is more recent than the remote repository.\n"
      puts "Deploying before committing might cause a regression of the website (at this or the next deploy).\n\n"
      puts "Are you sure you want to continue? [Y|n]"

      ans = STDIN.gets.chomp
      exit if ans != 'Y' 
    end

    deploy_dir = matchdata[1]
    sh "rsync -avz --delete _site/ #{deploy_dir}"
    time = Time.new
    File.open("_last_deploy.txt", 'w') {|f| f.write(time) }
    %x{git add -A && git commit -m "autopush by Rakefile at #{time}" && git push} if $git_autopush
  else
    puts "Error! deploy_url not found in _config_deploy.yml"
    exit 1
  end
end

desc 'Build and deploy to github'
task :deploy_github => :build do |t, args|
  args.with_defaults(:deployment_configuration => 'deploy')
  config_file = "_config_#{args[:deployment_configuration]}.yml"

  if git_requires_attention("gh_pages") then
    puts "\n\nWarning! It seems that the local repository is not in sync with the remote.\n"
    puts "This could be ok if the local version is more recent than the remote repository.\n"
    puts "Deploying before committing might cause a regression of the website (at this or the next deploy).\n\n"
    puts "Are you sure you want to continue? [Y|n]"

    ans = STDIN.gets.chomp
    exit if ans != 'Y' 
  end

  %x{git add -A && git commit -m "autopush by Rakefile at #{time}" && git push origin gh_pages} if $git_autopush
  
  time = Time.new
  File.open("_last_deploy.txt", 'w') {|f| f.write(time) }
end

desc 'Create a post'
task :create_post, [:date, :title, :category, :content] do |t, args|
  if args.title == nil then
    puts "Error! title is empty"
    puts "Usage: create_post[date,title,category,content]"
    puts "DATE and CATEGORY are optional"
    puts "DATE is in the form: YYYY-MM-DD; use nil or empty for today's date"
    puts "CATEGORY is a string; nil or empty for no category"
    exit 1
  end
  if (args.date != nil and args.date != "nil" and args.date != "" and args.date.match(/[0-9]+-[0-9]+-[0-9]+/) == nil) then
    puts "Error: date not understood"
    puts "Usage: create_post[date,title,category,content]"
    puts "DATE and CATEGORY are optional"
    puts "DATE is in the form: YYYY-MM-DD; use nil or the empty string for today's date"
    puts "CATEGORY is a string; nil or empty for no category"
    puts ""

    title = args.title || "title"

    puts "Examples: create_post[\"\",\"#{args.title}\"]"
    puts "          create_post[nil,\"#{args.title}\"]"
    puts "          create_post[,\"#{args.title}\"]"
    puts "          create_post[#{Time.new.strftime("%Y-%m-%d")},\"#{args.title}\"]"
    exit 1
  end

  post_title = args.title
  post_date = (args.date != "" and args.date != "nil") ? args.date : Time.new.strftime("%Y-%m-%d %H:%M:%S %Z")

  # the destination directory is <<category>>/$post_dir, if category is non-nil
  # and the directory exists; $post_dir otherwise (a category tag is added in
  # the post body, in this case)
  post_category = args.category
  if post_category and Dir.exists?(File.join(post_category, $post_dir)) then
    post_dir = File.join(post_category, $post_dir)
    yaml_cat = nil
  else
    post_dir = $post_dir
    yaml_cat = post_category ? "category: #{post_category}\n" : nil
  end

  def slugify (title)
    # strip characters and whitespace to create valid filenames, also lowercase
    return title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  end

  filename = post_date[0..9] + "-" + slugify(post_title) + $post_ext

  # generate a unique filename appending a number
  i = 1
  while File.exists?(post_dir + filename) do
    filename = post_date[0..9] + "-" +
               File.basename(slugify(post_title)) + "-" + i.to_s +
               $post_ext 
    i += 1
  end

  # the condition is not really necessary anymore (since the previous
  # loop ensures the file does not exist)
  if not File.exists?(post_dir + filename) then
    File.open(post_dir + filename, 'w') do |f|
      f.puts "---"
      f.puts "title: \"#{post_title}\""
      f.puts "layout: default"
      f.puts yaml_cat if yaml_cat != nil
      f.puts "date: #{post_date}"
      f.puts "---"
      f.puts args.content if args.content != nil
    end  

    puts "Post created under \"#{post_dir}#{filename}\""

    sh "open \"#{post_dir}#{filename}\"" if args.content == nil
  else
    puts "A post with the same name already exists. Aborted."
  end
  # puts "You might want to: edit #{$post_dir}#{filename}"
end


desc 'Create a post listing all changes since last deploy'
task :post_changes do |t, args|
  content = list_file_changed
  Rake::Task["create_post"].invoke(Time.new.strftime("%Y-%m-%d %H:%M:%S"), "Recent Changes", nil, content)
end


desc 'Show the file changed since last deploy to stdout'
task :list_changes do |t, args|
  content = list_file_changed
  puts content
end


#
# support functions for generating list of changed files
#

def list_file_changed
  content = "Files changed since last deploy:\n"
  IO.popen('find * -newer _last_deploy.txt -type f') do |io| 
    while (line = io.gets) do
      filename = line.chomp
      if user_visible(filename) then
        content << "* \"#{filename}\":{{site.url}}/#{file_change_ext(filename, ".html")}\n"
      end
    end
  end 
  content
end

# this is the list of files we do not want to show in changed files
EXCLUSION_LIST = [/.*~/, /_.*/, "javascripts?", "js", /stylesheets?/, "css", "Rakefile", "Gemfile", /s[ca]ss/, /.*\.css/, /.*.js/, "bower_components", "config.rb"]

# return true if filename is "visible" to the user (e.g., it is not javascript, css, ...)
def user_visible(filename)
  exclusion_list = Regexp.union(EXCLUSION_LIST)
  not filename.match(exclusion_list)
end 

def file_change_ext(filename, newext)
  if File.extname(filename) == ".textile" or File.extname(filename) == ".md" then
    filename.sub(File.extname(filename), newext)
  else  
    filename
  end
end


desc 'Check links for site already running on localhost:4000'
task :check_links do
  begin
    require 'anemone'

    root = 'http://localhost:4000/'
    puts "Checking links with anemone ... "
    # check-links --no-warnings http://localhost:4000
    Anemone.crawl(root, :discard_page_bodies => true) do |anemone|
      anemone.after_crawl do |pagestore|
        broken_links = Hash.new { |h, k| h[k] = [] }
        pagestore.each_value do |page|
          if page.code != 200
            referrers = pagestore.pages_linking_to(page.url)
            referrers.each do |referrer|
              broken_links[referrer] << page
            end
          else
            puts "OK #{page.url}"
          end
        end
        puts "\n\nLinks with issues: "
        broken_links.each do |referrer, pages|
          puts "#{referrer.url} contains the following broken links:"
          pages.each do |page|
            puts "  HTTP #{page.code} #{page.url}"
          end
        end
      end
    end
    puts "... done!"

  rescue LoadError
    abort 'Install anemone gem: gem install anemone'
  end
end


#
# General support functions
#

# remove generated site
def cleanup
  sh 'rm -rf _site'
  compass('clean')
end

# launch jekyll
def jekyll(directives = '')
  sh 'bundle exec jekyll ' + directives
end

# launch compass
def compass(command = 'compile')
  (sh 'compass ' + command) if $compass
end

# check if there is another rake task running (in addition to this one!)
def rake_running
  `ps | grep 'rake' | grep -v 'grep' | wc -l`.to_i > 1
end

def git_local_diffs
  %x{git diff --name-only} != ""
end

def git_remote_diffs branch
  %x{git fetch}
  %x{git rev-parse #{branch}} != %x{git rev-parse origin/#{branch}}
end

def git_repo?
  %x{git status} != ""
end

def git_requires_attention branch
  $git_check and git_repo? and git_remote_diffs(branch)
end

desc 'Task to be used on travis-ci'
task :travis do

  # if this is a pull request, do a simple build of the site and stop
  if ENV['TRAVIS_PULL_REQUEST'].to_s.to_i > 0
    puts 'Pull request detected. Executing build only.'
    Rake::Task["build"].invoke
    next
  else
    Rake::Task["build"].invoke
    puts "## Deploying website via rsync"
    success = system("sshpass -p $XAMSSH rsync -rvc --delete  --exclude coppermine --stats --exclude update _site/ xam.dk@ssh.xam.dk:/www/jekyll")
  end
  
  fail unless success
end

require 'html-proofer'

task :test do
  sh "bundle exec jekyll build"
  options = { :assume_extension => true,
              :alt_ignore => [/./],
              :url_ignore => [
                /www.goosync.com/,
                /www.operamini.com/,
                /63.209.20.13/,
                /blogs.sun.com/,
                'http://www.eclipse.org/swt/launcher.html',
                'http://labs.jboss.com/tools/',
                'http://blog.xam.dk/archives/68-Eclipse-and-memory-settings.html',
                'http://www.eclipsecon.org/2006/Sub.do?id=30',
                'http://download.jboss.org/jbosstools/builds/nightly/200706241629-nightly/buildResults.html',
                'http://home.worldonline.dk/ninotech/freeutil.htm#pathcopy',
                'https://www.tortoisecvs.org/',
                'http://www.redpill.se/utbildning/index.php?module=index&lang=en',
                'http://www.xyzcomputing.com/index.php?option=content&task=view&id=1002',
                'http://www.blueskyonmars.com/archives/2004/07/31/ibm_to_open_source_cloudscape_database.html',
                'http://www.eclipsecon.org/2006/Sub.do?id=186',
                'http://www.eye.fi/products/prox2',
                'http://www.trcs.com/projects/ippjava.htm',
                'http://developer.java.sun.com/developer/bugParade/bugs/4701198.html',
                'http://developer.java.sun.com/developer/bugParade/bugs/4707777.html',
                'http://www.javapolis.com/confluence/display/JP07/Intro+to+Exadel+RichFaces+and+JBoss+JSFUnit',
                'http://www.javapolis.com/confluence/display/JP07/Seam+in+Action',
                'http://blog.solutionperspectivemedia.co.uk/?p=63',
                'http://jaoo.dk/speakers/index.html',
                'http://sourceforge.net/forum/forum.php?thread_id=847876&forum_id=128638',
                'http://ourcomments.org/Emacs/EmacsW32.html',
                'http://www.research.ibm.com/people/k/koved/Presentations/OReilly2000/indexc.htm',
                'http://labs.jboss.com/jbossrichfaces/',
                'http://labs.jboss.com/rhdevstudio/',
                'http://www.redhat.com/developers/rhds',
                'http://www2.ilog.com/preview/Discovery/',
                'http://blog.emmanuelbernard.com/2007/02/svn-false-promises.html',
                'http://jira.jboss.com/jira/browse/JBIDE',
                /redhat.ats.hrsmart.com/,
                'http://www.google.com/calendar/images/calendar_sm2_en.gif',
                'https://linkedin.com/in/maxrydahlandersen', ## this one exists but linkedin denies automation
              ],
              :file_ignore => [
                /asylum\/.*/
              ],
            }
  HTMLProofer.check_directory("./_site", options).run
end
