# Time ago snippet guide

This quick guide demonstrates how to create a JavaScript snippet that converts date times into hours, days, minutes, and seconds from the current moment.

### Steps

1. Create an internal table this should include some form of date (In my case I will theme this around a blog)
2.  Add data to your table



    <figure><img src="https://files.readme.io/742dc97-Screenshot_2024-04-19_at_13.48.04.png" alt=""><figcaption></figcaption></figure>
3. Go to the design screen and add a Card block
4. Fill in the relevant bindings for Title, description etc
5. Go to the subtitle bindable area.
6. Switch the tab from text to javaScript
7. Click on the `</>` icon and create a new snippet called `timeago`
8. Paste the below code into it and click save

JavaScript

```Text
return function(pastTimeString) {
    const pastTime = new Date(pastTimeString);
    const now = new Date();
    const delta = now - pastTime;

    const days = Math.floor(delta / (1000 * 60 * 60 * 24));
    const hours = Math.floor((delta % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    const minutes = Math.floor((delta % (1000 * 60 * 60)) / (1000 * 60));
    const seconds = Math.floor((delta % (1000 * 60)) / 1000);

    const timeStrings = [];
    if (days > 0) {
        timeStrings.push(`${days} days`);
    }
    if (hours > 0) {
        timeStrings.push(`${hours} hours`);
    }
    if (minutes > 0) {
        timeStrings.push(`${minutes} minutes`);
    }
    if (seconds > 0) {
        timeStrings.push(`${seconds} seconds`);
    }

    if (timeStrings.length === 0) {
        return "just now";
    } else if (timeStrings.length === 1) {
        return timeStrings[0];
    } else {
        return timeStrings.slice(0, -1).join(', ') + ', and ' + timeStrings.slice(-1) + ' ago';
    }
}
```

9. Click the snippet and add the date binding you wish to use inside its brackets
10. Then click save

<figure><img src="https://files.readme.io/af76113-Screenshot_2024-04-05_at_13.21.07.png" alt=""><figcaption></figcaption></figure>

### The end result!

This quick guide demonstrates how to create a JavaScript snippet that converts date times into hours, days, minutes, and seconds from the current moment.

### Steps

1. Create an internal table this should include some form of date (In my case I will theme this around a blog)
2.  Add data to your table



    <figure><img src="https://files.readme.io/742dc97-Screenshot_2024-04-19_at_13.48.04.png" alt=""><figcaption></figcaption></figure>
3. Go to the design screen and add a Card block
4. Fill in the relevant bindings for Title, description etc
5. Go to the subtitle bindable area.
6. Switch the tab from text to javaScript
7. Click on the `</>` icon and create a new snippet called `timeago`
8. Paste the below code into it and click save

JavaScript

```Text
return function(pastTimeString) {
    const pastTime = new Date(pastTimeString);
    const now = new Date();
    const delta = now - pastTime;

    const days = Math.floor(delta / (1000 * 60 * 60 * 24));
    const hours = Math.floor((delta % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    const minutes = Math.floor((delta % (1000 * 60 * 60)) / (1000 * 60));
    const seconds = Math.floor((delta % (1000 * 60)) / 1000);

    const timeStrings = [];
    if (days > 0) {
        timeStrings.push(`${days} days`);
    }
    if (hours > 0) {
        timeStrings.push(`${hours} hours`);
    }
    if (minutes > 0) {
        timeStrings.push(`${minutes} minutes`);
    }
    if (seconds > 0) {
        timeStrings.push(`${seconds} seconds`);
    }

    if (timeStrings.length === 0) {
        return "just now";
    } else if (timeStrings.length === 1) {
        return timeStrings[0];
    } else {
        return timeStrings.slice(0, -1).join(', ') + ', and ' + timeStrings.slice(-1) + ' ago';
    }
}
```

9. Click the snippet and add the date binding you wish to use inside its brackets
10. Then click save

<figure><img src="https://files.readme.io/af76113-Screenshot_2024-04-05_at_13.21.07.png" alt=""><figcaption></figcaption></figure>
