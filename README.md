# Code scopes

Code scopes associate **files** in a codebase with a **core concept** or a
**feature**.

They are distinct from the codebase's directory structure. For example, both
`pages/Login.tsx` and `hooks/useLogin.ts` would likely belong to the `auth`
scope.

Avoid overly broad scopes such as `docs` or `test`, as they can be ambiguous and
may conflict with recommended commit types. Instead, use more specific scopes:
`docs(auth): Add documentation for the SSO provider` or
`test(analytics): Add load tests for the data aggregation function`.

## Why code scopes?

- **Maintain a mental model of the codebase**

  Scopes provide a structured way to understand and navigate the code.

- **Incentivize grouping of related files**

  Instead of placing all API calls in a single file such as `lib/api.ts`, you
  can organize them by feature: `lib/auth/api.ts` or `lib/api/analytics.ts`

- **Define commit or pull request scope**

  Code scopes can be used to specify the scope of a
  [commit](#Recommended-commit-message-syntax) or pull request

- **Visualize codebase complexity**

  Tools like [emerge](https://github.com/glato/emerge) could allow you to
  analyze and visualize the complexity of your codebase based on defined scopes

## Recommended commit message syntax

```
<type>(<scope>): <short summary>
  │       │             │
  │       │             └─► Capitalized summary in imperative form.
  │       │                  50 characters or less. No period at the end.
  │       │
  │       └─► Commit scope: One of the scopes defined in the CODESCOPES file.
  │
  └─► Commit type: feat|fix|docs|test|i18n|a11y|log|refactor

```

If files from only one scope are modified in a commit, the commit scope should
be the same as the scope of the modified files.

### Commit types

- **feat**: Add a new feature, with associated tests, translations, logs, and
  documentation
- **fix**: Fix a bug, with associated tests, translations, logs, and
  documentation
- **docs**: Change documentation only
- **test**: Add or correct tests only
- **i18n**: Add or correct translations only
- **a11y**: Add or correct accessibility features only
- **log**: Add or correct observability features only
- **refactor**: Change code without fixing a bug or adding a feature

> Should `perf` be a commit type? In general, I consider performance
> improvements a refactor. However, if a performance improvement is significant
> enough to be considered a feature, it should be classified as a `feat`.

### Examples

```
feat(auth): Add SSO login
fix(auth): Fix login form validation (closes #123)
docs(auth): Add documentation for the SSO providers
test(auth): Add e2e tests for the sign-up flow
i18n(auth): Fix typo in the French translations
a11y(auth): Add aria-label to the login form
log(auth): Send failed login attempts to Sentry
refactor(auth): Move login form validation to a hook
```

## CODESCOPES file

You can use a CODESCOPES file to define code scopes.

To use a CODESCOPES file, create a new file called `CODESCOPES` in the root or
`docs/` directory of your codebase.

### Syntax

A CODESCOPES file uses a pattern that follows most of the same rules used in
[gitignore](https://git-scm.com/docs/gitignore#_pattern_format) files. The
pattern is followed by one scope.

### Example

```gitignore
# This is a comment.
# Each line is a file pattern followed by one scope.

# This scope will be the default scope for everything in the repo.
# Unless a later match takes precedence, all files will be in the core scope.
* core

# Order is important; the last matching pattern takes the most
# precedence. In this example, `.yaml` files are in the ops scope,
# not the core scope.
*.yaml ops

# In this example, any file in the `/build/logs/` directory at the root of
# the repository and any of its subdirectories are in the analytics scope.
/build/logs/ analytics

# The `docs/*` pattern matches files like
# `docs/getting-started.md` but not nested files like
# `docs/build-app/troubleshooting.md`.
docs/* onboarding

# In this example, any file in an `apps/` directory
# anywhere in your repository is in the router scope.
apps/ router

# In this example, any file in the `/docs/`
# directory in the root of your repository and any of its
# subdirectories are in the onboarding scope.
/docs/ onboarding

# In this example, any file in the `/pages/` directory in the root of
# your repository is in the router scope, except for the `/pages/login/`
# subdirectory, as this subdirectory is in the auth scope.
/pages/ router
/pages/login auth
```

## Next steps

- Enforce the commit message syntax in your repository with
  [commitlint](https://commitlint.js.org/)
- Automatically scope commits based on the modified files using a
  [commitizen](https://commitizen-tools.github.io/commitizen/) adapter

## References

[About code owners - GitHub Docs](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
