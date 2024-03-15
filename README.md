<h1 align="center">
	<br>
	<img width="480" src="src/media/dy-logo.svg" alt="Kanstructor">
	<br>
	<br>
</h1>

> Write, test and repeat using YAML syntax

## Highlights

- No programming
- Automated tests in plain Yaml
- Visual regression tests in minutes
- Browser compatibility checks
- Designed for test engineers, product teams

## Installation

**IMPORTANT:** Since this package is node based project, let's remember to install `node` and `npm` as pre-requisites before setting up the project. [Refer here for more info.](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)

```sh
npx setup-dance-studio demo-project
```

Running the above command automatically sets up project structure, along with example files. [This section](#quick-start) provides the details on how the project resources are organised.

In order to run the tests, the following command does the job:

```sh
npm run test
```

## Quick Start

- **Step 1** : The `src` folder under the project is going to be a root directory for all testing stuff, and the most of the project contents will be inside `resources` directory.

- **Step 2** : Under `resources` folder, let's create `tests` folder which will contain test files in a plain YAML format. Note that, this package `dancing-yaml` identifies a file as a test file only if it ends in `*test.yaml`. [More on how to write tests?](#write-tests)

- **Step 3** : Many a times, `CSS` and `XPath` values used to identify html elements will be used in several places across test files. To keep all such values in centralized place, the folder `elements` needs to be created within `resources`. Just like test files, the element files need to be ending in `*element.yaml` The following snippet shows some example element yaml.

	```yaml
		### Login page elements' xpath and css

		login_email: "[name='email']"
		login_password: "[name='password']"
		login_button: "//button[normalize-space()='Login']"
		home_logo: "a[href*='home']"
	```

- **Step 4** : The next step is, the folders `extracted-contents` and `snapshots` need to be created to save all contents extracted during testing to external files and to keep golden copies of screenshots that will be verified against app under tests during testing respectively.

- **Step 5** : All common configurations such as browser, env etc will have to be in the file: `config.yaml` under `config` folder. The following snippet shows some examples.


	```yaml
		browser: chrome
		headless: false
		device: Desktop Chrome
		url: https://github.com/5v1988/kanstructor
	```

— **Step 6** : Lastly, to run all tests, the test runner `runMe.js` needs to be created in the project as follows:

  ```js
    import runMe from 'dancing-yaml'
    runMe();
  ```
Now execute tests using `node src/runMe.js` from command line. Note that, not necessarily that the runner method must always be named as `runMe`; The below is the sample structure once all setup is complete.


```sh
.
├── README.md
├── node_modules
├── package-lock.json
├── package.json
└── src
    ├── resources
    │   ├── config
    │   │   └── config.yaml
    │   ├── elements
    │   │   └── todo-element.yaml
    │   ├── reports
    │   │   ├── results.html
    │   │   └── results.json
    │   ├── snapshots
    │   │   ├── original-screenshot-1.png
    │   │   └── reference-screenshot-1.png
    │   └── tests
    │       └── todo-test.yaml
    └── runMe.js

```

## Write Tests

 — Tests are expected to be written in Yaml files, otherwise known as test files while using this package. Each of these tests should have to be written using well known testing format: `Arrange-Act-Assert`

 ```yaml

description: Some tests on cypress todo demo site
tests:
  - name: Set and delete todo lists
    exclude: false

    arrange:
      - name: openUrl
        url: url

    act:
      - name: Add the first item
        locator: .new-todo
        action: type
        value: Schedule doctor appointment

      - name: Press Enter
        pause: 1
        action: press
        value: Enter

      - name: Add the second item
        locator: .new-todo
        action: type
        value: Prepare a blog content

      - name: Press Enter
        pause: 1
        action: press
        value: Enter

      - name: Add the third item
        locator: .new-todo
        action: type
        value: Fix the air conditioner

      - name: Press Enter
        pause: 1
        action: press
        value: Enter

      - name: Screenshot after adding all items
        pause: 1
        action: snapshot
        path: "src/example/resources/snapshots/original-screenshot-1.png"    

      - name: Hover to the first item
        locator: "//div[normalize-space()='Schedule doctor appointment']"
        action: hover

      - name: Delete the first item
        pause: 2
        locator: "//div[normalize-space()='Schedule doctor appointment']//button"
        action: click

      - name: Hover to the second item
        locator: "//div[normalize-space()='Prepare a blog content']"
        action: hover

      - name: Delete the second item
        pause: 2
        locator: "//div[normalize-space()='Prepare a blog content']//button"
        action: click

      - name: Hover to the third item
        locator: "//div[normalize-space()='Fix the air conditioner']"
        action: hover

      - name: Delete the third item
        pause: 2
        locator: "//div[normalize-space()='Fix the air conditioner']//button"
        action: click

      - name: Screenshot after deleting all items
        pause: 1
        action: snapshot
        path: "src/example/resources/snapshots/original-screenshot-2.png"     

    assert:
      - name: Verify if the first item deleted
        pause: 2
        type: text
        text: Schedule doctor appointment
        state: invisible

      - name: Verify if the second item deleted
        pause: 2
        type: text
        text: Prepare a blog content
        state: invisible

      - name: Verify if the third item deleted
        pause: 2
        type: text
        text: Fix the air conditioner
        state: invisible

      - name: Compare screenshot after all items added
        type: snapshot
        original: "src/example/resources/snapshots/original-screenshot-1.png"
        reference: "src/example/resources/snapshots/reference-screenshot-1.png"

      - name: Compare screenshot after all items deleted
        type: snapshot
        original: "src/example/resources/snapshots/original-screenshot-2.png"
        reference: "src/example/resources/snapshots/reference-screenshot-2.png"

 ```

 — A test file can have more than one test, however, our recommendation is to have a few of them, organized by some commonalities

 — A test folder `tests` can contain several test files; No limits

— The high-level blocks — Arrange, Act and Assert, contain a sequence of steps to perform certain actions during testing.

### Arrange Reference

<table>
  <tr>
    <th>Name</th>
    <th>Description</th>
    <th>Keys</th>
    <th>Example</th>
  </tr>
  <tr>
    <td><pre>openUrl</pre></td>
    <td>Open an app url in browser</td>
    <td>Required — <br>
      name,<br>
      url<br>
      Optional — <br>
      pause
    </td>
    <td>
      <pre lang="yaml">
        - name: openUrl 
          url: https://github.com/5v1988
      </pre>  
    </td>
  </tr>
</table>


### Act Reference

<table>
  <tr>
    <th>Action</th>
    <th>Description</th>
    <th>Keys</th>
    <th>Example</th>
  </tr>
  <tr>
    <td>
      <pre>type</pre>
    </td>
    <td>Enter characters into textboxes</td>
    <td>Required — <br>
      name,<br>
      action,<br>
      locator,<br>
      value <br>
      Optional — <br>
      pause
    </td>
    <td>
      <pre lang="yaml">
          - name: Type in username
            action: type
            locator: "input[name='email']"
            value: 5v1988@gmail.com
        </pre>
    </td>
  </tr>
  <tr>
    <td>
        check,
        uncheck
    </td>
    <td>Check (or Uncheck) radio button/checkbox</td>
    <td>Required — <br>
      name,<br>
      action,<br>
      locator,<br>
      Optional — <br>
      pause
    </td>
    <td>
      <pre lang="yaml">
        - name: Choose a gender
          action: check
          locator: "input[type='checkbox']"
      <pre>    
    </td>  
  </tr>
  <tr>
    <td>click, <br>
      doubleclick <br>
    </td>
    <td>Click (or Doubleclick) button/link</td>
    <td>Required — <br>
      name,<br>
      action,<br>
      locator,<br>
      Optional — <br>
      pause
    </td>
    <td>
      <pre lang="yaml">
        - name: Type in username
          action: click
          locator: '#file-submit'
      </pre>  
    </td>  
  </tr>
  <tr>
    <td>select</td>
    <td>Select a dropdown value</td>
    <td>Required — <br>
      name,<br>
      action,<br>
      locator,<br>
      value <br>
      Optional — <br>
      pause
    </td>
    <td>
      <pre lang="yaml">
        - name: choose_dropdown
          locator: "#dropdown"
          action: select
          value: Option 2
      </pre>
    </td>  
  </tr>
  <tr>
    <td>press</td>
    <td>Simulate a key press</td>
    <td>Required — <br>
      name,<br>
      action,<br>
      value <br>
      Optional — <br>
      pause
    </td>
    <td>
      <pre lang="yaml">
        - name: Press enter
          action: press
          value: Enter
      </pre>
    </td>  
  </tr>
  <tr>
    <td>clear, <br>
        focus, <br>
        hover<br>
      </td>
    <td>Clear (or focus or hover) on html element</td>
    <td>Required — <br>
      name,<br>
      action,<br>
      locator <br>
      Optional — <br>
      pause
    </td>
    <td>
      <pre lang="yaml">
        - name: hover on the login link
          locator: "//button[@id='login']"
          action: hover
      </pre>
    </td>  
  </tr>    
  <tr>
    <td>snapshot</td>
    <td>Take a screenshot of a current window</td>
    <td>Required — <br>
      name,<br>
      action,<br>
      path <br>
      Optional — <br>
      pause
    </td>
    <td>
      <pre lang="yaml">
        - name: Screenshot the login failure
          pause: 1
          action: snapshot
          path: "path/to/save.png"
      </pre>
    </td>  
  </tr>
  <tr>
    <td>upload</td>
    <td>Upload a file to the app</td>
    <td>Required — <br>
      name,<br>
      action,<br>
      locator <br>
      path <br>     
      Optional — <br>
      pause
    </td>
    <td>
      <pre lang="yaml">
        - name: Upload an image
          action: upload
          locator: '#file-upload'
          path: src/example/innerText.txt
      </pre>
    </td>  
  </tr>  
  <tr>
    <td>extract</td>
    <td>
      Extract a text contents from the current window <br>
      By default, Page source of the current window will be extracted <br>
      Locator must also be given only if `extractType` is given
    </td>
    <td>Required — <br>
      name,<br>
      action,<br>
      path <br>
      Optional — <br>
      locator <br>
      extractType — textContents, innerText, innerHTML <br> 
      pause
    </td>
    <td>
      <pre lang="yaml">
        - name: Extract form contents
          action: extract
          path: "path/to/save.txt"
          locator: "form#customer"
          extractType: innerText
      </pre>
    </td>  
  </tr>  
</table>


### Assert Reference

<table>
  <tr>
    <th>Type</th>
    <th>Description</th>
    <th>Keys</th>
    <th>Example</th>
  </tr>
  <tr>
    <td>element</td>
    <td>Assert the expectation by having element(s) on the page</td>
    <td>Required — <br>
      name,<br>
      type,<br>
      locator,<br>
      state (Accepted values: visible, invisible, enabled, disabled, checked, unchecked, containText),<br>
      text (Required only if state = containText)
      Optional — <br>
      pause
    </td>
    <td>
      <pre lang="yaml">
        - name: Verify dropdown selected
          type: element
          locator: "//option[@selected]"
          state: containText
          text: Option 2
      </pre>
    </td>
  </tr>
  <tr>
    <td>text</td>
    <td>Verify if text displayed ( or not displayed)</td>
    <td>Required — <br>
      name,<br>
      type,<br>
      text,<br>
      state (Accepted values: visible, invisible)
      Optional — <br>
      pause
    </td>
    <td>
      <pre lang="yaml">
        - name: verify_upload
          type: text
          text: innerText.txt
          state: 'visible'
      </pre>
    </td>
  </tr>
  <tr>
    <td>snapshot</td>
    <td>Compare the expected screenshot with the actual one on the current screen</td>
    <td>Required — <br>
      name,<br>
      type,<br>
      original,<br>
      reference<br>
      Optional — <br>
      pause
    </td>
    <td>
      <pre lang="yaml">
        - name: Verify failure screen
          type: snapshot
          original: "path/to/screenshot.png"
          reference: "path/to/reference.png"
      </pre>
    </td>
  </tr>
<table>  


## Roadmap

— [X] Browser <br>
— [O] Summary report <br>
— [O] Parameterized tests <br>
— [O] API <br>
— [O] Mobile <br>

## Contribute

## Support

## License