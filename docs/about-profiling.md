---

template:      article
reviewed:      2016-03-10
title:         Profiling PHP on fortrabbit
naviTitle:     Profiling
lead:          What you can do to find performance bottlenecks in your PHP application.

tags:
    - beginner

keywords:
    - performance
    - benchmarking
    - code quality

seeAlsoLinks:
    - app-design
    - new-relic
    - blackfire

---

OK, let's assume that you are have considered our [application design tips](/app-design) already. Now you still want to optimize your application. The solution: profiling. 

One approach would be to clutter your code with `microtime()` logging. Or refresh your browser and keep track of the delivery time. Or you could do righ and use professional Profiling-as-a-Service offerings, like [Blackfire](/blackfire) or [New Relic](/new-relic), which can be easily enabled for your fortrabbit App.


## Further readings

* [Profiling in PHP](https://speakerdeck.com/asarturas/profiling-in-php) a deck by Arturas Smorgun comapring different methods