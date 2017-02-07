---

template:         article
reviewed:         2017-02-07
title:            Limitations & Troubleshooting
naviTitle:        Limitations
lead:             Our products come in different sizes. Here we explain what happens when a limit is reached.
category:         platform
stack:            all
---


## PHP memory

**Scope**: Universal and Professional Apps

The PHP component on fortrabbit has a memory limit. This limit depends on the selected scaling and is declared on the specs page ([Universal](https://www.fortrabbit.com/specs) & [Professional](https://www.fortrabbit.com/specs-pro#php)). Once the memory limit is reached, which can happen due to a [multitude of factors](php-pro#toc-php-memory), the application will start to fail. Failing means either a 503 error or a classic "PHP whitescreen". Depending on the type of failure you can expect to find errors in the log. A typical error would be:

```
Fatal error: Allowed memory size of 134217728 bytes exhausted (tried to allocate 54 bytes)
```

With Professional Apps, you can solve this issue by upgrading to a larger vertical scaling, which means: If you are using a `PHP s` scaling, upgrade to a `PHP m` scaling with the same amount of nodes. Respectively, if you are using `PHP m`, then upgrade to a corresponding `PHP l` scaling. Each of those upgrades doubles the available memory. Lastly, if `PHP l` is not sufficient, there are dedicated PHP scalings, which offer even more memory, if required.

The current and historic memory consumption of an App can be viewed in the Dashboard (Dashboard > App > See all metrics). The two relevant metrics are _Memory Usage_ and _Swap Usage_. Since both are hourly average values, you will not be able to detect peak usages, but you can monitor trends. To give you an example: A _Memory Usage_ value of 80-90% of the available memory is a soft indicator, that the memory peak usage is probably beyond the available capacity. Now if the _Swap Usage_ value also shows consistently anything beyond 5-10% of the available memory, this soft factor becomes nearly certainty.

For high level insight, especially regarding peak consumption, we recommend to use tools like [New Relic](new-relic) or [Blackfire](blackfire).


## PHP processes

**Scope**: Universal and Professional Apps

Besides PHP memory, the amount of processes per App are also limited. The general rule is two processes per node. Since Universal Apps run only on one Node, they come with only two processes. The amount of processes for Professional Apps are [available in the specs](specs-pro#php).

The amount of processes divided by the average PHP response time determines how many PHP requests the App can handle. For example, if the App (on average) responds within 100ms then two processes could handle 20 requests per second:

* `2 processes * 1 second % 100 ms = 20 requests per second`
* `8 processes * 1 second % 150 ms = ~53 requests per second`
* `16 processes * 1 second % 50 ms = 320 requests per second`

### Pitfall: Self requests

One commonly seen pitfall are so called "self requests". Those are scenarios in which the App executes a PHP script, which fires an HTTP request to the App itself. A simple PHP example would be:

```php
echo file_get_contents('https://{{app-name}}.frbit.com/navigation');
```

Now if this script can be queried under the URL `https://{{app-name}}.frbit.com/home`, then `/home` makes a self request to `/navigation`.

The danger with using such a technique is producing a [deadlock](https://en.wikipedia.org/wiki/Deadlock): Using the above example imagine an App with two processes on a single node which receives two concurrent requests to `/home`. Both then "internally" generate a new request to `/navigation` and return the output. However, since the App only can handle two concurrent requests with two processes, both of them are locked and wait for the response of the request to `/navigation`. Neither of the requests to `/navigation` can be executed, since both processes are already in use handling the request to `/home`. The App is in a deadlock.


## Object Storage

**Scope**: Professional Apps with Object Storage component