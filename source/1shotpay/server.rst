Server-Side Pay Link Checkout
=============================

For server-side integration, you give your server process the **API key** (and user ID) from your 1ShotPay account. The server can then generate **pay links** programmatically. The user clicks the payment link, which opens a payment page on 1ShotPay's domain. After the user approves the payment, they are redirected back to a URL you supply.

Install
-------

.. code-block:: bash

   yarn add @1shotapi/1shotpay-server-sdk @1shotapi/1shotpay-common

Import shared types (``UserId``, ``DecimalAmount``, ``AjaxError``, ``PayLinkId``, etc.) from **@1shotapi/1shotpay-common** when you need them.

Quick start (minimal)
---------------------

.. code-block:: typescript

   import { DecimalAmount, UserId } from "@1shotapi/1shotpay-common";
   import { OneShotPayServer } from "@1shotapi/1shotpay-server-sdk";

   const server = new OneShotPayServer(
     UserId(process.env.ONESHOT_USER_ID ?? ""),
     process.env.ONESHOT_API_TOKEN ?? "",
   );

   // 1) Create a pay link
   server
     .createPayLink(DecimalAmount(0.01), "Example checkout")
     // 2) Present the payLink.url to the user
     .andThen((payLink) => {
       console.log("Pay link:", payLink.url);

       // 3) Wait until it is paid (often in a background job)
       return server.waitForPayLinkPayment(payLink.id);
     })
     .match(
       (paidPayLink) => console.log("Paid!", paidPayLink),
       (err) => console.error("Payment failed:", err),
     );

Integration flow (high-level)
-----------------------------

1. **Create a pay link** with ``createPayLink()``. This returns an ``IPayLink`` that includes a hosted checkout ``url``.
2. **Present the link to the user** — open in a new tab, show a QR code, redirect, or embed as you prefer.
3. **Wait for completion** with ``waitForPayLinkPayment()``. In production this is usually done **outside the request/response thread** (e.g. a background job or worker), so your HTTP route can return the pay link URL immediately while you keep monitoring for completion.

Coding conventions
------------------

neverthrow (ResultAsync)
~~~~~~~~~~~~~~~~~~~~~~~~

The server SDK uses **neverthrow** so success and error are values, not exceptions:

- Methods generally return ``ResultAsync<T, AjaxError>`` instead of throwing.
- Handle results with ``.map(...)``, ``.andThen(...)``, and ``.match(okFn, errFn)``.

This keeps error handling explicit and composable, which is useful on the server for deterministic behavior and logging.

Branded types (type-safe primitives)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The shared package (**@1shotapi/1shotpay-common**) uses **branded types** for IDs, addresses, URLs, amounts, etc. They are normal runtime values (strings/numbers) tagged at the type level so you don't pass the wrong kind of value into the API.

Example:

.. code-block:: typescript

   import {
     DecimalAmount,
     PayLinkId,
     URLString,
     UserId,
   } from "@1shotapi/1shotpay-common";

   const userId = UserId("123e4567-e89b-12d3-a456-426614174000");
   const amount = DecimalAmount(0.01);
   const payLinkId = PayLinkId("123e4567-e89b-12d3-a456-426614174000");
   const mediaUrl = URLString("https://example.com/product.png");

resultAsyncToPromise() (async/await bridge)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you prefer ``async/await`` over chaining, **@1shotapi/1shotpay-common** provides ``resultAsyncToPromise()``. It turns a ``ResultAsync<T, Error>`` into a ``Promise<T>`` by throwing on error.

.. code-block:: typescript

   import {
     DecimalAmount,
     UserId,
     resultAsyncToPromise,
   } from "@1shotapi/1shotpay-common";
   import { OneShotPayServer } from "@1shotapi/1shotpay-server-sdk";

   const server = new OneShotPayServer(UserId("..."), "api-token");

   const payLink = await resultAsyncToPromise(
     server.createPayLink(DecimalAmount(0.01), "Async/await checkout"),
   );

OneShotPayServer API
--------------------

**OneShotPayServer** is the main entry point for server integration.

- **Authentication** is maintained per instance: each instance caches and refreshes its M2M JWT as needed. Internally, your ``userId`` and ``apiToken`` are exchanged for a short-lived JWT (and refreshed before expiry). You usually don't need to manage this unless you're tuning instance lifetime.

Construction
~~~~~~~~~~~~

.. code-block:: typescript

   import { ELocale, UserId } from "@1shotapi/1shotpay-common";
   import { OneShotPayServer } from "@1shotapi/1shotpay-server-sdk";

   const server = new OneShotPayServer(
     UserId(process.env.ONESHOT_USER_ID ?? ""),
     process.env.ONESHOT_API_TOKEN ?? "",
     ELocale.English, // optional (default)
   );

**Parameters:**

- **userId** (``UserId``) — Your 1ShotPay user ID.
- **apiToken** (``string``) — Your 1ShotPay API token / client secret.
- **locale** (optional ``ELocale``) — Used to build the hosted pay link URL (defaults to ``ELocale.English``).

createPayLink(amount, description, options?)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Creates a new pay link and returns it (including its hosted checkout URL).

.. code-block:: typescript

   import {
     DecimalAmount,
     UnixTimestamp,
     URLString,
     UserId,
   } from "@1shotapi/1shotpay-common";
   import { OneShotPayServer } from "@1shotapi/1shotpay-server-sdk";

   const server = new OneShotPayServer(UserId("..."), "api-token");

   server
     .createPayLink(DecimalAmount(0.03), "3Use Widget", {
       mediaUrl: URLString("https://example.com/widget.png"),
       reuseable: false,
       expirationTimestamp: UnixTimestamp(Math.floor(Date.now() / 1000) + 60 * 15),
       requestedPayerUserId: UserId("123e4567-e89b-12d3-a456-426614174000"),
       closeOnComplete: true,
     })
     .match(
       (payLink) => {
         console.log("Created pay link id:", payLink.id);
         console.log("Hosted URL:", payLink.url);
       },
       (err) => {
         console.error("createPayLink failed:", err);
       },
     );

The returned ``IPayLink`` has ``.url`` set to the hosted link for the locale you configured. ``closeOnComplete: true`` appends ``?closeOnComplete=true`` to the URL (useful for embedded flows).

getPayLink(payLinkId)
~~~~~~~~~~~~~~~~~~~~~

Fetches the current state of a pay link.

.. code-block:: typescript

   import { PayLinkId, UserId } from "@1shotapi/1shotpay-common";
   import { OneShotPayServer } from "@1shotapi/1shotpay-server-sdk";

   const server = new OneShotPayServer(UserId("..."), "api-token");

   server.getPayLink(PayLinkId("123e4567-e89b-12d3-a456-426614174000")).match(
     (payLink) => {
       console.log("Status:", payLink.status);
       console.log("URL:", payLink.url);
     },
     (err) => console.error("getPayLink failed:", err),
   );

waitForPayLinkPayment(payLinkId)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Polls until a pay link transitions to **paid**, then returns the paid ``IPayLink``. Use this after you've presented the hosted checkout URL to the user.

.. code-block:: typescript

   import { PayLinkId, UserId } from "@1shotapi/1shotpay-common";
   import { OneShotPayServer } from "@1shotapi/1shotpay-server-sdk";

   const server = new OneShotPayServer(UserId("..."), "api-token");
   const payLinkId = PayLinkId("123e4567-e89b-12d3-a456-426614174000");

   server.waitForPayLinkPayment(payLinkId).match(
     (paidPayLink) => console.log("Paid:", paidPayLink),
     (err) => console.error("waitForPayLinkPayment failed:", err),
   );

**Operational guidance:** For HTTP APIs, call ``waitForPayLinkPayment()`` in a **worker** (background job or separate process) so you can return the pay link URL to the client right away. For strict real-time completion, you can combine polling with server-sent events or a job-status endpoint in your app.
