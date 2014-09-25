<!-- Begin assets -->
<link href="./assets/style.css" rel="stylesheet"></link>
<!-- End assets -->

# The Future of Drupal Functional Testing

---

## About the speakers

---

## Nick Schuch

![Nick](./assets/nick.jpg "Nick")

---

## Cameron Zemek

![Grom](./assets/grom.png "Grom")

* Twitter/Drupal: grom358

---

## Questions

Who has written a test using..........?

---

## Thanks to the community

![Austin](./assets/austin.jpg "Austin")

Note:
* Thank simpletest, wouldn't have Drupal 8 without it
* Thanks everyone for coming so we can take Drupal testing forwards

---

## What is functional testing

![Unit testing](./assets/functional.jpg "Unit testing")

Note:
* Full stack testing
* Smoke, Sanity, Regression, Usability, High level

---

## The state of Drupal testing

![Fork](./assets/fork.jpg "Fork")

Note:
* Long ago forked from simpletest (Dries committed it on 21/04/2008)
* Simpletest last release was 23/01/2012
* Maintainers have to maintain this legacy testing system
* PHPUnit already adopted for Unit tests
* KernelTestBase -> Base class for integration tests, can access files and the database, but the entire environment is initially empty.
* WebTestBase. cURL + SimpleXML + custom code

---

## The state of Drupal testing - Cont...

![Flow](./assets/flow.jpg "Flow")

Note:
* run-tests.sh
* WebTestBase::setUp() creates test site using Drupal Multisite
* WebTestBase using cURL + SimpleXML + custom code connects to test site. Secured via User-Agent string

---

## Time to get off the island

![Island](./assets/island.jpg "Island")

Note:
* Follow our current methodologies in core
* BrowserTestBase -> PHPUnit + Mink + custom code

---

## PHPUnit

![Runner](./assets/runner.jpg "Runner")

Note:
* PHPUnit is the standard; most frameworks use it (like Zend Framework (1&2), Cake, Agavi, even Symfony is dropping their own Framework in Symfony 2 for phpunit).
* Already using it in Drupal 8 with unit tests.
* PHPUnit handles running tests, reporting and code coverage.

---

## PHPUnit example

```
<?php
class StackTest extends PHPUnit_Framework_TestCase {
  public function testPushAndPop() {
    $stack = array();
    $this->assertEquals(0, count($stack));

    array_push($stack, 'foo');
    $this->assertEquals('foo', $stack[count($stack) - 1]);
    $this->assertEquals(1, count($stack));

    $this->assertEquals('foo', array_pop($stack));
    $this->assertEquals(0, count($stack));
  }
}
```

---

## PHPUnit - Contrib

![Nyan](./assets/nyan.gif "Nyan")

---

## PHPUnit - Contrib for real

* **NSinopoli/VisualPHPUnit** - _A PHPUnit GUI_
* **sebastianbergmann/php-timer** - _Utility class for timing_
* **brianium/paratest** - _Parallel testing for PHPUnit_

Note:
* Pull from upstream
* Push our own projects

---

## Recognise this syntax

```
Scenario: The content page exists
  Given I am logged in as a user with the "administrator" role
  When I go to "admin/content"
  Then the response status code should be 200
```

Note: Actually powered by Mink

---

## Mink

![Bricks](./assets/bricks.jpg "Bricks")

Note:
* Two types of browser emulators:
** Headless browser emulators (ie. Goutte)
** Browser controllers (ie. Selenium2)
* Headless browsers send a real HTTP requests against an application and parse the response content. Advantages are simplicity, speed and ability to run it without the need in real browser. But this type of browsers have one big disadvantage - they have no JS/AJAX support. So, you can't test your rich GUI web applications with headless browsers.
* Browser controllers simulate user interactions on browser and are able to retrieve actual information from current browser page. Selenium and Sahi are two most famous browser controllers. The main advantage of browser controllers usage is the support for JS/AJAX interactions on page. Disadvantage is that such browser emulators require installed browser, extra configuration are usually much slower than headless counterparts.
* If you choose headless browser emulator - you'll not be able to test your JS/AJAX pages. And if you choose browser controller - your overall test suite will become very slow at some point. So, in real world we should use both! And that's why you need a Mink.
* Mink does the heavy lifting. Remove cURL and form handling code.

---

## Mink example

```
<?php
$driver = new \Behat\Mink\Driver\GoutteDriver();
$session = new \Behat\Mink\Session($driver);
$session->start();
$session->visit('http://localhost:8080');
$page = $session->getPage();
$el = $page->find('css', '#test');
echo $el->getText(), $el->getAttribute('href'), PHP_EOL;
$session->clickLink('Contact Us');
$session->fillField('name', 'John');
$session->fillField('message', 'Hello Amsterdam');
$session->pressButton('Email');
$session->stop();
```

---

## Did someone say JavaScript?

![Javascript](./assets/java.jpg "Javascript")

Note:
* Mouse manipulations: click, doubleClick, rightClick, mouseOver, focus, blur
* Drag & Drop: dragTo
* Not solution to javascript unit testing, but enable full stack testing that includes dynamic frontend
* Handle pages with AJAX

---

## PhantomJS

![Phantom](./assets/phantom.jpg "Phantom")

Note:
* PhantomJS is a headless WebKit scriptable with a JavaScript API. It has fast and native support for various web standards: DOM handling, CSS selector, JSON, Canvas, and SVG.
* Packaged in a binary.

---

## BrowserTestBase

* How it came to be https://www.drupal.org/node/2232861
* Try to port WebTestBase to Mink https://www.drupal.org/node/2274167#comment-8916585
* Mink for browser + PHPUnit for asserts.

Note:
* Relied on decorator and backwards compatibility layer
* Too many edge cases
* New plan of attack, start fresh

---

## Layers

![I can have my cake and eat it too](./assets/cake.png "Layers")

Note:
* Massive reduction in code in the Browser and Runner layers
* Can use real browsers in the browser layer
* 1000+ lines of custom code implementing a Browser. Replaced with using Mink library.

---

## Show me the code

```
<?php
namespace Drupal\simpletest\Tests;

use Drupal\simpletest\BrowserTestBase;

class BrowserTestBaseTest extends BrowserTestBase {
  function testGoTo() {
    // Go to the front page and make sure we can see some text.
    $this->drupalGet('');
    $this->assertPageTextContains("Enter your Drupal username.");
  }
}
```

---

## Recommendations

* Replace the Runner/Framework with PHPUnit
* Replace the "Browser" with Mink
* Agree on a Javascript driver for core. Then we can start building the "driver" functionality. (Please come to the BOF!)
* Commit along side existing test suite. Start converting AJAX tests in core.

---

## Blockers

* Testing infrastructure.
 * Not easy to swap out run-tests.sh with phpunit
 * Hard to setup PhantomJS on testbot
* Please help those guys out! #drupal-infrastructure

---

## How do we get this into Drupal 8 core

* Quickstart - https://github.com/nickschuch/drupal-phpunit-mink
* D.O Issue - https://www.drupal.org/node/2232861

---

## Checkout

* Modernizing testbot: The future of drupal.org automated testing (Wednesday 10:45-11:45)
* Automated Frontend testing (Wednesday 14:15-15:15)
* Doing behaviour driven development with behat (Wednesday 15:45-16:45)

---

## BOF

* **Time**: 2:15PM - 3:15PM
* **Room**: G001 · Lingotek

---

## Conversation time!

![Conversation](./assets/conversation.jpg "Conversation")
