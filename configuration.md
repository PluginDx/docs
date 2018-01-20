# Configuration

The PluginDx embed panel can be customized in a variety of ways. All customizations are applied before the panel is rendered and happens only once during the page lifecycle. You can configure the panel using data attributes or a JavaScript object.

## Getting Started

After creating a new integration in PluginDx, you'll be asked to include an embed snippet directly inside your plugin. Where this goes is dependent on the platform you're using. Most of the time it will look similar to this:

```html
<script async src="https://cdn.plugindx.com/embed/v1/panel.js"></script>
<div class="plugindx" data-label="Support" data-key="YOUR INTEGRATION KEY" data-platform="YOUR PLATFORM" data-report="REPORT AJAX URL"></div>
```

For more advanced customization such as [translations](#translation-attributes) and custom targets, you may prefer using a JavaScript object:

```html
<script>
    var pdxConfig = {
        label: 'Support',
        key: 'YOUR INTEGRATION KEY',
        platform: 'YOUR PLATFORM',
        report: 'REPORT AJAX URL',
        type: 'custom-target',
        target: '.plugindx',
        translations: {
            message: {
                title: 'Contact Us'
            }
        }
    };
</script>
<script async src="https://cdn.plugindx.com/embed/v1/panel.js"></script>
```

### Integration Key

Your **public** integration `key` is unique to your integration and used for several things:

- Pulling your report and integration configuration from PluginDx to generate diagnostic reports.
- Making API requests to PluginDx to send messages.
- Dispatching platform events unique to your integration for custom data.

This key is automatically generated after creating a new integration and will persist indefinitely to ensure all of your users can use PluginDx.

## Attributes

| Name | Type | Default | Description |
|------------------|----------|---------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| autoInit | Boolean | true | Automatically initialize and mount the PluginDx embed panel. If set to `false`, requires the `init` method to [initialize programmatically](/javascript-api.html).
| color | String | '#0275d8' | Primary color of the embed panel, mainly used for button and link styling. Supports any CSS color value including keywords and hex values. [Learn more.](/styling.html) |
| email | String | '' | Email address shown as a fallback if an error is encountered or PluginDx is unavailable. [Learn more.](#fallback) |
| key | String | '' | Your public integration key for PluginDx. |
| label | String | 'Support' | Label of the support button shown to toggle the PluginDx embed panel if `type` is 'button'. |
| messageFields | Array | ['name', 'email', 'subject', 'message', 'secure', 'screenshots', 'diagnostics'] | Fields shown in the message form. Default value includes all available fields. Email and message fields are required and always shown to the user. |
| onReady | Function | () => {} | Execute custom JavaScript code after the PluginDx embed panel is initialized and fully rendered. |
| overlayIcon | String | 'question-mark' | Icon shown in a circle overlay button if `type` is 'circle-overlay'. Icons include 'question-mark', 'email', and 'livesaver'. |
| overlayPlacement | String | 'bottom-right' | Placement of a circle overlay button if `type` is 'circle-overlay'. Options include 'bottom-right', 'bottom-left', 'top-left', and 'top-right'. |
| platform | String | 'other' | The platform of your integration. Used for platform-specific behavior and styling. |
| report | String | '' | Dynamic report URL used to generate diagnostic reports in the platform-specific PluginDx framework. |
| target | String | '' | Custom CSS selector to open the PluginDx embed panel from a specific DOM element rather than a support button if `type` is 'custom-target'. |
| translations | Object | {} | Re-label or translate text inside the PluginDx embed panel. [Learn more.](#translation-attributes) |
| type | String | 'button' | Type of embed element to toggle the PluginDx embed panel. Options include 'button', 'circle-overlay', and 'custom-target'. |

## Translation Attributes

To re-label or translate text in the PluginDx embed panel, use the `translations` attribute with a JavaScript object. Translations are grouped by panel view:

| View | Description |
|-------------|-----------------------------------------------------------------------------|
| message | Message form used to submit diagnostic reports in the PluginDx embed panel. |
| messageSent | Confirmation screen informing a user the message was sent. |
| error | Error screen for HTTP error responses. |

Each view has a unique set of translations. After referring to the view you'd like to update, provide any attributes based on the default structure:

```javascript
translations: {
    message: {
        title: 'Contact Us',
        fields: {
            message: {
                placeholder: 'How can we help? Please provide specific examples and order IDs.'
            }
        }
    }
}
```

### Message View

```javascript
message: {
    title: 'New Message',
    fields: {
        name: {
            placeholder: 'Your Name'
        },
        email: {
            placeholder: 'Your Email',
            error: 'Please enter a valid email address.'
        },
        subject: {
            placeholder: 'Subject'
        },
        message: {
            placeholder: 'How can we help you?',
            error: 'Please enter a message.'
        },
        secure: {
            placeholder: 'What are your admin credentials?',
            help: 'Your data is securely stored with AES-256 encryption and destroyed once we resolve your issue.'
        },
        screenshots: {
            placeholder: 'Drop screenshots here or click to upload'
        },
        diagnostics: {
            placeholder: 'Site Diagnostics',
            help: 'Send your site configuration data directly to us for expedited support.'
        }
    },
    action: {
        label: 'Send Message',
        loadingLabel: 'Sending Message...',
        diagnosticLabel: 'Preparing Store Diagnostics...',
        error: `Unable to send message. Email us directly at ${this.supportEmail}`
    }
}
```

### Message Sent View

```javascript
messageSent: {
    title: 'Message Sent',
    header: 'Message Sent',
    body: `We'll be in touch shortly.`,
    action: {
        label: 'Close Panel'
    }
}
```

### Error View

```javascript
error: {
    title: 'Error',
    notFound: {
        header: '404',
        body: `Sorry, we couldn't find this resource. Please try refreshing or contact us directly at`
    },
    other: {
        header: 'Oops!',
        body: `We're sorry, it looks like something went wrong. If you have a question please contact us directly at`
    }
}
```

## Fallback

If an error is encountered when loading the PluginDx embed panel or sending a message, your users should still be able to contact you directly in your plugin. For this reason it's **strongly recommended** that you provide your support email address via the `email` attribute.

Additionally, if you decide to opt out of PluginDx in the future, your users will still be able to contact you directly even if they're using an older version of your plugin with PluginDx included.