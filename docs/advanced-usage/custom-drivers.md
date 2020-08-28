---
title: Custom Drivers
sort: 1
---

You can easily extend settings to use your own drivers for storing and retrieving settings, such as using a json
or xml file. To do so, you will need to add your driver's configuration in the `drivers` key in the `config/settings.php`
config file, with the following minimum configuration:

<x-code lang="php">
'drivers' => [
    // ... other drivers
    'custom' => [
        'driver' => 'custom',
        // driver specific configuration
    ],
],
</x-code>

<x-tip>Replace <strong>custom</strong> with your driver name.</x-tip>

You will then need to tell settings about your custom driver in a service provider:

<x-code lang="php">
app('SettingsFactory')->extend('custom', fn ($app, $config) => new CustomDriver($config));

// You can also set your custom driver as the default driver here, or in the config/settings.php config file:
app('SettingsFactory')->setDefaultDriver('custom');
</x-code>

Any custom drivers you make must implement the `Rawilk\Settings\Contracts\Driver` interface. Here is what
the interface looks like:

<x-code lang="php">
namespace Rawilk\Settings\Contracts;

interface Driver
{
    public function forget($key);

    public function get(string $key, $default = null);

    public function has($key): bool;

    public function set(string $key, $value = null);
}
</x-code>

<x-tip><strong>Note:</strong> Your custom drivers <strong>do not need to handle encryption or caching</strong>; the settings service will handle that for you.</x-tip>