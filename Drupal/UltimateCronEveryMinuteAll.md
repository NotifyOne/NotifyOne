# Allows you to force the crown to start every minute

WARNING:
- Config directory must be setupped.
- Configuration must be synced.

>\[WEBROOT\]/sites/default/settings.local.php
```
if (!empty($config_directories['sync'])) {
  $cfgs = array_filter(
    scandir($config_directories['sync']),
    function ($value) {
      return str_starts_with($value, 'ultimate_cron');
    }
  );
  // Не показувати Ліду.
  foreach ($cfgs as $value) {
    $c = str_replace('.yml', '', $value);
    $config[$c]['scheduler'] = [
      'id' => 'crontab',
      'configuration' => [
        'rules' => [
          '* * * * *',
        ],
      ],
    ];
  }
}
```
