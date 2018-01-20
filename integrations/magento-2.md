# PluginDx for Magento 2

If you build extensions or themes for Magento 2, PluginDx is like a Swiss Army knife for your technical support toolkit. To get started, **[sign up for PluginDx](https://app.plugindx.com/register)** and create a new integration. Select "Magento 2" as your platform. From there, you'll be given specific instructions on how to add the PluginDx framework to your extension.

Copy and paste the framework code using the setup guide inside the PluginDx app. Afterwards, reference this guide for more information. Platform frameworks are completely open source:

[PluginDx Magento 2 Framework](https://github.com/plugindx/plugindx-magento2) (GitHub)

If you run into any technical issues, [send an email](https://plugindx.com/contact) or [open a new issue](https://github.com/plugindx/plugindx-magento2/issues/new) for help.

## Overview

PluginDx goes directly inside your extension. Here's a high-level view of the framework:

### Model/Plugindx/Report.php

Given your integration configuration, this class will automatically transform the JSON with diagnostic report data.

### Controller/Adminhtml/Report

A backend controller action is added to generate diagnostic reports. For security, this action is only accessible in the Magento admin panel and accessible by admins with access to the `Magento_Backend::system` resource. Only your customers can directly generate diagnostic reports.

Using the report model above, a diagnostic report is generated using the integration configuration passed to the action:

```php
$generatedReport = $this->reportModel->build($config);
```

### etc/adminhtml/routes.xml

A route is added to your backend `routes.xml` file to access the controller action:

```xml
<router id="admin">
    <route id="plugindx" frontName="plugindx">
        <module name="{{PACKAGE}}_{{MODULE}}" />
    </route>
</router>
```

### Helper/Plugindx.php

A helper method named `getReportUrl` is added to generate the controller action URL for AJAX requests:

```php
public function getReportUrl()
{
    return $this->backendUrl->getUrl('plugindx/report')
        . '?isAjax=true&form_key='
        . $this->formKey->getFormKey();
}
```

### Block Class

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

### Block Template

Finally, the block method above is then available inside the block template file:

```html
<script async src="https://cdn.plugindx.com/embed/v1/panel.js"></script>
<div class="plugindx" data-report="<?php echo $block->getReportUrl() ?>"></div>
```

## Customization

Beyond your typical extension configuration button, PluginDx also supports main menu buttons and setting menu links in Magento 2.

## Diagnostic Reports