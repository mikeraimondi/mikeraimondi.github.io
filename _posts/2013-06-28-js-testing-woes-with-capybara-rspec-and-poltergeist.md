---
title: JavaScript testing woes with Capybara, RSpec, and poltergeist
author: infotrope
layout: post
permalink: /2013/06/28/js-testing-woes-with-capybara-rspec-and-poltergeist/
categories:
  - Ruby on Rails
---
I've recently started acceptance testing my JavaScript, and it has been something of a rough ride. Using Capybara/RSpec with the poltergeist JS driver, I kept encountering seemingly random failures which nearly forced me to give up on acceptance testing my JS at all. In the end, I managed to work around the issues:

With the help of an [excellent post][1] by Avdi Grimm, I installed [database_cleaner][2] and configured it to work in the Capybara/poltergeist environment. However, this led to issues where tests didn't seem to be properly cleaning up after themselves.

Thanks to [this reply][3] I tracked down the problem to a bug in RSpec 2.13. **If you are going to use database_cleaner with Capybara/poltergeist, make sure you are using RSpec >= 2.14.0.rc1**. For my Rails app, this is what I put in my `Gemfile`:

    gem 'rspec-rails', "~> 2.14.0.rc1"
    

At this point, I thought I had the problem solved.

*And yet, there was more nondeterminism to come*

Tests were still failing with ActiveRecord `not found` exceptions. Less frequently, but still enough to ruin the utility of my test suite. After much trial and error, I added a single line to the database_cleaner config in the reply above, and it appears to actually *work*. Here's my `database_cleaner.rb`:



Hopefully this helps some folks looking to use Capybara to test their JavaScript

[1]: http://devblog.avdi.org/2012/08/31/configuring-database_cleaner-with-rails-rspec-capybara-and-selenium/
[2]: https://github.com/bmabey/database_cleaner
[3]: http://devblog.avdi.org/2012/08/31/configuring-database_cleaner-with-rails-rspec-capybara-and-selenium/#comment-935743168