# JavaScript API

The PluginDx JavaScript API exposes the following methods to the global object `PDX.panel`. You can use these methods to control the embed panel programmatically.

## .init\(\)

```javascript
PDX.panel.init(handler);
```

For integrations built as single-page applications (SPA) within a platform, you may wish to initialize the PluginDx embed panel programmatically in a specific view or route. Make sure you set `autoInit` to `false` in your embed panel [configuration](/embed/configuration.html). After the PluginDx `panel.js` script has been loaded, use the method above.

## .open\(\)

```javascript
PDX.panel.open();
```

Opens the PluginDx embed panel if it's closed, otherwise nothing happens. Returns the embed panel component object.

### Example

<button id="plugindx-open">Open Panel</button>
<script>
    $('#plugindx-open').click(function() {
        PDX.panel.open();
    });
</script>

### HTML

```html
<button id="plugindx-open">Open Panel</button>
```

### JavaScript (jQuery)

```javascript
$('#plugindx-open').click(function() {
    PDX.panel.open();
});
```

## .close\(\)

```javascript
PDX.panel.close();
```

Closes the PluginDx embed panel if it's open, otherwise nothing happens. Returns the embed panel component object.

### Example

<button id="plugindx-close">Close Panel</button>
<script>
    $('#plugindx-close').click(function() {
        PDX.panel.close();
    });
</script>

### HTML

```html
<button id="plugindx-close">Close Panel</button>
```

### JavaScript (jQuery)

```javascript
$('#plugindx-close').click(function() {
    PDX.panel.close();
});
```

## .toggle\(\)

```javascript
PDX.panel.toggle();
```

Toggles the PluginDx embed panel (open / close). Returns the embed panel component object.

### Example

<button id="plugindx-toggle">Toggle Panel</button>
<script>
    $('#plugindx-toggle').click(function() {
        PDX.panel.toggle();
    });
</script>

### HTML

```html
<button id="plugindx-toggle">Toggle Panel</button>
```

### JavaScript (jQuery)

```javascript
$('#plugindx-toggle').click(function() {
    PDX.panel.toggle();
});
```

<script async src="https://cdn.plugindx.com/embed/v1/panel.js"></script>
<div class="plugindx" data-key="019e4df072af5813b2c3e235" data-email="support@plugindx.com" data-platform="wordpress" data-type="circle-overlay" data-color="#007bff" data-message-fields="['name', 'email', 'subject', 'message', 'screenshots']"></div>

