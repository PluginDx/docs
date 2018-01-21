# PluginDx for WordPress

If you build plugins or themes for WordPress, PluginDx is like a Swiss Army knife for your technical support toolkit. To get started, **[sign up for PluginDx](https://app.plugindx.com/register)** and create a new integration. Select "WordPress" as your platform. From there, you'll be given specific instructions on how to add the PluginDx framework to your plugin.

Copy and paste the framework code using the setup guide inside the PluginDx app. Afterwards, reference this guide for more information. Platform frameworks are completely open source:

[PluginDx WordPress Framework](https://github.com/plugindx/plugindx-wordpress) (GitHub)

If you run into any technical issues, [send an email](https://plugindx.com/contact) or [open a new issue](https://github.com/plugindx/plugindx-wordpress/issues/new) for help.

## Overview

PluginDx goes directly inside your WordPress plugin. Here's a high-level view of the framework:

#### includes/class-plugindx.php

Inside this file you'll find a `wp_ajax` [action hook](https://codex.wordpress.org/Plugin_API/Action_Reference/wp_ajax_\(action\)) to generate a new diagnostic report:

```php
add_action( 'wp_ajax_plugindx_report', 'plugindx_report' );
```

For security, this WordPress action is only accessible to administrators.

Given your integration configuration, the `PluginDx_Report` class will automatically transform the JSON with diagnostic report data:

```php
$report = new PluginDx_Report( $config );
```

#### Main Plugin File

Make sure you require `class-plugindx.php` in your main plugin file, e.g. `plugin-name.php`:

```php
require_once( 'includes/class-plugindx.php' );
```

#### Embed Panel

Include the embed panel directly in an admin template:

```html
<script async src="https://cdn.plugindx.com/embed/v1/panel.js"></script>
<div class="plugindx" data-report="<?php echo admin_url('admin-ajax.php') ?>"></div>
```

If needed, enqueue the embed panel JavaScript `panel.js` somewhere in your plugin instead:

```php
wp_enqueue_script( 'plugindx-embed', 'https://cdn.plugindx.com/embed/v1/panel.js', array(), false, true );
```

## Diagnostic Reports

You can customize diagnostic reports in PluginDx however you'd like. When creating a new WordPress integration, you'll have access to all WordPress core configuration settings and server information provided by `phpinfo()`. If you have settings unique to your plugin, you can add them using the JSON editor inside PluginDx.

## Config

Provide WordPress core and plugin-specific configuration fields in the `config` array. To add a new custom field, provide a `name` and `path` attribute inside a new object:

```javascript
"config": [
  {
    "name": "Site Title",
    "path": "blogname"
  }
]
```

Inside the JSON editor you'll also have access to autocomplete for WordPress core configuration settings. Simply press the `CTRL` key and begin typing.

## Collections

You can use the `collections` array to pull down data directly from your customer's database and display it in a table grid inside PluginDx:

```javascript
"collections": [
  {
    "model": "terms",
    "attributes": [
      "name",
      "slug"
    ]
  }
]
```

## Helpers

By default PluginDx includes a set of useful helpers for WordPress:

```javascript
"helpers": [
    {
      "name": "WordPress Version",
      "path": "wordpress/version"
    },
    {
      "name": "Plugins Installed",
      "path": "wordpress/plugins"
    },
    {
      "name": "Plugin Data",
      "path": "wordpress/plugin_data"
    },
    {
      "name": "Plugin Version",
      "path": "wordpress/plugin_version"
    },
    {
      "name": "Multisite",
      "path": "wordpress/multisite"
    },
    {
      "name": "Debug Mode",
      "path": "wordpress/debug_mode"
    },
    {
      "name": "Cron",
      "path": "wordpress/cron"
    },
    {
      "name": "Language",
      "path": "wordpress/language"
    },
    {
      "name": "Theme Name",
      "path": "wordpress/theme_name"
    },
    {
      "name": "Theme Version",
      "path": "wordpress/theme_version"
    },
    {
      "name": "Theme Author URL",
      "path": "wordpress/theme_author_url"
    },
    {
      "name": "Child Theme",
      "path": "wordpress/child_theme"
    },
    {
      "name": "Parent Theme Name",
      "path": "wordpress/parent_theme_name"
    },
    {
      "name": "Parent Theme Version",
      "path": "wordpress/parent_theme_version"
    },
    {
      "name": "Parent Theme Author URL",
      "path": "wordpress/parent_theme_author_url"
    }
]
```

## Server

Gather server-specific configuration data and specs using the `server` attribute in your configuration JSON. This structure looks similar to the `config` attribute:

```javascript
"server": [
  {
    "name": "PHP Version",
    "path": "Core/PHP Version"
  }
]
```

Use the autocomplete dropdown in the JSON editor to quickly add server configuration fields.

## Logs

If you need access to WordPress logs inside `/wp-content`, use the `logs` attribute:

```javascript
"logs": [
  {
    "path": "debug.log",
    "lines": 250
  }
]
```

You can tail `debug.log` or custom logs created by your plugin. The `path` attribute supports [glob](http://php.net/manual/en/function.glob.php) patterns. Specify the `lines` attribute to tail a specific number of lines. By default, PluginDx will tail 100 lines.