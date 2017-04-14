# [LMS-1] - Registering new node
As a user I should be able to register a new module in LMS and view the module once it has been registered.

### Acceptance criteria
As a user I should be able to:
 - submit a URL to a new learning node in remote git repository via simple web form, then
     - gain access to a rendered node in browser via readable link
 - when trying to access an unregistered module I should see a 404 error page;
 - when trying to submit an invalid remote git URL I should get 422 error page (Unprocessable Entity) with a message about an invalid URL.