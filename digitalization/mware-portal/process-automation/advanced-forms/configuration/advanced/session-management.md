# Session management

### Who should read this <a href="#who-should-read-this" id="who-should-read-this"></a>

You'll most likely be interested in the information on this page if:

* You are able to load a form produced by Advanced Forms, but then Ajax requests sent by the browser occasionally or systematically get an HTTP 403 response, causing the "An error has occurred" dialog to show up.
* In your environment, requests from the browser don't immediately reach the app server running Advanced Forms, but instead go through some other software. For instance, this is the case if you're using a reverse proxy or have your own code embedding forms produced by Advanced Forms in your web pages.
* You want to learn more about session management in Advanced Forms.

### `JSESSIONID` and `UUID` <a href="#jsessionid-and-uuid" id="jsessionid-and-uuid"></a>

#### Normal operation <a href="#normal-operation" id="normal-operation"></a>

1. The first time a browser requests a web page from Advanced Forms, Advanced Forms creates a session, and the HTTP response has a header with `Set-Cookie: JSESSIONID=123`, where `123` is a unique identifier. (The specific cookie name may differ depending on how you configured your app server, but typically `JSESSIONID` is the default and for simplicity we'll use that name in the rest of this document.) From that point, any subsequent requests issued by the browser will have a header that looks like `Cookie: JSESSIONID=123`.
2.  When Advanced Forms generates a web page for a form, it produces a unique UUID, and stores _state_ in the session related to this UUID. If the user reloads the form, Advanced Forms generated a different UUID. You can see that UUID in the HTML sent by the browser, looking for the HTML hidden field named `$uuid`:

    ```
    <input type="hidden" name="$uuid" value="abc">
    ```
3.  When the Advanced Forms client-side code sends an Ajax request, it includes that UUID in the body of the request, and the browser passes the `JSESSIONID`. On the server, Advanced Forms uses that information to find the _state_ it stored in step 1. The UUID sent by Advanced Forms in the Ajax request looks like:

    Copy

    ```
    <xxf:uuid>abc</xxf:uuid>
    ```

#### Advanced Forms requirement <a href="#orbeon-forms-requirement" id="orbeon-forms-requirement"></a>

For Advanced Forms to operate normally, for a given web page loaded by the browser from Advanced Forms, the `JSESSIONID` set in the HTTP response produced by Advanced Forms (with `Set-Cookie: …`), or if none is set the `JSESSIONID` on in the HTTP request received by Advanced Forms must also be the one Advanced Forms receives in all subsequent Ajax requests issued by that page.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC7YvuzcRRYZeuDtS%252Fsession-where.png%3Fgeneration=1528743365912027%26alt=media\&width=768\&dpr=4\&quality=100\&sign=5a9c983662005b5aff2852f3a4c0dff9911fb5f7cf8723f6ed2757d6601e88df)

Which HTTP requests/responses we are interested in

Note that those requirements apply to the HTTP requests and responses sent to and coming from Advanced Forms. As illustrated in the above diagrams, if you have reverse proxy or embedding code those will be different than the HTTP requests and responses made by and received by the browser, and the `JSESSIONID` (or equivalent) used between the browser and the proxy / embedding code is likely to be different than the `JSESSIONID` used between the proxy / embedding code and Advanced Forms. Again, Advanced Forms' requirement apply to the latter.

### HTTP 403 / "An error has occurred" dialog <a href="#http-403-an-error-has-occurred-dialog" id="http-403-an-error-has-occurred-dialog"></a>

When Advanced Forms receives an Ajax request (see the third step in the previous section), if it can't find the state associated to the UUID in the session, it responds with an [HTTP 403 error](https://en.wikipedia.org/wiki/HTTP\_403). When the Advanced Forms client-side code receives the 403, it shows a dialog titled "An error has occurred".

#### Normal cases <a href="#normal-cases" id="normal-cases"></a>

The following 2 cases can happen, and if they they do getting a 403 response and the corresponding "An error has occurred" dialog is expected, but those situations should happen very rarely:

1. The user's session has expired – As long as users keep a form open in their browser, by default the Advanced Forms client-side code makes sure to send a "session heartbeat" Ajax request before the session expires to keep it alive. However:
   * It's possible for you to disable the session heartbeat feature. If you do, it is possible for the session to expire even if users keep a browser window with the form open.
   * In situations where the browser looses connectivity to the server for an extended period of time, the session heartbeat mechanism might not be able to contact the server before the session expires. For instance, this could happen if a user were to load a form on her laptop, close the lid, commute home, open the lid, and try to continue filling out the same form.
2. A user loads the form, the server running Advanced Forms is restarted, and then the user tries to interact with the form.

In both cases, if, after getting the error, users reload the form and get the error again, then you might have hit one of the "problematic cases" described in the section below.

#### Problematic cases <a href="#problematic-cases" id="problematic-cases"></a>

If you're getting 403 errors more regularly, and you have software sitting between the browser and Advanced Forms, it is a sign that this software does not handle cookies properly. That software can be:

* A reverse proxy, for instance used to provide authentication, single sign-on, serve assets more efficiently, or provide security services.
* Code running in a web app used to embed a form produced by Advanced Forms into a page produced by the web app.

In those cases, you can solve the issue by either:

* If you have own Java code to embed forms created with Form Builder, switch to using the built-in Form Runner Java Embedding API, which will handle cookies properly.
* Debug and fix the issue is the said software, armed with a better understanding of the Advanced Forms' requirements when it comes to cookies, based on this information on this page.
* If using Tomcat, in Tomcat's directory edit `conf/context.xml`, and add `sessionCookiePath="/"` on the root element, so it looks as follows: `<Context sessionCookiePath="/">`. This will make the job of any reverse proxy or embedding code you might have much simpler, and could help you get around bugs in that code.
