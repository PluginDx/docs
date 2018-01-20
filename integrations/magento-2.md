# PluginDx for Magento 2

If you build extensions or themes for Magento 2, PluginDx is like a Swiss Army knife for your technical support toolkit. To get started, **[sign up for PluginDx](https://app.plugindx.com/register)** and create a new integration. Select "Magento 2" as your platform. From there, you'll be given specific instructions on how to add the PluginDx framework to your extension.

Copy and paste the framework code using the setup guide inside the PluginDx app. Afterwards, reference this guide for more information. Platform frameworks are completely open source:

[PluginDx Magento 2 Framework](https://github.com/plugindx/plugindx-magento2) (GitHub)

If you run into any technical issues, [send an email](https://plugindx.com/contact) or [open a new issue](https://github.com/plugindx/plugindx-magento2/issues/new) for help.

## Overview

PluginDx goes directly inside your extension. Here's a high-level view of the framework:

#### Model/Plugindx/Report.php

Given your integration configuration, this class will automatically transform the JSON with diagnostic report data.

#### Controller/Adminhtml/Report

A backend controller action is added to generate diagnostic reports. For security, this action is only accessible in the Magento admin panel and accessible by admins with access to the `Magento_Backend::system` resource. Only your customers can directly generate diagnostic reports.

Using the report model above, a diagnostic report is generated using the integration configuration passed to the action:

```php
$generatedReport = $this->reportModel->build($config);
```

#### etc/adminhtml/routes.xml

A route is added to your backend `routes.xml` file to access the controller action:

```xml
<router id="admin">
    <route id="plugindx" frontName="plugindx">
        <module name="{{PACKAGE}}_{{MODULE}}" />
    </route>
</router>
```

#### Helper/Plugindx.php

A helper method named `getReportUrl` is added to generate the controller action URL for AJAX requests:

```php
public function getReportUrl()
{
    return $this->backendUrl->getUrl('plugindx/report')
        . '?isAjax=true&form_key='
        . $this->formKey->getFormKey();
}
```

#### Block Class

The helper method above can then be used inside a block class:

```php
<?php
namespace {{PACKAGE}}\{{MODULE}}\Block\Adminhtml;

protected $helper;

class YourBlock
{
    public function __construct(
        \{{PACKAGE}}\{{MODULE}}\Helper\Plugindx $helper
    ) {
        $this->helper = $helper;
    }

    public function getReportUrl()
    {
        return $this->helper->getReportUrl();
    }
}

```

#### Block Template

Finally, the block method above is then available inside the block template file:

```html
<script async src="https://cdn.plugindx.com/embed/v1/panel.js"></script>
<div class="plugindx" data-report="<?php echo $block->getReportUrl() ?>"></div>
```

## Diagnostic Reports

You can customize diagnostic reports in PluginDx however you like to fit your requirements. When creating a new Magento 2 integration, you'll have access to all of Magento 2's core configuration settings and server information provided by `phpinfo()`. If you have settings unique to your extension, you can add them using the JSON editor inside PluginDx.

## Config

Store Magento 2 core and extension-specific configuration fields in the `config` array. To add a new custom field, provide a `name` and `path` attribute inside a new object:

```javascript
"config": [
    {
        "name": "Customer Email",
        "path": "section/yourextension/email"
    }
]
```

Inside the JSON editor you'll also have access to autocomplete for Magento 2's core configuration settings. Simply press the `CTRL` key and begin typing.

## Collections

You can use the `collections` array to pull down data directly from your customer's database and display it in a table grid inside PluginDx:

```javascript
"collections": [
    {
        "name": "Tax Rates",
        "model": "Magento\\Tax\\Model\\Calculation\\Rate",
        "attributes": [
            "tax_country_id",
            "tax_region_id",
            "tax_postcode",
            "code",
            "rate",
            "zip_is_range",
            "zip_from",
            "zip_to"
        ]
    }
]
```

## Helpers

By default PluginDx includes a set of useful helpers for Magento 2:

```javascript
"helpers": [
    {
        "name": "Magento Edition",
        "path": "magento/edition"
    },
    {
        "name": "Magento Locale",
        "path": "magento/locale"
    },
    {
        "name": "Magento Version",
        "path": "magento/version"
    },
    {
        "name": "Modules Installed",
        "path": "magento/modules"
    },
    {
        "name": "Module Version",
        "path": "magento/module_version"
    },
    {
        "name": "Magento Applied Patches",
        "path": "magento/applied_patches"
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

If you need access to Magento 2 logs inside `/var/log`, use the `logs` attribute:

```javascript
"logs": [
    {
        "path": "system.log",
        "lines": 250
    }
]
```

You can tail `system.log`, `exception.log`, or custom logs created by your extension. Specify the `lines` attribute to tail a specific number of lines. By default, PluginDx will tail 100 lines.

