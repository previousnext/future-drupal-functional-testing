<!-- Begin assets -->
<link href="./assets/style.css" rel="stylesheet"></link>
<!-- End assets -->

# The Future of Drupal Functional Testing

---

## About the speakers

---

## Nick Schuch

---

## Cameron Zemek

* PHP developer for 10 years
* Drupal/Twitter: grom358

---

## Thanks to the community

![Austin](./assets/austin.jpg "Austin")

Note:
* Thank simpletest, wouldn't have Drupal 8 without it
* Thanks everyone for coming so we can take Drupal testing forwards

---

## The state of Drupal testing

![Fork](./assets/fork.jpg "Fork")

Note:
* Long ago forked from simpletest
* Simpletest last release was 23/01/2012
* Maintainers have to maintain this legacy testing system
* PHPUnit already adopted for Unit tests
* KernelTestBase -> Base class for integration tests, can access files and the database, but the entire environment is initially empty.
* WebTestBase. cURL + SimpleXML + custom code

---

## Not a replacement for unit testing

![Unit testing](./assets/unit.jpg "Unit testing")

Note:
* 

---

## Time to get smarter about our tests

![Levels](./assets/levels.jpg "Levels")

Unit -> DB -> HTTP -> Functional!!!

---

## Time to get off the island

![Island](./assets/island.jpg "Island")

Note:
* BrowserTestBase -> PHPUnit + Mink + custom code

---

## PHPUnit

![Runner](./assets/runner.jpg "Runner")

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

---

## Did someone say JavaScript?

![Javascript](./assets/java.jpg "Javascript")

---

## PhantomJS

![Phantom](./assets/phantom.jpg "Phantom")

---

## Show me the code

WE NEED TO WRITE SOME CODE!

---

## How do we get this into Drupal 8 core

Link to patch

---

## Checkout

* Modernizing testbot: The future of drupal.org automated testing (Wednesday 10:45-11:45)
* Automated Frontend testing (Wednesday 14:15-15:15)
* Doing behaviour driven development with behat (Wednesday 15:45-16:45)

---

## Question time!
