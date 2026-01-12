# Introduction

These are instructions on how to test EVP in Chrome:

# Setup

1. Install [chrome canaries](https://www.google.com/chrome/canary/)
2. Go to `chrome://flags/`, search for `Email Verification Protocol` and enable `#email-verification-protocol`. Restart the browser.
3. Go to `chrome://version` and make sure you are in 145+ (e.g. `145.0.7568.0 (Official Build) canary (arm64)`).
4. Go to `chrome://settings/addresses` and make sure an email address from a domain that supports EVP is present there. If not, add it.
5. Make sure you are logged in to the domain of your email address.

# Trying out

1. Before you change your own website, it can be useful to see if your set up works in an existing website. You can use this one [here](https://code.sgo.to/2024/10/25/verified-email-autocomplete.html) to try if you'd like.

# Develop

If you managed to give it a try on the step above, it means your setup is correct and you managed to use it on a demo website!

Now, to implement this on your own website, start by:

## [Step 1](https://github.com/WICG/email-verification-protocol?tab=readme-ov-file#1-email-request): Request a verified email address

First, start by augument your existing `input` box that you use to acquire the user's email address with a [server-side generated](https://github.com/WICG/email-verification-protocol?tab=readme-ov-file#1-email-request) value for `nonce`. 

For example: 

```html
<input id="email" type="email" autocomplete="email" nonce="12345677890..random">
```

Second, add an event listener for the `emailverified` event in the input box:

```html
<script>
const input = document.getElementById('email')

input.addEventListener('emailverified', e => {
  // e.presentationToken is SD-JWT+KB
  console.log({
      presentationToken: e.presentationToken
  })
})
</script>
```

## [Step 6](https://github.com/WICG/email-verification-protocol?tab=readme-ov-file#6-token-verification): Verify the email address

Finally, when the `emailverified` event is fired, you'll get back a `presentationToken` in it. 

Send this token to your server and follow the steps defined [here](https://github.com/WICG/email-verification-protocol?tab=readme-ov-file#6-token-verification).
