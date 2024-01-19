---
description: Add items to the search toolbar
---

# Search Toolbar

Search toolbar items display below the search query input field. They have access to the current search query \(if available\), and can react to click events with content in a popover, or a custom event.[![](https://camo.githubusercontent.com/2a30eac0eb8ca19eb0682281f1e09d91335f6283b28b4e6177bc5b08d175eb41/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f736561726368546f6f6c6261722f746f6f6c6261722e706e67)](https://camo.githubusercontent.com/2a30eac0eb8ca19eb0682281f1e09d91335f6283b28b4e6177bc5b08d175eb41/687474703a2f2f6c6f63616c686f73742f657874656e73696f6e2d706f696e74732f66726f6e742d656e642f736561726368546f6f6c6261722f746f6f6c6261722e706e67)

**Example**

```javascript
    registry.registerExtension('org.bigconnect.search.toolbar', {
        tooltip: 'My Search Toolbar Item',
        icon: 'myIcon.png',
        canHandle: function(currentSearch) {
            // Only show the icon if there's a search
            if (currentSearch) return true;
            return false;
        },
        action: {
            type: 'popover',
            componentPath: 'com/example/js/content'
        }
    });
```

