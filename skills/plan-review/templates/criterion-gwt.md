# Given/When/Then template

```gherkin
Scenario: <short name of the behaviour>
  Given <initial state, actor, data>
  When <action of the actor or event>
  Then <observable, measurable outcome>
  And <further outcome, optional>
```

Per story at least: one happy-path scenario, one error/negative scenario, one edge case.

## Example
```gherkin
Scenario: Password reset with unknown email
  Given no account exists for "x@example.com"
  When I enter "x@example.com" under "Forgot password" and submit
  Then I see the same confirmation as for a known address
  And no email is sent
```
