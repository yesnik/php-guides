# Fibers

[Fibers](https://www.php.net/manual/en/language.fibers.php) represent full-stack, interruptible functions. Fibers may be suspended from anywhere in the call-stack, pausing execution within the fiber until the fiber is resumed at a later time.

Suppose that we want to send three HTTP requests and process their combined result. 
The synchronous way of doing so is by sending the first one, waiting for the response, then sending the second one, waiting, etc.

In the world of parallel processing, we send the request but don't wait. Then we send the next request, followed by another. Only then do we wait for all requests. And while waiting we periodically check whether one of our requests is already finished. If that's the case we can process it immediately.

We would create one fiber for each request, and pause the fiber after the request is sent. After we've created all 3 fibers, we'd loop over them, and resume them one by one. By doing so, the fiber checks whether the request is already finished, if not it pauses again, otherwise it can process the response and eventually finish.

Fibers are a mechanism to start, pause and resume the execution flow of an isolated part of your program. Fibers are also called "green threads": threads that actually live in the same process. Those threads aren't managed by the operating system, but rather the runtime — the PHP runtime in our case. They are a cost efficient way of managing some forms of parallel programming.

But note how they don't add anything truly asynchronous: all fibers live in the same PHP process, and only one can run at a time. It's the main process that loops over them and checks them while waiting, and that loop is often called the "event loop".

We're encouraged to use one of the existing async frameworks: Amp or ReactPHP.

With ReactPHP, for example, we could write something like this:

```php
$loop = React\EventLoop\Factory::create();

$browser = new Clue\React\Buzz\Browser($loop);

$promises = [
    $browser->get('https://example.com/1'),
    $browser->get('https://example.com/2'),
    $browser->get('https://example.com/3'),
];

$responses = Block\awaitAll($promises, $loop);
```

Application developers shouldn't need to worry about fibers, it's an implementation detail for frameworks like Amp or ReactPHP.
