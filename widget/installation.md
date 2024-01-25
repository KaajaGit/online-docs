---
label: Installation Guide
icon: code-of-conduct
---

# Widget Installation Guide

This document will guide you through the installation and configuration of the
Kaaja Widget.

## Installation

The Widget is integrated through an HTML `iframe`, a tag that incorporates
elements external to your web page you are working on, and a URL formatted using
a standard Kaaja methodology.

### Test / Production Environments

In Kaaja there are two distinct environments, one for testing and another for
production, respectively served at the following URLs:

1. https://www.kaajab.com/ (Test Environment)
2. https://www.kaaja.com/ (Production Environment)

### How to Embed an iFrame

In order to embed correctly, it is necessary to structure a URL in such a way
that Kaaja can interpret it and provide the correct information.

The path to the widget is found under: `/mate`.

The parameters required by a URL are as follows:

- `id` which represents the code of a Kaaja property (aligned with BNL unique
  reference code).
- `apiKey` which is provided by the Kaaja team and identifies who is making the
  calls.

There is inolte another, optional parameter:

- `data` an encrypted, url-safe string used to pre-populate some form fields
  with customer data.

A URL formatted solely as an example might be:

```
https://www.kaajab.com/mate?data=U2FsdGVkX1%2Bp753VI83n&id=PRE-SC-LU-51&apiKey=7fh802d10c9a3552ba45db30eb3f68f1
```

## Cyphered Form Data

As can be seen from the installation section, the `data` parameter is an
encrypted, url-safe string containing the data that will pre-populate some
fields of the form with customer data.

### How To Encrypt the User Data

The process of creating, encoding and sanitizing the string is done by following
the following steps (illustrated using JavaScript):

1. Perform the `JSON.stringify` of the object containing the data.
2. Perform encryption of the result using AES-256 as the algorithm, and the
   SECRET KEY provided securely by Kaaja.
3. Perform the `encodeURIComponent` of the result so that the encrypted string
   is usable in URLs.

Below is an example of the code used to perform the procedure:

```js
import CryptoJS from "crypto-js";

const AES_SECRET_KEY = "SECRET";

export function createEncryptedQs(data) {
  return encodeURIComponent(
    CryptoJS.AES.encrypt(JSON.stringify(data), AES_SECRET_KEY).toString()
  );
}

// Call the function
const result = createEncryptedQs({
  firstName: "Mario",
  lastName: "Rossi",
  email: "mario.rossi@example.com",
  zip_code: "23058",
  prefix: "+39",
  mobile: "3331334449",
  address: "Via del Dollaro 12",
  city: "Milano",
  province: "Milano",
  country: "Italia",
  tax_code: "PSLMRC94T13B428V",
});
```

> Please note: The algorithm used for encryption is AES-256.

## Customizations

Depending on your needs, you can set the size of the iframe to fit in different
contexts.

The widget is responsive and works accordingly on both desktop and mobile
devices, the average CSS query representing the breakpoint for the interface
change is as follows:

```css
@media (min-width: 640px) {
  /* CSS Here */
}
```

## Working Example

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Widget Environment</title>

    <!-- Dummy stylings for the sake of the example -->
    <style type="text/css">
      body {
        margin: 0;
      }

      .container {
        min-height: 100vh;
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
      }

      .frame {
        border: 1px solid #cccccc;
        border-radius: 16px;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <!-- Height and Width are customizable -->
      <iframe width="400" height="600" id="frame" class="frame"></iframe>
    </div>

    <script>
      // This must to be done at server-side in real environments.
      // const data = createEncryptedQs(...dataFromQs)

      const urlQs = new URLSearchParams([
        ["id", "H2P-MI-25-1"],
        ["apiKey", "vg3184d00c9a8217ba45db30eb3f99w1"], // Not the real API Key.
        // ['data', data] // This is optional, for automatic form fulfill.
      ]);

      const siteWidgetUrl = `https://www.kaajab.com/meta?${urlQs.toString()}`;

      window.addEventListener("load", (event) => {
        const frame = document
          .getElementById("frame")
          .setAttribute("src", siteWidgetUrl);
      });
    </script>
  </body>
</html>
```

### Encryption Sandbox

In order to create the data object as requested and test it, you can play with
the following CodeSandbox, which contains the source code useful for carrying
out the process.

Please **DO NOT** use sensitive data within the Sandbox as it is in the public
domain online.

Link to the Sandbox: https://codesandbox.io/p/devbox/lhmg93

[!embed](https://codesandbox.io/p/devbox/lhmg93?file=%2Fsrc%2Findex.ts)
