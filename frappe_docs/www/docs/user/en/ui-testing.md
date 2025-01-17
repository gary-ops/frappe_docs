---
add_breadcrumbs: 1
title: Testing
metatags:
 description: >
  Frappe provides some basic tooling to write automated tests. We use Cypress
  for writing Integration Tests.
---

# UI Testing

You can write UI tests using [Cypress](https://cypress.io). It is a NodeJS based
full-stack testing framework which doesn't rely on Selenium.

To write integration tests, create a `.js` file in the `cypress/integration`
directory.

### Example

Here is an example of an integration test to check insertion of a ToDo

```js
context('ToDo', () => {
    before(() => {
        cy.login('Administrator', 'admin');
        cy.visit('/desk');
    });

    it('creates a new todo', () => {
        cy.visit('/app/todo/new-todo-1');
        cy.fill_field('description', 'this is a test todo', 'Text Editor').blur();
        cy.get('.page-title').should('contain', 'Not Saved');
        cy.get('.primary-action').click();
        cy.visit('/desk#List/ToDo');
		cy.location('hash').should('eq', '/app/todo');
        cy.get('.list-row').should('contain', 'this is a test todo');
    });
});
```

### Running Cypress Locally

Cypress uses any chromium based browser installed on your system to run tests.
Every app has it's own cypress test suite. To run test for an app, run the
following command from the `frappe-bench` directory.

```sh
bench --site [sitename] run-ui-tests [app]
```

This will open the Cypress Electron shell where you can run any test manually or
run all of the tests.

<img src="/docs/assets/img/running-cypress-tests.gif" class="screenshot">

You can also run tests in headless mode.

```sh
# run in headless mode
bench --site [sitename] run-ui-tests [app] --headless
```

To enable cypress parallel testing you can pass `--parallel` flag.
More information on how cypress parallel tests work can be found [here](https://docs.cypress.io/guides/guides/parallelization).

```sh
# run tests parallelly
bench --site [sitename] run-ui-tests [app] --parallel
```
