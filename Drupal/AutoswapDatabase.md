# Drupal autochange database by branch

WARNING: 
- file_get_contents() must be enabled

>html/sites/default/settings.local.php
```
// Feature: override the database via branch.
try {
  $db = &$databases['default']['default'];
  $gitHead = file_get_contents('../.git/HEAD');

  if ($gitHead === FALSE) {
    throw new Exception('Not found git head file');
  }

  $branch = '';
  foreach (explode("\n", $gitHead) as $line) {
    [$key, $value] = explode(': ', $line);

    if ($key !== 'ref') {
      continue;
    }
    $value = trim($value);
    $value = explode('/', $value);
    $branch = end($value);
    break;
  }

  if (empty($branch)) {
    throw new Exception('Empty branch, nothing to do.');
  }

  $connect = new PDO("${db['driver']}:host=${db['host']};dbname=$branch", $db['username'], $db['password']);
  $connect->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
  $db['database'] = $branch;
  $config['system.site']['name'] = $branch;
  $config['system.site']['slogan'] = $branch;
}
catch (Throwable $th) {
  // Use default database.
  $config['system.site']['name'] = 'DEFAULT';
  $config['system.site']['slogan'] = 'DEFAULT';
}
unset($db);
```
