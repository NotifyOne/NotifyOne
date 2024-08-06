# Disable caching

>\[WEBROOT\]/sites/default/settings.local.php
```
// Disable CSS and JS aggregation.
$config['system.performance']['css']['preprocess'] = FALSE;
$config['system.performance']['js']['preprocess'] = FALSE;
$config['system.performance']['css']['gzip'] = FALSE;
$config['system.performance']['js']['gzip'] = FALSE;
$config['advagg.settings']['enabled'] = FALSE;
```