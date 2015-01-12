# swot-clj

[![Build Status](https://travis-ci.org/ipavl/swot-clj.svg?branch=master)](https://travis-ci.org/ipavl/swot-clj)

swot-clj is a Clojure port of [Lee Reilly](https://github.com/leereilly)'s [Swot library](https://github.com/leereilly/swot) to validate email addresses and domains of academic institutions, which is described as follows:

> If you have a product or service and offer **academic discounts**, there's a good chance there's some manual component to the approval process. Perhaps `.edu` email addresses are automatically approved because, for the most part at least, they're associated with American post-secondary educational institutions. Perhaps `.ac.uk` email addresses are automatically approved because they're guaranteed to belong to British universities and colleges. Unfortunately, not every country has an education-specific TLD (Top Level Domain) and plenty of schools use `.com` or `.net`.

> Swot is a community-driven or crowdsourced library for verifying that domain names and email addresses are tied to a legitimate university of college - more specifically, an academic institution providing higher education in tertiary, quaternary or any other kind of post-secondary education in any country in the world.

## Installation

If using Leiningen, add the following to your `project.clj` file under the `:dependencies` section:

[![Clojars Project](http://clojars.org/swot-clj/latest-version.svg)](http://clojars.org/swot-clj)

swot-clj also works from the REPL.

## Usage

The two main functions of swot-clj are:

* `is-academic?` to verify an email address or domain as belonging to a legitimate academic institution
* `get-institution-name` to associate a domain with an institution name

Both functions take a single string argument, representing a domain name or an email address.

### Verify email addresses

    (is-academic? "test@stanford.edu") ;; true
    (is-academic? "test@strath.ac.uk") ;; true
    (is-academic? "test@uottawa.ca")   ;; true
    (is-academic? "test@ugr.es")       ;; true
    (is-academic? "test@gmail.com")    ;; false

### Verify domains

    (is-academic? "mit.edu")              ;; true
    (is-academic? "uoguelph.ca")          ;; true
    (is-academic? "https://uwaterloo.ca") ;; true
    (is-academic? "http://google.com")    ;; false

### Find school names

    (get-institution-name "umanitoba.ca")
    ;; => ["University of Manitoba"]
    (get-institution-name "test@uvic.ca")
    ;; => ["University of Victoria"]
    (get-institution-name "dal.ca")
    ;; => ["Dalhousie University"]
    (get-institution-name "notaschool.edu")
    ;; => nil

Please see the [Limitations](#limitations) section for known issues and limitations.

## Documentation

Project documentation is generated by [Codox](https://github.com/weavejester/codox) and can be found [on GitHub Pages](https://ipavl.github.io/swot-clj/doc), or generated locally with:

    lein doc

## Contributing

Pull requests are welcome! The [Limitations](#limitations) section below is a good place to start if you're looking for something that needs to be done.

If adding or changing functionality, please add tests for your feature/change if relevant, and make sure that your changes pass the existing tests. Tests can be run locally with `lein test`, and will also be run via Travis CI when you make a pull request.

To add/edit/remove a school, follow the [same guidelines](https://github.com/leereilly/swot/blob/master/CONTRIBUTING.md) as the main Swot library. You should make changes *to Swot itself*, and then submit a request to update swot-clj once your changes are merged upstream so that the two projects can stay roughly in sync.

## Limitations

Like Swot, swot-clj only gives you a high confidence level - *not a guarantee* - that an email address belongs to an individual that is a current student. You should (occasionally) monitor sign ups to verify registrations, particularly if you see a lot of traffic from certain domains.

Other limitations and known issues, some of which are shared with Swot, include:

* only top-level institution domains are checked, so a `mail.school.edu` or `cs.school.edu` email address will not pass checks, nor will `www.school.edu` (but `school.edu` and `@school.edu` will)
* known institutions must be in the local database to be detected
  * there is a `whitelist.txt` file of known academic TLDs for heuristics, however it is not implemented yet
  * you should handle this prior to passing a string to this library's functions for the time being
* there may be false positives or missing institutions - feel free to [contribute](#contributing)!
* there is no way to tell if an email address belongs to a student, faculty, contractor, club, or alumni
  * emails could be checked for the string `alumni` to cover alumni, however this won't apply to all institutions

## License

Copyright © 2015 ipavl

Distributed under the Eclipse Public License either version 1.0 or (at your option) any later version.
