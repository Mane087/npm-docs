# npm docs

## `--save`

This flag indicates that the package should be installed as a **production dependency**.

Production dependencies are packages required for the application to **run in production**.

Example:

```bash
npm install express --save
```

This adds the dependency to the `dependencies` section in `package.json`.

Example:

```json
{
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

Important detail:
Since **npm v5**, `--save` is **no longer required** because it is applied automatically when installing a package.

Equivalent command today:

```bash
npm install express
```

---

## `--save-dev`

This flag installs the package as a **development dependency**.

Development dependencies are only needed during development, such as:

* Testing frameworks
* Linters
* Build tools
* Type checkers
* Transpilers

Example:

```bash
npm install typescript --save-dev
```

This adds the package to the `devDependencies` section:

```json
{
  "devDependencies": {
    "typescript": "^5.0.0"
  }
}
```

Typical examples of dev dependencies:

* `eslint`
* `jest`
* `typescript`
* `webpack`
* `vite`
* `nodemon`

These tools help **build, test, or validate code**, but they are not needed when the application is running in production.

---

# Security and Dependency Auditing

## `npm audit`

This command checks your installed dependencies for **known security vulnerabilities**.

It compares your dependency tree against vulnerability databases maintained by npm.

Example:

```bash
npm audit
```

The report includes:

* Severity level (low, moderate, high, critical)
* Vulnerable package
* Dependency path
* Suggested fix

---

## `npm audit fix`

Attempts to automatically fix vulnerabilities by upgrading dependencies.

Example:

```bash
npm audit fix
```

Important behavior:

* Only applies **safe and compatible updates**
* Follows **Semantic Versioning rules**
* Does **not upgrade major versions automatically**

This prevents breaking changes in your project.

---

## `npm audit fix --force`

Forces npm to install the latest versions even if they contain **major version changes**.

Example:

```bash
npm audit fix --force
```

Warning:

This may break your application because APIs may change between major versions.

In production projects, this should be used carefully.

---

# Dependency Updates

## `npm update`

Updates installed packages to the **latest version allowed by the version range in `package.json`**.

Example:

```bash
npm update
```

Example scenario:

If your package.json contains:

```json
"express": "^4.17.0"
```

Then `npm update` may upgrade it to:

```
4.18.2
```

But **not to version 5.x**, because that would violate the semver constraint.

---

## `npm outdated`

Displays packages that have newer versions available.

Example:

```bash
npm outdated
```

The output usually includes:

| Column  | Meaning                                    |
| ------- | ------------------------------------------ |
| Current | Installed version                          |
| Wanted  | Latest version matching package.json rules |
| Latest  | Latest version published                   |

Example output:

```
Package   Current  Wanted  Latest
express   4.17.1   4.18.2  5.0.0
```

This helps identify packages that may require manual upgrades.

---

# Version Management

## `npm version <major | minor | patch>`

Updates the version number of your project following **Semantic Versioning (SemVer)**.

Example:

```bash
npm version patch
```

Version types:

| Type  | Meaning                           |
| ----- | --------------------------------- |
| patch | Bug fixes                         |
| minor | New features, backward compatible |
| major | Breaking changes                  |

Example progression:

```
1.2.3 → patch → 1.2.4
1.2.3 → minor → 1.3.0
1.2.3 → major → 2.0.0
```

This command also:

* Creates a **git commit**
* Creates a **git tag**

---

# Package Removal

## `npm uninstall`

Removes a dependency from the project.

Example:

```bash
npm uninstall lodash
```

This:

* Removes it from `node_modules`
* Removes it from `package.json`

---

# Cache Management

## `npm cache clean --force`

Clears npm’s local cache.

Example:

```bash
npm cache clean --force
```

Useful when:

* Packages fail to install
* Cache corruption occurs
* Registry metadata becomes inconsistent

However, clearing the cache is rarely necessary with modern npm versions.

---

# Dependency Tree Maintenance

## `npm prune`

Removes packages in `node_modules` that are **not listed in `package.json`**.

Example:

```bash
npm prune
```

This can happen when:

* Dependencies are manually deleted from `package.json`
* Installation processes fail
* Old modules remain in the tree

`npm prune` ensures the dependency tree matches your manifest.

---

# Dependency Inspection

## `npm list`

Displays the dependency tree installed in your project.

Example:

```bash
npm list
```

You can also check a specific package:

```bash
npm list express
```

Or view only top-level dependencies:

```bash
npm list --depth=0
```

This is useful for debugging dependency conflicts.

---

# Important Concept: Semantic Versioning (SemVer)

Most npm behavior relies on **Semantic Versioning**.

Version format:

```
MAJOR.MINOR.PATCH
```

Example:

```
2.4.1
```

Meaning:

* **MAJOR** → breaking changes
* **MINOR** → new features, backward compatible
* **PATCH** → bug fixes

Common version prefixes in `package.json`:

| Prefix   | Meaning                        |
| -------- | ------------------------------ |
| `^1.2.3` | Allows minor and patch updates |
| `~1.2.3` | Allows only patch updates      |
| `1.2.3`  | Exact version only             |

---

# A Practical Observation

Modern JavaScript applications often depend on **hundreds or thousands of packages**.

For example:

```
Your project → 20 dependencies
Those dependencies → 200 sub-dependencies
```

This is why tools like:

* `npm audit`
* `npm outdated`
* `Dependabot`

exist. They help maintain the **health of the dependency ecosystem** in a project.

Left unmanaged, dependency trees slowly decay into security risks and compatibility problems. In large systems, dependency management becomes a discipline of its own.
