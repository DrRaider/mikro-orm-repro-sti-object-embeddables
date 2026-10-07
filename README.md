# MikroORM reproduction: STI object embeddables with different embeddable classes

STI subtypes share one JSON column (`config`) through `@Embedded({ object: true })`, but declare different
embeddable classes (`ModuleA`/`ModuleC` use `BasicConfig`, `ModuleB` uses `ExtendedConfig`). Loading the
entities through the STI root (`em.findAll(Module)`) hydrates `config` as an empty embeddable (`BasicConfig {}`),
although the data is persisted correctly. Loading through the subtype works.

Uses the PGlite driver (in-process Postgres), no database setup needed:

```sh
npm install
npm test
```

Reproduces on 7.1.0 through 7.2.4 and current `master`. The same model worked on v6.

---

# MikroORM reproduction example

This repository serves as a base reproduction example, it contains basic setup with MikroORM 7 with SQLite driver and vitest. This is what the main repository is using, and therefore allows for a simple integration to the codebase.

## Few hints for creating your own reproductions

- Focus on reproducing one problem at a time.
- Set up the data, the test needs to be **self-contained**.
- Don't use the CLI, do everything programmatically as part of the test.
- Remove everything unrelated, keep things as simple as possible.
- Duplication in tests is fine, better than complex abstractions.
- Comments are fine, asserts are better!
- If the problem is not driver specific, use in-memory SQLite database.
