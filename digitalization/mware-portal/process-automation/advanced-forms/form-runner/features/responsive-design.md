# Responsive design

### Fields in one column <a href="#fields-in-one-column" id="fields-in-one-column"></a>

Starting with version 4.10, Orbeon Forms supports [responsive design](https://en.wikipedia.org/wiki/Responsive\_web\_design) for forms you create with Form Builder. One significant implication is that when your forms are shown on a narrow screen, for instance on a mobile phone, most fields are reflowed to be on the same "column".

### Example <a href="#example" id="example"></a>

If you have a form with a section containing a grid with two columns, on a desktop or tablet your form might show as:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC33J8NE94yP4rjDR%252Fresponsive-desktop.png%3Fgeneration=1528743347848576%26alt=media\&width=768\&dpr=4\&quality=100\&sign=deae13e5952a320f0a3b67a5be821104f3f7d6ec2010d4e35e5097f0584473ad)

Fields as shown on a desktop

When the same form is loaded from a mobile phone, fields will automatically be shown in one column to better fit that screen:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC33N-vh5hmxuVrVg%252Fresponsive-mobile.png%3Fgeneration=1528743353037754%26alt=media\&width=768\&dpr=4\&quality=100\&sign=62c9734d3d4344678286bbefc9fc665e1c19f76fa6d5dcb8661a05cb3ccc116d)

Fields as shown on a mobile phone

### Limitations <a href="#limitations" id="limitations"></a>

Version 4.10 introduces responsive design to the "detail page", i.e. the page used to fill out a form, as it is the page accessed by most end users, who are also more likely to be using a mobile device. In future versions, responsive design will be brought also to the summary page and [Published Forms page](https://doc.orbeon.com/form-runner/pages/published-forms-page).
