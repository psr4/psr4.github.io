### 服务端
```
<?php
$s = stream_socket_server('tcp://0.0.0.0:8888', $errno, $errstr);
while (true) {
    $f = stream_socket_accept($s);
    $content = fread($f, 4096);
	fwrite($f,'i am recived!');
    fclose($f);
    echo 'recived:'.$content.PHP_EOL;
}
```

### 客户端
```
$s = stream_socket_client('tcp://127.0.0.1:8888',$errno,$errstr);
fwrite($s,'text');
$content = fread($s, 4096);
fclose($s);
echo 'return: '.$content;
```