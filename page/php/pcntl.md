```
�ػ�����
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
// �����
<?php
$stime = microtime(true);
$max = 10;
$run = 0;
for ($i = 0; $i < 20; $i++) {
	//���������� �ȴ��ӽ����˳�
    if ($max <= $run) {
        pcntl_waitpid(-1, $status);
        $run--;
        if ($res == '-1') {
            echo 'wait ����!';
        }
    }
    $pid = pcntl_fork();
    if ($pid > 0) {
		//������
        $run = $run + 1;
    } elseif ($pid == 0) {
		//�ӽ���
        sleep(3);
        echo 'task', $i, '-finish', PHP_EOL;
        exit;
    } else {
        echo 'fork ����!';
    }
}
//�ȴ��ӽ���ȫ���˳�
for (;pcntl_waitpid(-1, $status) != '-1';) {

}
$etime = microtime(true);
echo $etime - $stime;
die();
```