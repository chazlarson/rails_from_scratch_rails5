# Unit 1

## Chapter 4: RuboCop

RuboCop is a Ruby code analyzer that points out violations of the Ruby Style Guide.  Violations don't cause the code to fail but can slow down expert Rubyists when they read and analyze the code.  It's best to add RuboCop very early in a project, because it gets increasingly difficult to introduce later on.

SPECIAL NOTE: The number of RuboCop offenses you get may be different from the number listed here.  The point is to eliminate offenses by fixing the issues cited or by configuring the .rubocop.yml file to ignore them.

### New Branch

Enter the command "git checkout -b 01-04-rubocop".

### Gemfile

* Add the following lines to the end of the Gemfile:
```

# BEGIN: gems used in test_code.sh script
gem 'rubocop', require: false # Code style checking tool; not recommended for legacy apps
```
* Enter the command "bundle install; bundle exec rubocop -D".  This installs and runs RuboCop.  You can see why it's best to add Rubocop early on and why I don't recommend adding it to a legacy app.  Even at this early stage in the project, I found 48 offenses.
* Enter the following commands:
```
sh git_check.sh
git add .
git commit -m "Installed RuboCop"
```
### RuboCop compliance
* Create the file .rubocop.yml with the following content:
```
AllCops:
  Exclude:
    - bin/spring
    - db/migrate/*
    - db/schema.rb

Metrics/LineLength:
  Exclude:
    - config/application.rb
    - config/environments/development.rb
    - config/environments/production.rb
    - config/initializers/assets.rb
    - config/initializers/backtrace_silencers.rb
    - config/initializers/new_framework_defaults.rb
    - config/initializers/session_store.rb
    - config/initializers/wrap_parameters.rb
    - config/environments/production.rb
    - config/environments/test.rb
    - config/puma.rb
    - config/routes.rb
    - db/seeds.rb
    - Gemfile
    - Rakefile
    - test/integration/*
    - test/test_helper.rb

Style/ClassAndModuleChildren:
  Exclude:
    - test/test_helper.rb
```
* The .rubocop.yml file tells RuboCop which violations to ignore in specific files.  Some violations are impossible to avoid or are not worth fixing.  For example, db/schema.rb is automatically generated every time you migrate the database.
* Although there are no files in db/migrate yet, scripts will be automatically generated later.  Editing these scripts to make them compliant with RuboCop is more trouble than it's worth.
* Although config/routes.rb does not violate the Metrics/LineLength cop yet, comments will be automatically generated it by the annotate gem later.  These comments will violate this cop but are too valuable to omit.
* Enter the command "bundle exec rubocop -D" to see how many violations remain.  This .rubocop.yml file reduced the number of offenses to 12.  These remaining violations are relatively easy to fix.
*  In the config/puma.rb file, replace the double-quotes cited by RuboCop with single-quotes.  Enter the command "bundle exec rubocop -D" again.  8 offenses remain.
*  In the config/environments/production.rb file, replace the double-quotes cited by Rubocop, and get rid of the extra spaces it cited.  Entering the command "bundle exec rubocop -D" shows that 5 offenses remain.
*  In the app/helpers/application_helper.rb, app/controllers/application_controller.rb, app/mailers/application_mailer.rb, and app/models/application_record.rb files, add a "#" directly above the "class" statements cited.  Entering the command "bundle exec rubocop -D" shows that 1 offense remains.
* Remove the extra blank lines in the Gemfile that were cited by RuboCop.  Enter the command "bundle exec rubocop -D" again.  There should be no offenses remaining.
* If there are any additional offenses remaining, correct them or configure the .rubocop.yml file to ignore them.
* Enter the command "sh git_check.sh".  Everything should proceed as expected.
* Enter the following commands:
```
git add .
git commit -m "RuboCop compliant"
```
### git_check.sh
* Replace the contents of git_check.sh with the following:
```
#!/bin/bash

# Run this script before entering "git add" and "git commit".

sh build_fast.sh

echo '----------------------'
echo 'bundle exec rubocop -D'
bundle exec rubocop -D

echo '----------'
echo 'git status'
git status
```
* Now you can easily check for RuboCop violations prior to executing the git add and git commit commands.  Enter the command "sh git_check.sh".  There should be no errors, RuboCop offenses, or new files other than the ones explicitly added in this chapter.
* Enter the following commands:
```
git add .
git commit -m "Added rubocop command to git_check.sh"
```
### Wrapping Up
*  Enter the command "git push origin 01-04-rubocop".
*  Go to the GitHub repository and click on the "Compare and pull request" button for this branch.
*  Accept this pull request to merge it with the master branch, but do NOT delete this branch.
*  Enter the following commands:
```
git checkout master
git pull
```

### Conclusions
From now on, be sure to check for RuboCop compliance before entering a "git commit" command.  This is now covered in the git_check.sh script.
