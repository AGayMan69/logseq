- #+BEGIN_NOTE
  [Spring Container](Spring Boot/Concept/Spring Container) default only create one instance of the [Bean](Spring Boot/Concept/Bean) **([[Singleton]])**
  #+END_NOTE
- | Scope | Usage |
  |---|---|
  | `singleton` | Create a single shared instance of the [Bean](Spring Boot/Concept/Bean) ***Default** |
  | `prototype` | Create a new instance of the [Bean](Spring Boot/Concept/Bean) for each container request (*e.g. injection*) |
  | `request` | Only existed(scoped) to an HTTP request ***webapp only** |
  | `session` | Only existed(scoped) to an HTTP session ***webapp only** |
  | `global-session` | Only existed(scoped) to a global HTTP session ***webapp only** |