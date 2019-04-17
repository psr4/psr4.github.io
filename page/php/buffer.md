```
实时输出
<?php
        ob_implicit_flush();
        header('X-Accel-Buffering: no');
        for ($i = 100; $i > 0; $i--) {
            echo $i . "<br>";
            sleep(1);
        }
```

```
提前输出
<?php
        echo date('Y-m-d H:i:s');
        fastcgi_finish_request();
        ignore_user_abort(true);
        set_time_limit(0);
        sleep(3);
        file_put_contents('uploads/1.txt',date('Y-m-d H:i:s'));

```