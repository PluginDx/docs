# PluginDx for Magento 1

If you build extensions or themes for Magento 1, PluginDx is like a Swiss Army knife for your technical support toolkit. To get started, **[sign up for PluginDx](https://app.plugindx.com/register)** and create a new integration. Select "Magento 1" as your platform. From there, you'll be given specific instructions on how to add the PluginDx framework to your extension.

Copy and paste the framework code using the setup guide inside the PluginDx app. Afterwards, reference this guide for more information. Platform frameworks are completely open source:

[PluginDx Magento 1 Framework](https://github.com/plugindx/plugindx-magento) (GitHub)

If you run into any technical issues, [send an email](https://plugindx.com/contact) or [open a new issue](https://github.com/plugindx/plugindx-magento/issues/new) for help.

## Overview

PluginDx goes directly inside your Magento 1 extension. Here's a high-level view of the framework:

#### Model/Plugindx/Report.php

Given your integration configuration, this class will automatically transform the JSON with diagnostic report data.

#### controllers/Adminhtml/PlugindxController.php

A backend controller action is added to generate diagnostic reports. For security, this action is only accessible in the Magento admin panel and accessible by admins with access to the `admin/system` resource. Only your customers can directly generate diagnostic reports.

Using the report model above, a diagnostic report is generated using the integration configuration passed to the action:

```php
$generatedReport = $report->build($config);
```

#### etc/config.xml

A route is added to your backend `routes.xml` file to access the controller action:

```xml
<config>
    <admin>
        <routers>
            <adminhtml>
                <args>
                    <modules>
                        <{{PACKAGE}}_{{MODULE}} after="Mage_Adminhtml">{{PACKAGE}}_{{MODULE}}_Adminhtml</{{PACKAGE}}_{{MODULE}}>
                    </modules>
                </args>
            </adminhtml>
        </routers>
    </admin>
</config>
```

#### Helper/Plugindx.php

A helper method named `getReportUrl` is added to generate the controller action URL for AJAX requests:

```php
public function getReportUrl()
{
    return Mage::helper('adminhtml')->getUrl('adminhtml/plugindx/report')
                . '?isAjax=true&form_key='
                . Mage::getSingleton('core/session')->getFormKey();
}
```

#### Template File

Add the PluginDx embed panel snippet to one of your extension's template files:

```html
<script async src="https://cdn.plugindx.com/embed/v1/panel.js"></script>
<div class="plugindx" data-report="<?php echo Mage::helper('plugindx')->getReportUrl() ?>"></div></div>
```

## Diagnostic Reports

You can customize diagnostic reports in PluginDx however you'd like. When creating a new Magento 1 integration, you'll have access to all of Magento 1's core configuration settings and server information provided by `phpinfo()`. If you have settings unique to your extension, you can add them using the JSON editor inside PluginDx.

## Config

Provide Magento 1 core and extension-specific configuration fields in the `config` array. To add a new custom field, provide a `name` and `path` attribute inside a new object:

```javascript
"config": [
    {
        "name": "Customer Email",
        "path": "section/yourextension/email"
    }
]
```

Inside the JSON editor you'll also have access to autocomplete for Magento 1's core configuration settings. Simply press the `CTRL` key and begin typing.

## Collections

You can use the `collections` array to pull down data directly from your customer's database and display it in a table grid inside PluginDx:

```javascript
"collections": [
    {
        "name": "Tax Rates",
        "model": "tax/calculation_rate",
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

By default PluginDx includes a set of useful helpers for Magento 1:

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

If you need access to Magento 1 logs inside `/var/log`, use the `logs` attribute:

```javascript
"logs": [
    {
        "path": "system.log",
        "lines": 250
    }
]
```

You can tail `system.log`, `exception.log`, or custom logs created by your extension. The `path` attribute supports [glob](http://php.net/manual/en/function.glob.php) patterns. Specify the `lines` attribute to tail a specific number of lines. By default, PluginDx will tail 100 lines.

