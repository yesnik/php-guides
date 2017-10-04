 # microtime()
 ## Mesure execution time in seconds
 ```php
 $startTime = microtime(true);
 
 // Some code
 sleep(2);
 
 echo 'Task took: ' . round($endTime - $startTime, 3) . ' sec.'
 // 'Task took: 2 sec.'
 ```
 
