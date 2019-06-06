There will come a day when you need to make breaking changes to your addon. How can you guide your users through the changes? Follow these steps to create deprecation warnings, give your users enough information to resolve them, and test that your deprecations work as expected.

## What is a deprecation warning?

A deprecation warning is a message printed to the console that informs a developer that they need to refactor part of their app to accommodate upcoming breaking changes to a library they use.

## When to write a deprecation warning

Deprecation warnings should be written whenever you know that you want to remove or change a feature of your addon.

You can create deprecations at any time, but to provide the best experience for other developers, it's a good idea to create a deprecation only once you have a workaround available.

For example, a workaround could be instructions about how to use your other existing features to accomplish the same thing, or a new API method that developers should migrate to.

## How to make a deprecation warning

You can create a deprecation warning that looks and works the same as the deprecations used by Ember itself:

```js
import { deprecate } from '@ember/application/deprecations';
// ...

yourDeprecatedMethod() {
    deprecate('You used yourDeprecatedMethod in my-addon-name, which is deprecated. Here are instructions to resolve this...', 
        false,
        {
            id: 'your-deprecation-id',
            until: '2.0.0',
            url: 'link-to-your-deprecations-instructions'
        }
    );
    // code that should run for the deprecated method
}
// ...
```

The most important reason to use Ember's built in [`deprecate`](https://api.emberjs.com/ember/release/functions/@ember%2Fapplication%2Fdeprecations/deprecate) method is that the warning will be silenced in production.
If a customer opens up the console for an app that uses your addon, they shouldn't see anything!
Developers can configure whether it shows up in other build environments as well, like testing and local development.

## Testing the deprecation warning

Whenever you add a deprecation, it's important to add a couple test cases that would trigger the warning. You should also write tests for the "new" way to use the addon.
It's a good idea to make these examples match what is in your `README` or API docs, so that you know the advice you are giving users actually works.

If you want to test if the deprecation warning itself fires at the right times, you can use community addons like [ember-qunit-assert-helpers](https://github.com/workmanw/ember-qunit-assert-helpers#emberdeprecate-assertions).

After you install the addon with `ember install ember-qunit-assert-helpers`, you can use the `assert.expectDeprecation()` anywhere in your tests where you already use `qunit`. For examples and more ways to use `expectDeprecation`, see the documentation for that addon.

## Documenting the deprecation

There are a few different ways to document the deprecation.
First, and most importantly, make sure your deprecation warning itself is easy to understand, clearly states which addon it comes from, and has brief instructions for resolving the deprecation.

You should also add information to your `README`, a `CHANGELOG`, or make a dedicated `DEPRECATIONS.md` file that you can link to in your deprecation warning.
For larger projects, you may even want to have a separate section in your documentation that is just for deprecations.
Keep in mind that you don't want your library to look like it is swimming in deprecations, so avoid notes about deprecation caveats in your new-user-facing materials, like quick start guides.

## Removing deprecated features

The best time to remove a deprecated feature is when you do a major version, such as going from `v1.3.4` to `2.0.0`.

Removing a deprecation is a "breaking change" and developers rely on each segment of the version numbers to know whether a dependency upgrade is safe. The first number, the "major" version, is for breaking changes. The second number, the "minor" version, is for adding new features. Lastly, the "patch" version is for bugfixes and refactors. This system is referred to as [SemVer](https://semver.org/), and it is the standard for packages across the JavaScript ecosystem.

If you haven't done a `v1.0.0` release yet, you can make breaking changes in the "minor" version number, such as `0.12.2`.
Most developers understand that a package that is pre-1.0 will have lots of breaking changes.

### Checklist for removing a deprecation

Here are some things to look over when you are ready to remove a deprecation:

- Did you take out all the deprecated code and the warning?
- Did you include an entry in your `CHANGELOG.md`, `README.md`, or release notes to let your users know that the feature is removed?
- Do you have good test coverage of the new feature? Does your test coverage include examples similar to those in your migration docs?
- Do the `README.md` or API docs have any outdated information?
- Does the `README` or API docs clearly show how to use the new feature or approach?
- Are you doing a major release?
- Did you check your Issue queue to make sure that you didn't miss any user bug reports related to the new feature or migrations?