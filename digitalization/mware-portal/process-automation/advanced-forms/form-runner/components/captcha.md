# Captcha

### Which captcha is right for you <a href="#which-captcha-is-right-for-you" id="which-captcha-is-right-for-you"></a>

[reCAPTCHA](http://en.wikipedia.org/wiki/ReCAPTCHA) is almost a de facto standard on the web: more than a million reCAPTCHA are solved every day, it is used by a large number of mainstream sites, like Facebook, and is constantly updated providing a high level of security. Since 2009, this service is owned by Google.

Because of the high level of safety provided by reCAPTCHA, we recommend you use it, unless doing so isn't possible in your situation. Maybe, for instance, you don't want the server on which Orbeon Forms runs to connect to any external service, which the reCAPTCHA component does to verify the answer provided. If you can't use the reCAPTCHA, Orbeon Forms provides an alternate component, SimpleCaptcha, which runs entirely within Orbeon Forms, without the need to connect to any external server.

### Events <a href="#events" id="events"></a>

Both the `fr:recaptcha` and `fr:simple-captcha` components support the same events:

1. **Verifying the answer entered by users** — Both components don't include a _Verify_ button that triggers the value entered by users to be checked. This is to give more control to you, the form author, as to when the verification is done. For instance, you might want to verify the captcha when users click on a _Save_ button on your form. To trigger the value to be verified, dispatch a `fr-verify` event to the captcha.
2. **Verification succeeded** — When the verification succeeds, the component dispatches a `fr-verify-done` event. The example below, using the reCAPTCHA, listens to that event to run a submission.
3. **Verification failed** — When the verification fails, you get the `fr-verify-error` event. The example below, using the reCAPTCHA, listens to that event to show a case id `failure-case` (which might tell users the verification failed).
4. **Loading another captcha** — Specifically for the reCAPTCHA, as part of the context information for the fr-verify-error event, you get an error code, which you can access with `event('fr-error-code')`. This is the error code returned by the reCAPTCHA API, which is a string. Its value can either be:
   * `empty` — this tells you users didn't provide any answer. When that happens, you could notify users and keep the same challenge.
   *   One of the values listed in the [reCAPTCHA API documentation](https://developers.google.com/recaptcha/docs/verify?csw=1) (look for the table under _Error Code Reference_). When this happens, you could notify users, and _need_ to change the challenge by dispatching `fr-reload` to the reCAPTCHA. (For added safety, the reCAPTCHA won't let users try to solve the same captcha multiple times.)



       ```
       <fr:recaptcha id="my-captcha">
         <xf:send ev:event="fr-verify-done" submission="save-submission"/>
         <xf:action ev:event="fr-verify-error">
             <xf:toggle case="failure-case"/>
             <xf:dispatch target="my-captcha" name="fr-reload"/>
         </xf:action>
       </fr:recaptcha>
       ```

### reCAPTCHA <a href="#recaptcha" id="recaptcha"></a>

#### Supported version <a href="#supported-version" id="supported-version"></a>

Advanced Forms supports the reCAPTCHA v2. Google [stopped supporting the reCAPTCHA v1](https://developers.google.com/recaptcha/docs/faq#what-happens-to-recaptcha-v1) after March 31, 2018.

#### Usage <a href="#usage" id="usage"></a>

You can use this component to show users a captcha, like the one shown in the following screen shot:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBwzgrnZr1wwR5Yq6%252Fxbl-recaptcha.gif%3Fgeneration=1528743339008692%26alt=media\&width=768\&dpr=4\&quality=100\&sign=750214a0624a4f07d27be9b6e549b1cb12054a023c6fbce8dd6d6ee039ba1165)

1. First, you need to [sign up with reCAPTCHA](https://www.google.com/recaptcha/admin#list) to get your own public/private key pair.
2.  Store your public key (now called "site key") and private key in your properties, as follows:



    ```
     <property
         as="xs:string"
         name="oxf.xforms.xbl.fr.recaptcha.public-key"
         value="..."/>
     <property
         as="xs:string"
         name="oxf.xforms.xbl.fr.recaptcha.private-key"
         value="..."/>
    ```
3. Add the reCAPTCHA component to your form:
   *   For forms you're creating in Form Builder, enable the reCAPTCHA by setting the following property:



       ```
         <property
             as="xs:string" 
             name="oxf.fr.detail.captcha.*.*" 
             value="reCAPTCHA"/>
       ```
   *   For forms you're creating by writing XForms "by hand", add the component to your form as follows. You'll also want to add handlers for the `fr-verify-done` and maybe `fr-verify-error` events.



       ```
         <fr:recaptcha id="my-recaptcha"/>
       ```

#### Configuration <a href="#configuration" id="configuration"></a>

You can configure:

* The theme using properties, either:
  *   For forms you're creating with Form Builder, with:



      ```
        <property 
            as="xs:string" 
            name="oxf.xforms.xbl.fr.recaptcha.theme.*.*" 
            value="light"/>
      ```
  *   For forms you're creating by writing XForms "by hand", with:



      ```
        <property 
            as="xs:string" 
            name="oxf.xforms.xbl.fr.recaptcha.theme"     
            value="light"/>
      ```
* The language with the `lang` attribute on the `<html>` element.

See the [reCAPTCHA documentation](https://developers.google.com/recaptcha/docs/display), under _Look & Feel Customizations_ for more information on the possible values for the `theme` and `lang` properties.

#### SimpleCaptcha <a href="#simplecaptcha" id="simplecaptcha"></a>

You can use this component to show users a captcha, like the one shown in the following screen shot:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBwzq5H2ia01pQFVj%252Fxbl-simple-captcha.png%3Fgeneration=1528743319187556%26alt=media\&width=768\&dpr=4\&quality=100\&sign=5af2a12f7e395891851612f4c3190e9ea176cf08f59e7c41bc02606e27891c0b)

To use this component, where you want the captcha to show in your form, add:

```
<fr:simple-captcha id="my-simple-captcha">
```

Most likely, you'll want to add code dispatching an `fr-verify` event to your component to trigger a verification, and listeners on the `fr-verify-done` and `fr-verify-error` events. For more information on this, see the section _Events_ above.

### &#x20; <a href="#see-also" id="see-also"></a>
