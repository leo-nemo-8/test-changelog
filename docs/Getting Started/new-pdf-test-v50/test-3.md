---
title: test 3
deprecated: false
hidden: false
metadata:
  robots: index
---
/test This article describes the configuration options available for **Checkout SDK**, including top-level initialisation settings, configuration, field styling, and 3-D Secure challenge presentation. It also covers configuration defaults, parameter behaviour, and styling rules to help ensure a consistent and secure payment experience.

## CheckoutInitConfig — top level

<HTMLBlock>{`
<table class="custom-table">
  <tr>
    <th class="header-main">Parameter</th>
    <th class="header-main">Type</th>
    <th class="header-main">Default</th>
    <th class="header-main">Description</th>
  </tr>
  <tr>
    <td><code>apiKey</code> *</td>
    <td><code>string</code></td>
    <td>—</td>
    <td>The account's public API key.</td>
  </tr>
  <tr>
    <td><code>merchantAccountId</code> *</td>
    <td><code>string</code></td>
    <td>—</td>
    <td>The Merchant account identifier.</td>
  </tr>
  <tr>
    <td><code>paymentRequestId</code> *</td>
    <td><code>string</code></td>
    <td>—</td>
    <td>The identifier of the payment request (<code>pr_…</code>) created by your backend.</td>
  </tr>
  <tr>
    <td><code>baseUrl</code></td>
    <td><code>string</code></td>
    <td>script origin (CDN) or <code>window.location.origin</code> (npm)</td>
    <td>The single domain hosting the iframe fields, the bridge, and the Checkout API. Must be an absolute HTTP(S) URL. For npm integrations it is virtually always set explicitly.</td>
  </tr>
  <tr>
    <td><code>features</code></td>
    <td><code>FeatureStatic[]</code></td>
    <td><code>[]</code></td>
    <td>npm mode: feature statics (e.g. <code>[hpfFeature]</code>). Registration only makes a type available — an instance is created only by calling <code>initFeature(type)</code>. If the session configuration contains a type that is not registered, <code>init()</code> fails immediately. Not passed in CDN mode (the features are already in the bundle via <code>?features=</code>).</td>
  </tr>
  <tr>
    <td><code>featureOptions</code></td>
    <td><code>object</code></td>
    <td><code>{}</code></td>
    <td>Merchant-side feature settings layered over the backend configuration (the merchant wins). Read at the moment of <code>initFeature(type)</code>. The key is the feature type: <code>featureOptions.hpf</code> (see 7.2).</td>
  </tr>
  <tr>
    <td><code>challenge</code></td>
    <td><code>ChallengeConfig</code></td>
    <td><code>{ acsMode: 'redirect' }</code></td>
    <td>3DS challenge presentation (see 7.4). Applies only to the <code>acs_redirect</code> directive.</td>
  </tr>
  <tr>
    <td><code>timeout</code></td>
    <td><code>number</code> (ms)</td>
    <td><code>30000</code></td>
    <td>Timeout for RPC calls to the bridge. A positive finite number.</td>
  </tr>
  <tr>
    <td><code>initTimeout</code></td>
    <td><code>number</code> (ms)</td>
    <td><code>30000</code></td>
    <td>Timeout for the bridge handshake during <code>init()</code>. A positive finite number.</td>
  </tr>
</table>
<style> 

  :root {
    --custom-light-head: 236, 245, 254; 
    --custom-light-sub-head: 247, 250, 252; 
    --custom-light-row: 247, 250, 252; 
    --custom-dark-head: 52, 71, 84;  
    --custom-dark-sub-head: 46, 58, 66; 
    --custom-dark-row: 46, 58, 66; 
  }

  [data-color-mode="light"] .custom-table {
    --table-head: rgba(var(--custom-light-head), 1); 
    --table-sub-head: rgba(var(--custom-light-sub-head), 1); 
    --table-row: rgba(var(--custom-light-row), 0); 
    --table-stripe: rgba(var(--custom-light-row), 0);
  }

  [data-color-mode="dark"] .custom-table {
    --table-head: rgba(var(--custom-dark-head), 1); 
    --table-sub-head: rgba(var(--custom-dark-sub-head), 1); 
    --table-row: rgba(var(--custom-dark-row), 0); 
    --table-stripe: rgba(var(--custom-dark-row), 0); 
  }

  .custom-table {
    width: 100%; 
    border-collapse: collapse; 
    padding: 10px; 
    text-align: left; 
    vertical-align: top; 
    border: 1px solid var(--table-edges, #dfe2e5); 
  }
  
  .custom-table .header-main {
    background: var(--table-head);
    font-weight: bold; 
  }

  .custom-table .header-sub {
    background: var(--table-sub-head);
    font-weight: bold; 
  }
</style> 
`}</HTMLBlock>

\*required parameter. A missing or empty value → init() rejects with an invalid_config error.

## FeatureOptions.hpf — card fields

<HTMLBlock>{`
<table class="custom-table">
  <tr>
    <th class="header-main">Parameter</th>
    <th class="header-main">Type</th>
    <th class="header-main">Description</th>
  </tr>
  <tr>
    <td><code>style</code></td>
    <td><code>CommonStyling</code></td>
    <td>The global base style for all fields. The per-field style is layered on top (wins on each individual property).</td>
  </tr>
  <tr>
    <td><code>fields</code></td>
    <td><code>Record&lt;FieldType, …&gt;</code></td>
    <td>Individual field settings. Keys: <code>cardNumber</code>, <code>expiryDate</code>, <code>cvv</code>, <code>cardholderName</code>.</td>
  </tr>
  <tr>
    <td><code>fields.&lt;type&gt;.container</code></td>
    <td><code>string | HTMLElement</code></td>
    <td>A CSS selector or DOM element where the field's iframe is mounted. The container must exist in the DOM at the moment of <code>initFeature('hpf')</code>. If the backend configuration marks the field as <code>required</code>, a missing container for it is a feature initialisation error (<code>feature_error</code>).</td>
  </tr>
  <tr>
    <td><code>fields.&lt;type&gt;.style</code></td>
    <td><code>CommonStyling</code></td>
    <td>The style of a specific field on top of the global one.</td>
  </tr>
  <tr>
    <td><code>fields.&lt;type&gt;.placeholder</code></td>
    <td><code>string</code></td>
    <td>The field's placeholder. Recommended values: <code>1234 1234 1234 1234</code>, <code>MM/YY</code>, <code>CVC</code>, the name on the card.</td>
  </tr>
</table>
<style> 

  :root {
    --custom-light-head: 236, 245, 254; 
    --custom-light-sub-head: 247, 250, 252; 
    --custom-light-row: 247, 250, 252; 
    --custom-dark-head: 52, 71, 84;  
    --custom-dark-sub-head: 46, 58, 66; 
    --custom-dark-row: 46, 58, 66; 
  }

  [data-color-mode="light"] .custom-table {
    --table-head: rgba(var(--custom-light-head), 1); 
    --table-sub-head: rgba(var(--custom-light-sub-head), 1); 
    --table-row: rgba(var(--custom-light-row), 0); 
    --table-stripe: rgba(var(--custom-light-row), 0);
  }

  [data-color-mode="dark"] .custom-table {
    --table-head: rgba(var(--custom-dark-head), 1); 
    --table-sub-head: rgba(var(--custom-dark-sub-head), 1); 
    --table-row: rgba(var(--custom-dark-row), 0); 
    --table-stripe: rgba(var(--custom-dark-row), 0); 
  }

  .custom-table {
    width: 100%; 
    border-collapse: collapse; 
    padding: 10px; 
    text-align: left; 
    vertical-align: top; 
    border: 1px solid var(--table-edges, #dfe2e5); 
  }
  
  .custom-table .header-main {
    background: var(--table-head);
    font-weight: bold; 
  }

  .custom-table .header-sub {
    background: var(--table-sub-head);
    font-weight: bold; 
  }
</style> 
`}</HTMLBlock>

<Callout icon="📘" theme="info">
  Field configuration cascade: the session's backend configuration (default styles/placeholders/required flags) → the global featureOptions.hpf.style → the per-field fields.<type>.style. Each subsequent layer overrides the previous one on individual properties.
</Callout>

<br />

## CommonStyling — field style properties