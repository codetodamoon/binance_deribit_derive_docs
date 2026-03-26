# Change Session Key Label

### Method Name

#### `private/change_session_key_label`

TODO description

### Parameters

<HTMLBlock>
  {`
  <div class="rdmd-table">
      <div class="rdmd-table-inner">
          <table>
              <thead>
                  <tr>
                      <th style="text-align: left"></th>
                  </tr>
              </thead>
              <tbody><tr>
    <td style="text-align: left"><span class="ws-small-font"></span><strong>label
      </strong><span class="ws-data-type ws-small-font">string
      </span><span class="ws-required-tag ws-small-font">required</span>
  <br /></td>
  </tr>
  </tbody>
          </table>
      </div>
  </div>

  <style title="rm-custom-css">
      :root {
          --project-color-primary: #0e0f13;
          --project-color-inverse: #fff;
      }

      [id='enterprise'] .ReadMeUI[is='AlgoliaSearch'] {
          --project-color-primary: #0e0f13;
          --project-color-inverse: #fff;
      }

      .theme-solid header#hub-header #header-top {
          background-color: #0e0f13;
      }

      .theme-solid.header-gradient header#hub-header #header-top {
          background: linear-gradient(to bottom, #0e0f13, #000);
      }

      .theme-solid.header-custom header#hub-header #header-top {
          background-image: url(undefined);
      }

      .theme-line header#hub-header #header-top {
          border-bottom-color: #0e0f13;
      }

      .theme-line header#hub-header #header-top .btn {
          background-color: #0e0f13;
      }

      #hub-subheader-parent #hub-subheader .hub-subheader-breadcrumbs .dropdown-menu a:hover {
          background-color: #0e0f13;
      }

      #subheader-links a.active {
          color: #0e0f13 !important;
          box-shadow: inset 0 -2px 0 #0e0f13;
      }

      #subheader-links a:hover {
          color: #0e0f13 !important;
          box-shadow: inset 0 -2px 0 #0e0f13;
          opacity: 0.7;
      }

      .main_color {
          color: #0e0f13 !important;
      }

      .border_bottom_main_color {
          border-bottom: 2px solid #0e0f13;
      }

      .main_color_hover:hover {
          color: #0e0f13 !important;
      }

      #hub-reference .hub-reference .logs:last-child .logs-empty,
      #hub-reference .hub-reference .logs:last-child .logs-login {
          margin-bottom: 35px;
      }

      .main-color-accent {
          border-bottom: 3px solid #0e0f13;
          padding-bottom: 8px;
      }

      
      h1 {
          font-family: Lato, sans-serif !important;
      }

      .markdown-body p {
          font-family: Lato, sans-serif;
      }

      table,
      td {
          background-color: #303b42;
          line-height: 25px;
      }

      thead {
          display: none;
      }

      .markdown-body code {
          border: solid 0.5px #4f5a66 !important;
          background-color: #2a3339;
      }

      summary {
          background-color: #303b42;
          padding: 10px;
      }

      .ws-method-div {
          margin-top: -10px !important;
      }

      .ws-small-font {
          font-size: 13px;
      }

      .ws-description {
          font-size: 0.9em;
          line-height: 0;
      }

      .ws-data-type {
          color: #adb4c1;
      }

      .ws-required-tag {
          color: #e95f6a;
      }

      .ws-row-blank {
          text-align: left;
          background: #242E34;
      }

      .red-code {
          background-color: #f6f8fa;
          background-color: var(--md-code-background, #f6f8fa);
          border-radius: 3px;
          color: var(--md-code-text);
          font-size: 85%;
          margin: 0;
          padding: 0.2em 0.4em;
      }

      .child-header {
          background-color: #242e34;
      }

      [data-color-mode="light"] table,
      [data-color-mode="light"] td {
          background-color: #f6f8fa;
          line-height: 25px;
      }

      @media (prefers-color-scheme: light) {
      [data-color-mode="system"] table
      [data-color-mode="system"] td {
          background-color: #f6f8fa;
          line-height: 25px; }
      }

      [data-color-mode="light"] .markdown-body code {
          border: solid 0.5px #cccccc !important;
          background-color: #ffffff;
      }

      @media (prefers-color-scheme: light) {
      [data-color-mode="system"] .markdown-body code {
          border: solid 0.5px #cccccc !important;
          background-color: #ffffff; }
      }

      [data-color-mode="light"] .ws-row-blank {
          background-color: #ffffff;
          text-align: left;
      }

      @media (prefers-color-scheme: light) {
      [data-color-mode="system"] .ws-row-blank {
          background-color: #ffffff;
          text-align: left;}
      }

      
  </style>
  `}
</HTMLBlock>

### Response

<HTMLBlock>
  {`
  <div class="rdmd-table">
      <div class="rdmd-table-inner">
          <table>
              <thead>
                  <tr>
                      <th style="text-align: left"></th>
                  </tr>
              </thead>
              <tbody><tr>
    <td style="text-align: left"><span class="ws-small-font"></span><strong>id
      </strong><span class="ws-data-type ws-small-font">string&nbsp;or&nbsp;integer
      </span><span class="ws-required-tag ws-small-font">required</span>
  <br /></td>
  </tr>
  <tr>
    <td style="text-align: left"><span class="ws-small-font"></span><strong>result
      </strong><span class="ws-data-type ws-small-font">object
      </span><span class="ws-required-tag ws-small-font">required</span>
  <br /></td>
  </tr>
  <tr>
    <td style="text-align: left"><span class="ws-small-font">result.</span><strong>label
      </strong><span class="ws-data-type ws-small-font">string
      </span><span class="ws-required-tag ws-small-font">required</span>
  <br /></td>
  </tr>
  </tbody>
          </table>
      </div>
  </div>

  <style title="rm-custom-css">
      :root {
          --project-color-primary: #0e0f13;
          --project-color-inverse: #fff;
      }

      [id='enterprise'] .ReadMeUI[is='AlgoliaSearch'] {
          --project-color-primary: #0e0f13;
          --project-color-inverse: #fff;
      }

      .theme-solid header#hub-header #header-top {
          background-color: #0e0f13;
      }

      .theme-solid.header-gradient header#hub-header #header-top {
          background: linear-gradient(to bottom, #0e0f13, #000);
      }

      .theme-solid.header-custom header#hub-header #header-top {
          background-image: url(undefined);
      }

      .theme-line header#hub-header #header-top {
          border-bottom-color: #0e0f13;
      }

      .theme-line header#hub-header #header-top .btn {
          background-color: #0e0f13;
      }

      #hub-subheader-parent #hub-subheader .hub-subheader-breadcrumbs .dropdown-menu a:hover {
          background-color: #0e0f13;
      }

      #subheader-links a.active {
          color: #0e0f13 !important;
          box-shadow: inset 0 -2px 0 #0e0f13;
      }

      #subheader-links a:hover {
          color: #0e0f13 !important;
          box-shadow: inset 0 -2px 0 #0e0f13;
          opacity: 0.7;
      }

      .main_color {
          color: #0e0f13 !important;
      }

      .border_bottom_main_color {
          border-bottom: 2px solid #0e0f13;
      }

      .main_color_hover:hover {
          color: #0e0f13 !important;
      }

      #hub-reference .hub-reference .logs:last-child .logs-empty,
      #hub-reference .hub-reference .logs:last-child .logs-login {
          margin-bottom: 35px;
      }

      .main-color-accent {
          border-bottom: 3px solid #0e0f13;
          padding-bottom: 8px;
      }

      
      h1 {
          font-family: Lato, sans-serif !important;
      }

      .markdown-body p {
          font-family: Lato, sans-serif;
      }

      table,
      td {
          background-color: #303b42;
          line-height: 25px;
      }

      thead {
          display: none;
      }

      .markdown-body code {
          border: solid 0.5px #4f5a66 !important;
          background-color: #2a3339;
      }

      summary {
          background-color: #303b42;
          padding: 10px;
      }

      .ws-method-div {
          margin-top: -10px !important;
      }

      .ws-small-font {
          font-size: 13px;
      }

      .ws-description {
          font-size: 0.9em;
          line-height: 0;
      }

      .ws-data-type {
          color: #adb4c1;
      }

      .ws-required-tag {
          color: #e95f6a;
      }

      .ws-row-blank {
          text-align: left;
          background: #242E34;
      }

      .red-code {
          background-color: #f6f8fa;
          background-color: var(--md-code-background, #f6f8fa);
          border-radius: 3px;
          color: var(--md-code-text);
          font-size: 85%;
          margin: 0;
          padding: 0.2em 0.4em;
      }

      .child-header {
          background-color: #242e34;
      }

      [data-color-mode="light"] table,
      [data-color-mode="light"] td {
          background-color: #f6f8fa;
          line-height: 25px;
      }

      @media (prefers-color-scheme: light) {
      [data-color-mode="system"] table
      [data-color-mode="system"] td {
          background-color: #f6f8fa;
          line-height: 25px; }
      }

      [data-color-mode="light"] .markdown-body code {
          border: solid 0.5px #cccccc !important;
          background-color: #ffffff;
      }

      @media (prefers-color-scheme: light) {
      [data-color-mode="system"] .markdown-body code {
          border: solid 0.5px #cccccc !important;
          background-color: #ffffff; }
      }

      [data-color-mode="light"] .ws-row-blank {
          background-color: #ffffff;
          text-align: left;
      }

      @media (prefers-color-scheme: light) {
      [data-color-mode="system"] .ws-row-blank {
          background-color: #ffffff;
          text-align: left;}
      }

      
  </style>
  `}
</HTMLBlock>

### Example

```shell
{request_example_shell}
```

```javascript
{request_example_javascript}
```

```python
{request_example_python}
```

> The above command returns JSON structured like this:

```json
{response_example_json}
```