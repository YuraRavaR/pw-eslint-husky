## 📖 Jira

Link to Jira issue:

## 💬 What or Why

Following changes

## PR review checklist

- [ ] Selectors are not matching multiple elements
- [ ] Changes are passing in all environments
- [ ] No `page.waitForTimeout` used; auto-retrying assertions are preferred
- [ ] Tests use user-facing locators (e.g., `getByRole`, `getByText`) instead of CSS/XPath where possible
- [ ] Test names are descriptive and follow the project conventions
- [ ] Page Object Model is used for reusability and maintenance
- [ ] No `console.log`, `debugger`, or `page.pause()` left in the code
- [ ] Tests are independent and can run in parallel
- [ ] Data created during tests is properly cleaned up
- [ ] Code is properly formatted and passes linting rules
- [ ] Assertions are specific and use web-first assertions (e.g., `toBeVisible`)
- [ ] Tests have been run locally to ensure no flakiness
