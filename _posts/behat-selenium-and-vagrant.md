**Don't run Selenium server in your Vagrant box,** run it in your host. On one of my laptops I have a [Vagrant](https://www.vagrantup.com/) box which is a replica of a live [CentOS](https://www.centos.org/) web server (provisioned via [Ansible](https://www.ansible.com/)), and there is where all my code resides and naturally where I run my tests from also, whilst my host is completely clean (14.04 Ubuntu and SublimeText as IDE). Just before getting to the steps, I don't know whether you read already or not the [Behat And Selenium in Vagrant](http://programmingarehard.com/2014/03/17/behat-and-selenium-in-vagrant.html/) article, but it depicts how to run the tests inside Vagrant, by having to install **firefox**, **xvfb** and **headless jre** in your Vagrant box. Well, I don't think it's the right way to do it. ****Leave your box intact**** And the reason is that, unless you run in a complete CLI environment, most certainly is that your host has almost everything you need to run Selenium there (maybe missing JRE). Right. In order to run my [Behat/Mink](http://mink.behat.org/en/latest/) Selenium tests and see the results in my host browser for a [Phalcon PHP](https://phalconphp.com/en/) project, this is what I ended up doing 1\. Download Selenium on my host machine from [their official page](http://www.seleniumhq.org/download/) 2\. Adjust my project's **behat.yml** parameters for Selenium to call to my host:

<pre class="EnlighterJSRAW" data-enlighter-language="null">selenium2:
    wd_host: HOST.IP.HERE.PLEASE:4444/wd/hub
    capabilities:
        browser: firefox
        version: ANY</pre>

so for example, you'd end up with something like:

<pre class="EnlighterJSRAW" data-enlighter-language="null">default:
    autoload:
        '': %paths.base%/tests/features/bootstrap
    extensions:
        Behat\MinkExtension:
            base_url: 'http://local.my-project.com/'
            goutte: ~
            selenium2:
                wd_host: 192.168.1.3:4444/wd/hub
                capabilities:
                    browser: firefox
                    version: ANY
            default_session: goutte
            javascript_session: selenium2
            browser_name: firefox
    suites:
        auth_features:
            paths:    [ %paths.base%/tests/features/auth ]
            contexts: [ AuthContext ]</pre>

3\. Start Selenium server inside your host:

<pre class="EnlighterJSRAW" data-enlighter-language="null">java -jar /path/to/selenium-server-standalone-2.52.0.jar</pre>

4\. Run your tests inside vagrant:

<pre class="EnlighterJSRAW" data-enlighter-language="null">cd /my/project/path && vendor/bin/behat</pre>

5\. See Firefox opening and tests being executed. Remember to tag the scenario you want to test with Selenium, with **@javascript**, eg.:

<pre class="EnlighterJSRAW" data-enlighter-language="generic">@javascript
Scenario: Accessing the registration page and seeing the registration form inputs
    Given I am on "/"
    When I follow "Sign Up"
    And I am on "/sign-up2
    Then I should see a "email" "input" type "text"
    And I should see a "firstName" "input" type "text"
    And I should see a "lastName" "input" type "text"
    And I should see a "password" "input" type "password"
    And I should see a "confirmPassword" "input" type "password"
    And I should see a "howFind" "select"
    And I should see a "termsText" "textfield"
    And I should see a "terms" "input" type "checkbox"
    And I should see a "optOut" "input" type "checkbox"</pre>

  And there you go. No need to overcomplicate your Vagrant box, having to install additional tools, setting up X11 forward, and so on.
