# pcntl_fork() example

1. Create file `pcntlForkTest.php`:
```php
class PcntlForkTest
{
    public function run()
    {
        echo '=== PID - main: ' . getmypid() . PHP_EOL;
        $this->createFork();
        $this->createFork();

        pcntl_wait($status);
        echo '=== End main' . PHP_EOL;
    }

    private function createFork()
    {
        $pid = pcntl_fork();

        if ($pid === SIG_ERR) {
            throw new Exception('Cannot do process fork');
        }

        if ($pid) {
            // Parent process
            echo 'PID - parent: ' . getmypid() . PHP_EOL;
            echo 'PPID - parent: ' . posix_getppid() . PHP_EOL;

        } else {
            // Child process
            $this->makeWork();
            die();
        }
    }

    private function makeWork() {
        $pid = getmypid();
        echo " > Work start (PID: {$pid})" . PHP_EOL;
        $sum = 0;
        while ($sum < 100000) {
            $sum += 0.001;
        }
        echo " > Work end (PID: {$pid})" . PHP_EOL;
    }
}

// Run script
(new PcntlForkTest())->run();
```

2. Run it in a console:
    ```bash
    php ./pcntlForkTest.php
    ```
    You'll see example output:
    ```
    === PID - main: 6471
    PID - parent: 6471
    PPID - parent: 5885
    PID - parent: 6471
    PPID - parent: 5885
     > Work start (PID: 6473)
     > Work start (PID: 6472)
     > Work end (PID: 6473)
     > Work end (PID: 6472)
    === End main
    ```
    We created 2 separate child processes that do their own work.
