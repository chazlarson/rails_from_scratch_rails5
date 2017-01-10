# Unit 11
## Chapter 2: Upgrading Gems

In this chapter, you will upgrade the gems in your app.  The gem upgrading process is done in multiple stages.  Upgrading everything at once complicates any troubleshooting that needs to be done.

### Gem Updating Script
* Create the file upgrade_gems.sh with the following contents:
```
#!/bin/bash

# Use this script for upgrading gems.

# If the version of a gem is pinned in the Gemfile, you must update
# the version number specified in that file.

# Do NOT use this script until you have set up this project with the build_fast.sh script.

echo '-------------'
echo 'bundle update'
bundle update

sh git_check.sh

echo '----------------------------------------------'
echo 'gemsurance --output log/gemsurance_report.html'
gemsurance --output log/gemsurance_report.html
echo 'The Gemsurance Report is at log/gemsurance_report.html .'
```

### Upgrading Gems
* Enter the command "sh upgrade_gems.sh".  This upgrades all gems that can be upgraded without violating the conditions specified by the Gemfile.  The resulting log/gemsurance_report.html file shows which gems are up-to-date, which ones are outdated, and which ones have security issues that require updates.  The gems listed in boldface are listed in the Gemfile, and all other gems are dependencies.
* For each outdated gem listed in the Gemfile (except rails, pg, nokogiri, and any other gem that takes a long time to install), use the procedure below to upgrade.  Continue until you are finished.

### Upgrading An Individual Gem
1. Specify the new version of the gem in the Gemfile.
2. Enter the command "gem update (gem name)".
3. Enter the command "sh git_check.sh" to make sure that all tests pass, and no offenses are found.
4. If all goes well, use the "git add" and "git commit" the changes you just made.
5. If there are any test failures or offenses, make the appropriate changes to the source code and repeat steps 2 and 3.
6. If you are unable to correct the test failures or offenses, repeat steps 1-3 using the old Gem version.  The "git status" command should show no changes.

### Recurring Maintenance Tasks
* Once you have completed the easy gem upgrades, tackle the more difficult ones.
* Gems are constantly updated.  Be sure to check for outdated gems and insecure gems periodically.