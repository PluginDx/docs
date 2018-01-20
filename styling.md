# CSS & Styling

Since the PluginDx embed panel is injected directly into the DOM of your platform, you can easily add custom CSS to your plugin for styling. Classes utilize the `pdx` namespace to avoid styling conflicts with other elements on the page.

After installing the panel in your plugin, open the panel locally and use Chrome DevTools or the browser of your choice to inspect the DOM. Locate the embed panel and review the underlying markup to create your own CSS rules:

```css
.pdx-embed p {
    font-size: 17px;
    color: #000;
}

.pdx-embed-btn {
    background: #ccc;
    color: #000;
}
```

If needed, use the `!important` declaration as hard override of select CSS properties if needed.