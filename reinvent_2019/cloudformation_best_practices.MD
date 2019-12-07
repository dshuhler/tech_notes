### Cloud formation best practices  
By releasing once a week we are in top 10% (this disputes other stats I have heard at this conference)

Infrastructure as code: ALL configuration should be version controlled

We should use a linter for our template (cfn-lint?)

Add autocompletion: aws-cloudformation-schema (didn't get full address, appeared to integrate w/ intellij)

authoring best practices:
- parameterization - use integration with secrets manager and parameter store
- you can group, label, and add descriptions to parameters
- mappings: use where you would use a case statement
- use !Sub over !Join
- use imports and exports
- leverage ssm parameter store for latest AMI instance IDs

infra as code allows auditability  
testing your templates:

 "the more serverless you get, the less code you can usefully unit test, and the more you have to rely on tests of your deployed infrastructure"- Forrest Brazel

 lint headlessly as part of your build pipeline. fail if it fails. add custom linting rules to enforce policy  
 *** we could add a linter that fails if the required flags are omitted

 TaskCat - testing tool
  - tests templates by creating stacks in multipole AWS regions simultaneously
  - good for testing same stack accross different regions - not all regions have all features so might succeed in one but fail in another

deploying you application
  - change sets (appears to be a GUI in the console?)
    - lists out the different things that will happen to the different resources in your stack
    - Have someone else look at the changes to verify they are not dangerous

they love saying "minimize blast radius"

new feature: resource import. allows you to bring a resource that was created outside cloudformation without having to delete/recreate it

treat stars in IAM policy as spiders: squish them
