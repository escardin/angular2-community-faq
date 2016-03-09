
[A solution from @jgongo](https://gitter.im/angular/angular?at=56a915aa80ad69394a7ad156) with some additional changes based on [comments from @krimple](https://gitter.im/angular/angular?at=56913628d739f50a36029ef7)
```typescript
beforeEachProviders([
 HTTP_PROVIDERS,
 MockBackend,
 provide(XHRBackend, {useClass: MockBackend}),
 AuthenticationService
]);


it('should return an error upon unsuccessful authentication', injectAsync([XHRBackend, AuthenticationService], (mockBackend, authenticationService) => {
 return new Promise((resolve, reject) => {
  mockBackend.connections.subscribe((connection: MockConnection) => {
   connection.readyState = ReadyState.Done;
   connection.response.error(new Response(new ResponseOptions({
   body: {error: 'invalid_grant', error_description: 'Incorrect username and / or password'},
   status: 400
  })));
  });
   
  authenticationService.authenticate({username: 'john.doe', password: '12345'}).subscribe(
   (oauthTokenResponse) => {
    reject(Error('Unexpected successful response'));
   },
   (error: Response) => {
    expect(error.status).toBe(400);
    expect(error.json().error).toBe('invalid_grant');
    expect(error.json().error_description).toBe('Incorrect username and / or password');
   }
  );
 });
}));
```
