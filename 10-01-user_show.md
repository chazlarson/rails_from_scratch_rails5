# Unit 10
## Chapter 10: User Profile

In this chapter, you will create user profile pages.  In the interest of user privacy, a given user profile will be made available ONLY to that particular user and to admins.  Users will NOT be allowed to view the profiles of other users, and the general public will NOT be allowed to view the profile of any users.

While not all Rails apps require user privacy, it's best to provide it by default in the GenericApp gem.  If an app created with GenericApp does not require user privacy, the user methods can be modified.

### New Branch
Enter the command "git checkout -b 10-01-user_show".

### User Controller Test
* Enter the command "rails generate controller Users new".
* Enter the command "rm app/helpers/users_helper.rb".
* Enter the command "rm app/views/users/new.html.erb".
* Replace the contents of test/controllers/users_controller_test.rb with the following:
```
require 'test_helper'

class UsersControllerTest < ActionController::TestCase
  #############
  # BEGIN: show
  #############
  test 'should redirect profile page when not logged in' do
    get :show, params: { id: @u1 }
    assert_redirected_to root_path
    get :show, params: { id: @u2 }
    assert_redirected_to root_path
    get :show, params: { id: @u3 }
    assert_redirected_to root_path
    get :show, params: { id: @u4 }
    assert_redirected_to root_path
    get :show, params: { id: @u5 }
    assert_redirected_to root_path
    get :show, params: { id: @u6 }
    assert_redirected_to root_path
    get :show, params: { id: @u7 }
    assert_redirected_to root_path
  end

  test 'should redirect users from profiles other than their own' do
    sign_in @u1, scope: :user
    get :show, params: { id: @u2 }
    assert_redirected_to root_path
    get :show, params: { id: @u3 }
    assert_redirected_to root_path
    get :show, params: { id: @u4 }
    assert_redirected_to root_path
    get :show, params: { id: @u5 }
    assert_redirected_to root_path
    get :show, params: { id: @u6 }
    assert_redirected_to root_path
    get :show, params: { id: @u7 }
    assert_redirected_to root_path
  end

  test 'should not redirect users from their own profiles' do
    sign_in @u1, scope: :user
    get :show, params: { id: @u1 }
    assert :success
  end

  test 'should not redirect profile page when logged in as a super admin' do
    sign_in @a1, scope: :admin
    get :show, params: { id: @u1 }
    assert :success
    get :show, params: { id: @u2 }
    assert :success
    get :show, params: { id: @u3 }
    assert :success
    get :show, params: { id: @u4 }
    assert :success
    get :show, params: { id: @u5 }
    assert :success
    get :show, params: { id: @u6 }
    assert :success
    get :show, params: { id: @u7 }
    assert :success
  end

  test 'should not redirect profile page when logged in as a regular admin' do
    sign_in @a4, scope: :admin
    get :show, params: { id: @u1 }
    assert :success
    get :show, params: { id: @u2 }
    assert :success
    get :show, params: { id: @u3 }
    assert :success
    get :show, params: { id: @u4 }
    assert :success
    get :show, params: { id: @u5 }
    assert :success
    get :show, params: { id: @u6 }
    assert :success
    get :show, params: { id: @u7 }
    assert :success
  end
  ################
  # FINISHED: show
  ################
  ##############
  # BEGIN: index
  ##############

  #################
  # FINISHED: index
  #################
  ###############
  # BEGIN: delete
  ###############

  ##################
  # FINISHED: delete
  ##################
end
```
* Enter the command "sh testc.sh".  All five new controller tests fail.  Four of these test failures are due to the undefined method sign_in, and one test failure is due to a missing route.
* Automatically provide Devise test controller helpers (which provide the method sign_in), the setup_universal actions, and the teardown_universal actions to all controller tests. Edit the file test/test_helper.rb and add the following lines before the Capybara setup section:
```
##############################
# BEGIN: controller test setup
##############################
class ActionController::TestCase
  include Devise::Test::ControllerHelpers

  # Execute before each integration test
  def setup
    setup_universal
  end

  # Execute after each integration test
  def teardown
    teardown_universal
  end
end
############################
# END: controller test setup
############################
```
* Enter the command "sh testc.sh".  Now all five new tests fail due to a missing route.
* Update the routing. Edit the file config/routes.rb and add the following line to the end of the user section:
```
  resources :users, only: [:show]
```
* Enter the command "sh testc.sh".  Now all five new tests fail because the "show" action is missing from the user controller.
* Replace the contents of the file app/controllers/users_controller.rb with the following:
```
#
class UsersController < ApplicationController
  ##############################
  # BEGIN: before_action section
  ##############################
  before_action :may_show_user, only: [:show]
  ############################
  # END: before_action section
  ############################

  #######################
  # BEGIN: action section
  #######################
  def show
    @user = User.find(params[:id])
  end
  #####################
  # END: action section
  #####################

  private
  ########################
  # BEGIN: private section
  ########################
  def admin_or_correct_user
    current_user == User.find(params[:id]) || admin_signed_in?
  end
  helper_method :admin_or_correct_user

  def may_show_user
    return redirect_to(root_path) unless admin_or_correct_user
  end
  helper_method :may_show_user
  ######################
  # END: private section
  ######################
end
```

### Wrapping Up
* Enter the command "git push origin 10-01-user_show".
* Go to the GitHub repository and click on the "Compare and pull request" button for this branch.
* Accept this pull request to merge it with the master branch, but do NOT delete this branch.
* Enter the following commands:
```
git checkout master
git pull
sh heroku.sh
```