- {{renderer excalidraw, excalidraw-2023-05-22-02-37-03}}
- | Lifecycle Hooks| Description |
  | Bean Init| <ul><li>Calling business logic methods</li><li>Setup handles to db, sockets, file, etc...</li></ul> |
  | Bean Destroy| <ul><li>Calling business logic methods</li><li>Cleanup handles to db, sockets, file, etc...</li></ul> |
- **How it works**
	- define custom methods
	- add ((646a6a85-6841-4f53-b7ea-846b8eaa2ed1)) and ((646a6924-4f31-490a-8be5-cb12e5fb73b7)) annotation to the custom methods
-
- #+BEGIN_IMPORTANT
  For Prototype beans, Spring does not call the [destroy method](646a6a85-6841-4f53-b7ea-846b8eaa2ed1)
  Since then, client code must clean up the prototype beans
  #+END_IMPORTANT