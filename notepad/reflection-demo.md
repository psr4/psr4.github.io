```
<?php
$router = $_GET['s'];
$html_suffix = '.html';
list($module, $class, $method) = explode('/', strtolower(substr($router, 0, -strlen($html_suffix))));
$ControllerFile = '/app/' . $module . '/controller/' . ucfirst($class) . '.php';
is_file($ControllerFile) && require($ControllerFile);

if (class_exists($class)) {
    $instance = invokeClass($class, $_GET);
    var_dump(invokeMethod([$instance, $method], $_GET));

}

function invokeMethod($method, $vars)
{
    try {
        if (is_array($method)) {
            $class = $method[0];
            $reflection = new \ReflectionMethod($class, $method[1]);
        } else {
            $reflection = new \ReflectionMethod($method);
        }

        $args = getParameters($reflection, $vars);
        return $reflection->invokeArgs(isset($class) ? $class : null, $args);
    } catch (\ReflectionException $e) {
        exit ($e->getMessage());
    }
}

function invokeClass($class, $vars = [])
{
    try {
        $reflection = new \ReflectionClass($class);
        $constructor = $reflection->getConstructor();
        $args = $constructor ? getParameters($constructor, $vars) : [];
        return $reflection->newInstanceArgs($args);
    } catch (\ReflectionException $e) {
        exit ($e->getMessage());
    }
}

function getParameters(\ReflectionMethod $reflection, $vars = [])
{
    try {
        $parameters = $reflection->getParameters();
        $args = [];
        foreach ($parameters as $param) {
            if (!isset($vars[$param->name])) {
                if ($param->isDefaultValueAvailable()) {
                    $result = $param->getDefaultValue();
                } elseif ($param->getClass() === null) {
                    throw new \Exception ('不存在该参数:' . $param->name);
                } else {
                    $result = invokeClass($param->getClass()->getName(), $vars);
                }
                $args[] = $result;
            } else {
                $args[] = $vars[$param->name];
            }
        }
        return $args;
    } catch (\Exception $e) {
        exit ($e->getMessage());
    }
}

```