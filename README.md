# Welcome to PluginDx

These guides will help you get up and running with the PluginDx framework and embed panel. From there, you can customize your integration's panel and diagnostic reports to fit your needs. You'll be able to create a professional support form directly inside your plugin and instantly get the following data from your customers:

- Plugin, platform, server configuration data
- Encrypted admin credentials
- Data collections from the database
- Installed plugins with versions
- Screenshot uploads
- Tailed log files
- And much more!

**To get started, [sign up for PluginDx](https://app.plugindx.com/register).** Create a new integration and run through the setup process.

## Framework

The PDX framework is a small collection of files that go directly inside your plugin. These files are basically used to make an AJAX request that transforms your integration configuration into a full-fledged diagnostic report based on the given customer's website.

It should take less than 30 minutes to implement this framework in your plugin. The time you spend integrating should be saved within the first couple of days using PluginDx.

To learn more about the PluginDx framework, select your platform:

- [Magento 1](/integrations/magento1.html)
- [Magento 2](/integrations/magento2.html)
- [WordPress](/integrations/wordpress.html)
- [WooCommerce](/integrations/woocommerce.html)

More platforms are on the way. If you have any suggestions, feel free to [get in touch](https://plugindx.com/contact).

## Embed Panel

The PDX embed panel is an HTML / JavaScript snippet that goes inside a template file in your plugin. It typically looks like this:

```html
<script async src="https://cdn.plugindx.com/embed/v1/panel.js"></script>
<div class="plugindx" data-label="Support" data-key="{{API_TOKEN}}" data-platform="magento2" data-report="<?php echo $block->getReportUrl() ?>"></div>
```

This panel allows your customers to voluntarily send you a message and submit a diagnostic report where your plugin lives &mdash; in the platform's admin panel. Once a message is sent, it will arrive in your help desk. You'll click a button inside the email to view the underlying diagnostic report which is hosted on PluginDx.

To learn more about the PluginDx embed panel, check out the [configuration guide](/configuration.html).

