```
守护进程
<?php
if (pcntl_fork() > 0) {
    echo 'exit!';
    exit();
}
posix_setsid();
chdir('/');
$old = umask(0);
for (; ;) {
    sleep(1);
    file_put_contents('1.txt', 'child', 8);
}
```

```
// 多进程
<?php
$stime = microtime(true);
$max = 10;
$run = 0;
for ($i = 0; $i < 20; $i++) {
	//超过进程数 等待子进程退出
    if ($max <= $run) {
        pcntl_waitpid(-1, $status);
        $run--;
        if ($res == '-1') {
            echo 'wait 出错!';
        }
    }
    $pid = pcntl_fork();
    if ($pid > 0) {
		//主进程
        $run = $run + 1;
    } elseif ($pid == 0) {
		//子进程
        sleep(3);
        echo 'task', $i, '-finish', PHP_EOL;
        exit;
    } else {
        echo 'fork 出错!';
    }
}
//等待子进程全部退出
for (;pcntl_waitpid(-1, $status) != '-1';) {

}
$etime = microtime(true);
echo $etime - $stime;
die();
```