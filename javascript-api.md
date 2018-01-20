# JavaScript API

The PluginDx JavaScript API exposes the following methods to the global object `PDX.panel`. You can use these methods to control the embed panel programmatically.

## .init\(\)

```javascript
PDX.panel.init(handler);
```

## .open\(\)

```javascript
PDX.panel.open();
```

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



