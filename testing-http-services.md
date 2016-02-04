Based on comments from @krimple
```typescript
 beforeEachProviders([
      HTTP_PROVIDERS,
      BaseRequestOptions,
      MockBackend,
      provide(XHRBackend, {useClass: MockBackend}),
      BlogService
    ]);

 it('should get results simple', inject([XHRBackend,BlogService],(mockBackend,blogService) => {
    mockBackend.connections.subscribe(
      (connection: MockConnection) => {
        connection.mockRespond(new Response(new ResponseOptions())
      }

    blogService.getBlogs().subscribe((blogs: BlogEntry[]) => {
        expect(blogs.length).toBe(1);
        expect(blogs[0].id).toBe(25);
      });
  });
```
