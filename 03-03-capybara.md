# Unit 3
## Chapter 3: Capybara

Capybara is an acceptance test framework for web applications.  It allows you to create integration tests that provide a better simulation of the user experience than the integration tests in the Rails Tutorial Sample App. 

### New Branch
Enter the command "git checkout -b 03-03-capybara".

### Gemfile
* Add the following lines to the end of the Gemfile:
```
# BEGIN: Capybara (for integration testing)
group :test do
  gem 'capybara'
  gem 'capybara-email'
  gem 'capybara-slow_finder_errors'
end
# END: Capybara (for integration testing)
```
* The capybara-email gem will allow you to test the process of email confirmations for admin and user authentication.
* The capybara-slow_finder_errors gem throws an error when a Capybara process times out.  This flags bottlenecks in the testing process that slow it down and increase the temptation to skip it.
* Enter the command "sh git_check.sh".
* Enter the following commands:
```
git add .
git commit -m "Installed Capybara gems"
```

### Setting Up Capybara
* Add the following code to the end of the test/test_helper.rb file:
```
#######################
# BEGIN: Capybara setup
#######################
require 'capybara/rails'
require 'capybara/email'

class ActionDispatch::IntegrationTest
  # Make the Capybara DSL available in all integration tests
  include Capybara::DSL
  include Capybara::Email::DSL

  # Reset sessions and driver after each test
  def teardown
    teardown_universal
  end
end
#######################
# END: Capybara setup
#######################

def teardown_universal
  Capybara.reset_sessions!
  Capybara.use_default_driver
end

```
* Enter the command "sh git_check.sh".
* Enter the following commands:
```
git add .
git commit -m "Configured test/test_helper.rb to use Capybara"
```

### Adding Capybara Tests
* Enter the command "rails generate integration_test test3".
* Edit the file test/integration/test3_test.rb and give it the following content:
```
require 'test_helper'

class Test3Test < ActionDispatch::IntegrationTest
  test 'home page has expected content' do
    visit 'index.html'
    assert page.has_text?('Welcome to public/index.html!')
    assert page.has_text?("There's Ruby on Racetracks")
    assert page.has_text?("and there's Not Exactly.")
    assert page.has_text?('Make sure you choose the correct one.')
  end

  test 'about page has expected content' do
    visit 'about.html'
    assert page.has_text?('Welcome to public/about.html!')
    assert page.has_text?('Not using Ruby on Racetracks is like')
    assert page.has_text?('being in a Hertz commercial')
    assert page.has_text?('and renting from Not Exactly Hertz.')
  end

  test 'home page has link to about page' do
    visit 'index.html'
    assert page.has_link?('About', href: 'about.html')
    click_on 'About'
    assert page.has_text?('Welcome to public/about.html!')
  end
end
```
* Enter the command "rails test".  You'll see that all three of the new tests fail.
* Enter the following command:
```
alias test1='(command provided in test results with the TESTOPTS part omitted)'
```
* Enter the command "test1".
* Edit the file public/about.html and replace it with the following content:
```
<html>
<header>
    <title>About</title>
</header>
<body>
    <h1>About</h1>
    Welcome to public/about.html!
    <br>
    Not using Ruby on Racetracks is like being in a Hertz commercial
    and renting from Not Exactly Hertz.
</body>
</html>
```
* Enter the command "test1".  Now only 2 of the tests fail.
* Edit the file public/index.html and replace it with the following content:
```
<html>
    <head>
        <title>Home</title>
    </head>
    <body>
    <h1>Home</h1>   
    Welcome to public/index.html!
    <br>
    There's Ruby on Racetracks, and there's Not Exactly.  Make sure you
    choose the correct one.
    <br>
    <a href='about.html'>About</a>
    </body>
</html>
```
* Enter the command "test1".  Now all of the tests in test/integration/test3_test.rb pass.
* Enter the command "sh git_check.sh".  Everything should be ready for git commit.
* Enter the following commands:
```
git add .
git commit -m "Added successful Capybara test"
```
### Wrapping Up
* Enter the command "git push origin 03-03-capybara".
* Go to the GitHub repository and click on the "Compare and pull request" button for this branch.
* Accept this pull request to merge it with the master branch, but do NOT delete this branch.
* Enter the following commands:
```
git checkout master
git pull
sh heroku.sh
```
