# 10. Placeholder content
Unfinished or sample material left in work that is supposed to be done.
- Look for: stub bodies (`pass`, `...`, `raise NotImplementedError`, `// TODO: implement`) in code that callers reach; fake sample values (`example.com`, `foo`, `bar`, `John Doe`, `test@test.com`, `123456`) outside test fixtures and docs; generic UI text that was never replaced ("Welcome to the App!", "Get Started", "Lorem ipsum", "Your content here").
- Fix: delete stubs nothing reaches. Hold reachable stubs, real-looking values, and visible UI text for review, since replacing them changes behavior or what the user sees. Sample values in tests are fine when clearly marked as fixtures.
