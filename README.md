<!-- Project Logo -->
<img src='.github/nodejs-cli-apps-best-practices-logo.svg' width="100px" align="left">

<!-- Main Header Links -->
<div align="right">
	<a href="https://www.github.com/lirantal/nodejs-cli-apps-best-practices" target="_blank">
		<img src="https://badgen.net/badge/Node.js CLI Apps/Best Practices/purple" style="margin:8px;" alt="Node.js CLI Apps Best Practices">
	</a>
</div>

<!-- Project Title -->
<h1>Node.js CLI Apps Best Practices</h1>

A collection of curated best practices on how to build successful, empathic and user-friendly Node.js Command Line Interface applications.

- 8 best practices for building successful Node.js CLI applications
- Read in other languages: [ [🇪🇸](./README-es.md) , [🇩🇪](./README-de.md) ]
- Last update: 2019-12-29
- <a href="https://twitter.com/liran_tal/" alt="follow Liran Tal twitter">Follow me</a> on twitter for more updates

<!-- Shields -->
<p align="center">
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img src="https://badgen.net/badge/License/CC BY-SA 4.0/gray"></a>
<a href="https://itunes.apple.com/us/app/apple-store/id375380948?mt=8" target="_blank">
  <img src="https://img.shields.io/badge/Rating-&starf;&starf;&starf;&starf;&starf;-brightgreen.svg">
</a>
<a href="https://twitter.com/liran_tal/" alt="follow Liran Tal twitter"><img src="https://badgen.net/twitter/follow/liran_tal" /></a>
</p>

<hr/>

<h3>Table of Contents</h3>

- (1) Command Line Experience
  - (1.1) [Respect the POSIX](#respect-the-posix)
  - (1.2) [Build empathic CLIs](#build-empathic-clis)
	- (1.3) [Stateful data](#stateful-data)
- (2) Distribution
  - (2.1) [Prefer a small dependency footprint](#Prefer-a-small-dependency-footprint)
  - (2.2) [Use the shrinkwrap, Luke](#use-the-shrinkwrap-luke)
- (3) Interoperability
  - (3.1) [Accept input as STDIN](#accept-input-as-stdin)
  - (3.2) [Enable structured output](#enable-structured-output)
- (4) Accessibility
  - (4.1) [Containerize the CLI](#containerize-the-cli)
  - (4.2) [Graceful downplay](#graceful-downplay)
  - (4.3) [Node.js versions compatibility](#node.js-versions-compatibility)
  - (4.4) [Shebang autodetect the Node.js runtime](shebang-autodetect-the-nodejs-runtime)
- (5) Testing
- (6) Errors
  - (6.1) [Informational errors](#informational-errors)
  - (6.2) [Actionable errors](#actionable-errors)
  - (6.3) [Provide debug mode](#provide-debug-mode)

## (1) Command Line Experience

This section deals with best practices concerned with creating beautiful and high-value user experience Node.js command line applications.

### 1.1. Respect the POSIX

✅ **Do:**
Use [POSIX-compliant](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html) command line argument syntax, which is widely accepted as a standard for command line tools.

❌ **Otherwise:**
Users may get frustrated when CLIs syntax for arguments, options or command parameters deviate from defacto standards that they are used to from the Unix world.

<details>
	<summary>➡️ <b>Details</b></summary>

Unix-like operating systems popularized the use of the command line and tools such as `awk`, `sed`. These tools have effectively standardized the behavior of command line arguments, such as square brackets (`[]`) refer to optional arguments, or angle brackets (`<>`) refer to required arguments.

Command line power users will expect another CLI tool to have similar conventions as to others in the Unix family.

</details>

### (1.2) Build empathic CLIs

✅ **Do:**
Put workflows in place that assist the user to interact with the CLI successfully when otherwise such interactions will result in errors and frustration.

❌ **Otherwise:**
Failing to provide actionable assistance in supporting the user will result in frustration due to the lack of capability to operate the CLI.

<details>
	<summary>➡️ <b>Details</b></summary>

A command line interface for your program is no different than a web user interface in the sense of doing as much as you can as the program author to ensure that it is being used successfully.

Optimize for successful interactions by building empathic CLIs that support the user. As an example, let's explore the case of the `curl` program that expects a URL as its primary data input and the user failing to provide it. Such failure will lead to reading through a (hopefully) descriptive error messages or reviewing a `curl --help` output. However, an empathic CLI would have presented an interactive prompt to capture input from the user, resulting in a successfull interaction.

</details>

### (1.3) Stateful data

✅ **Do:**
Provide a stateful experience between numerous invocations of the CLI app and remember values and data in order to provide a seamless and stateful interaction.

❌ **Otherwise:**
Requiring the user to repeatedly provide the same information with multiple invocations of the CLI is cumbersome and unuseful.

<details>
	<summary>➡️ <b>Details</b></summary>

It may happen that you find yourself needing to provide storage persistence for your CLI application, such as remembering a username, email, API token or other preferences between multiple invocations of the CLI. Use a configuration helper that allows to persist such user settings that your CLI is able to read and write to. 

</details>

## (2) Distribution

This section deals with best practices concerned with distributing and packaging a Node.js command line application in an optimal matter for consumers.

### (2.1) Prefer a small dependency footprint

✅ **Do:**
Minimize your use of production dependencies, use alternative dependencies which are smaller, and vet your dependencies footprint as well for transitive dependencies cost to ensure an overall small bundle of the Node.js CLI. Be careful to not over-optimize on dependencies by reinventing the wheel.

❌ **Otherwise:**
The size and use of dependencies in the application will impact the install time of your Node.js CLI, potentially providing a poor user experience.

<details>
	<summary>➡️ <b>Details</b></summary>

A fast npm install for Node.js CLIs invoked with `npx` will provide a better user experience. This is made possible when the overall dependency and transitive dependency footprint is kept to a reasonable size.

Where-as with a global npm installation of a package, a slow-to-install npm package will be a one-off poor experience, the use of `npx` for users to invoke executable packages is more significant and visible in its degraded performance due to npx always fetching and installing packages from the registry.

</details>

### (2.2) Use the shrinkwrap, Luke

✅ **Do:**
Use npm's `npm-shrinkwrap.json` as a lockfile to ensure that pinned-down dependency versions (direct and transitive) propagate to your end users when they install your npm package.

❌ **Otherwise:**
Not forcing the versions of package dependencies will mean that the package manager (e.g. `npm`) will resolve them during installation, and transitive dependencies installed via version ranges may introduce breaking changes that you can't control, resulting with a potential breakage of a Node.js CLI application.

<details>
	<summary>➡️ <b>Details</b></summary>

Use the ~~force~~ shrinkwrap, Luke!

Typically, an npm package only defines its direct dependencies and their version range when being installed, and the npm package manager client will resolve all the transitive dependencies versions upon installation. Through time, the resolved versions of dependencies will vary as new direct and transitive dependencies will release new versions.

Even though [Semantic Versioning](https://semver.org/) is broadly accepted among maintainers, npm is [known to introduce many dependencies](https://snyk.io/blog/how-much-do-we-really-know-about-how-packages-behave-on-the-npm-registry/) for an average package being installed, which adds the risk of a package introducing breaking changes or regressions that render an application dependent on these as non-functional.

The flip side of using `npm-shrinkwrap.json` is the security implications you are forcing upon your consumers. The versions of dependencies being installed are pinned to specific versions, and so even if newer versions of these dependencies are released they won't be installed. This moves the responsibility on you, the maintainer, to stay up to date with security fixes in your dependencies, and release the CLI application regularly with security updates. Consider using [Snyk Dependency Upgrade](https://snyk.io/) to automatically fix security issues across your dependency tree. _Full disclosure: I am a developer advocate at Snyk._

> 👍 Tip
>
> Use `npm shrinkwrap` command to generate the shrinkwrap lockfile, which is of the same
> format as that of a `package-lock.json` file.

References:

- [Do you really know how a lockfile works for yarn and npm packages?](https://snyk.io/blog/making-sense-of-package-lock-files-in-the-npm-ecosystem/)
- [Yarn docs: Should lockfiles be committed to the repository?](https://next.yarnpkg.com/advanced/qa#should-lockfiles-be-committed-to-the-repository)

</details>

## (3) Interoperability

This section deals with best practices concerned with making the Node.js CLI seamlessly integrate with other command line tools and conventions which are natural for CLIs to operate in.

This section answer questions such as:

- _Can I export the output of this CLI for easy parsing?_
- _Can I pipe the output of this CLI to the input of another command line tool?_
- _Can I pipe the result of another tool to this CLI?_

### (3.1) Accept input as STDIN

✅ **Do:**
For command line applicatios that are expected to work with data, make it available for a consumer to pipe the data to standard input (STDIN).

❌ **Otherwise:**
Other command line applications will not be able to provide their result, directly as input for the CLI you build and it prevents common UNIX one-liners such as:

```bash
$ curl -s "https://api.example.com/data.json" | your_node_cli
```

<details>
	<summary>➡️ <b>Details</b></summary>

If the command line application works with data, such as performing some kind of task on a JSON file, which is usually specified with `--file <file.json>` command line argument

</details>

### (3.2) Enable structured output

✅ **Do:**
Enable a flag to allow structured output of the program's result, if such result is available, to enable parsing and easy manipulation of the data.

❌ **Otherwise:**
Users of the CLI may need to apply complicated regex parsing and matching techniques to extract the output data provided by your CLI.

<details>
	<summary>➡️ <b>Details</b></summary>

It is often useful for users of a command line application to parse the data and perform other tasks with it, such as using it to feed web dashboards, email notifications.

Being able to easily extract the data of interest from a command line output provides a friendlier experience to consumers of the CLI.

</details>

## (4) Accessibility

This section deals with best practices concerned with making a Node.js CLI application available to users who wish to consume it but are lacking an ideal environment than that which the maintainer designed the application.

### (4.1) Containerize the CLI

✅ **Do:**
Create a docker image for the CLI and publish it to a public registry like Docker Hub so that users without a Node.js environment can use it.

❌ **Otherwise:**
Users without a Node.js environment will not have `npm` easily available and won't be able to run your CLI application.

<details>
	<summary>➡️ <b>Details</b></summary>

Installing Node.js CLI applications from the npm registry will typically be done with Node.js native toolchain such as `npm` or `npx`. These are common across JavaScript and Node.js developers, and are expected to be referenced within install instructions.

However, if you are targeting a CLI application to be consumed by the general public, regardless of their affiliation with JavaScript or availability of this tooling, then distributing the CLI application only in the form of an install from the npm registry will be restricting. Moreover, if the CLI application is intended to be used in a build or CI environment then those may also be required to install Node.js related toolchain dependencies.

There are many forms of packaging and distributing an executable and containerizing it as a Docker container that is pre-bundled with your CLI application is an easily consumable alternative and dependency-free (aside of requiring a Docker environment ready).

</details>

### (4.2) Graceful downplay

✅ **Do:**
Provide users with the ability to opt-out of colorful and animation-rich display in unsupported environments, such as by skipping interactivity and providing formatted output in the form of JSON.

❌ **Otherwise:**
Having colorful output, using terminal interactive such as prompts and other display-rich interfaces may significantly degrade the end user experience for users not having a supported terminal and deter them away from using the CLI application.

<details>
	<summary>➡️ <b>Details</b></summary>

It is common to provide a rich terminal display in the form of colorful output, ascii charts, or even animation on the terminal and powerful prompt mechanism. These may contribute to a great a user experience for those who have a supported terminal, however it may display garbled text or be completely inoperable for those without it.

To enable users with unsupported terminal to properly use the Node.js CLI application, one may choose to:

- Auto-detect a terminal capability and evaluate during run-time whether to downplay the CLI interactivity
- Provide an opt-in for users to explicitly toggle a graceful downplay such as by providing a `--json` command line argument to force it.

```
👍	Tip

	Supporting a graceful downplay such as JSON output isn't only useful for
	end-users and their unuspported terminals but is also valuable for running
	in build and continuous integration environments, as well as enabling users
	the ability to connect your program's output with other program's input or
	export data if needed.
```

</details>

### (4.3) Node.js versions compatibility

✅ **Do:**
Target old versions of Node.js such as Node.js 6 or Node.js 4 (both are End of Life) using a transpiler such as Babel to make sure the generated code is compliant with the version of V8 JavaScript engine shipped with those versions.

Another workaround is to provide a container version of the CLI to avoid old targets. See Section [(4.1) Containerize the CLI](#containerize-the-cli).

❌ **Otherwise:**
Don't level down the program code to use an older ECMAScript language specification that matches unmaintained or EOL Node.js versions that you target as this will only lead to complex code maintenance in time and will warrant a technical debt from the beginning.

<details>
	<summary>➡️ <b>Details</b></summary>

Sometimes it may be necessary to specifically target older Node.js versions which are missing new ECAMScript specification. For example, if you are building a Node.js CLI that is mostly geared towards DevOps or IT, they may not have an ideal Node.js environment with an up to date runtime. As a reference, Debian Stretch (oldstable) ships with [Node.js 8.11.1](https://packages.debian.org/search?suite=default&section=all&arch=any&searchon=names&keywords=nodejs).

</details>

### (4.4) Shebang autodetect the Node.js runtime

✅ **Do:**
Use an installation-agnostic reference in your [Shebang](<https://en.wikipedia.org/wiki/Shebang_(Unix)>) declaration that locates the Node.js runtime automaticaly based on the runtime environment.

❌ **Otherwise:**
Using a hard-coded Node.js runtime location such as `#!/usr/local/bin/node` is only specific to your own environment and may render the Node.js CLI inoperable in other environments where the location of Node.js is different.

<details>
	<summary>➡️ <b>Details</b></summary>

It may be an easy start to develop a Node.js CLI by running the entry point file as `node cli.js`, and later on adding `#!/usr/local/bin/node` to the top of the `cli.js` file, however the latter is still a flawed approach to the matter as that filesystem location of the `node` executable is not guaranteed for other users environments.

It should be noted that specifying `#!/usr/bin/env node` as the best practice, is still making the assumption that the Node.js runtime is referrenced as `node` and not `nodejs` or otherwise.

</details>

<hr/>

# Author

**Node.js CLI Apps Best Practices** © [Liran Tal](https://github.com/lirantal), Released under [CC BY-SA 4.0](./LICENSE) License.

# License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
