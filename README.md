# Configure `clasp`

📜 Authenticate the [`clasp` Google Apps Script CLI tool](https://github.com/google/clasp)

<table align=center><td>

<tr><td>

```yml
- uses: jcbhmr/configure-clasp@v1
  with:
    clasprc-json: ${{ secrets.CLASPRC_JSON }}
    secret-name: CLASPRC_JSON
    secret-token: ${{ secrets.CONFIGURE_CLASP_GH_PAT }}
- run: clasp push
```

</table>

## Usage

**🚀 Here's what you're after:**

```yml
on:
  release:
    types: [released]
concurrency: ${{ github.workflow }}
jobs:
  clasp-push:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install --global @google/clasp
      - uses: jcbhmr/configure-clasp@v1
        with:
          clasprc-json: ${{ secrets.CLASPRC_JSON }}
          secret-name: CLASPRC_JSON
          secret-token: ${{ secrets.CONFIGURE_CLASP_GH_PAT }}
      - run: clasp push
```

1. **Enable the Google Apps Script API**

    1. Navigate to [your Google Apps Script dashboard](https://script.google.com/).
    2. Go to [the <kbd>⚙️ Settings</kbd> page](https://script.google.com/home/usersettings) using the left menu pane.
    3. Click the <kbd>Google Apps Script API</kbd> setting. It's currently the only Google Apps Script setting available.
    4. Read the ℹ informative warning about enabling the API.
    5. Click the toggle to enable the API.

2. **Get your local `clasp` credentials**

    You can use `clasp login` to generate a `.clasprc.json` file. You may already have one from previous `clasp` usage.
    
    ```sh
    clasp login
    ```
    
    This `.clasprc.json` file contains all the necessary information to issue authorized requests to the Google Apps Script API on your behalf. You should now copy it to use in the next step.
    
    ```sh
    cat ~/.clasprc.json
    # 👆 Copy the output to your clipboard
    ```

3. **Store your `clasp` credentials in a GitHub Actions secret**

    1. Navigate to the GitHub repository you want to use `clasp` with.
    2. Go to the repository's <kbd>⚙️ Settings</kbd> page.
    3. Find the <kbd>*️⃣ Secrets and variables</kbd> dropdown on the left navigation pane. Choose <kbd>Actions</kbd>.
    4. Under <kbd>Repository secrets</kbd> click <kbd>New repository secret</kbd>.
    5. Choose a secret name. I like to use `CLASPRC_JSON`.
    6. Paste your `.clasprc.json` contents that you copied earlier into the <kbd>Secret *</kbd> textbox.
    7. Click <kbd>Add secret</kbd>
