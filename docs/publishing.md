# Publishing

## Regular releases

> [!NOTE]
> This information is for [project owners](../GOVERNANCE.md#roles-and-responsibilities).

These are the steps to publish a regular release on npm and as a GitHub release:

1. Determine if it should be a major, minor, or patch release.

   A major version is required for releases when:

   - Previously valid references are invalid (for example, a group ID is renamed or a feature ID is removed).
   - Types have incompatibly narrowed, widened, or otherwise changed (for example, a string value now accepts an array of strings or an ID has become a URL). Changes to `data.schema.json` often indicates a major or minor version is required.

   A minor version is required for releases that contain only additions, such as new feature or new properties on existing types.

   Patch versions are required for releases that contain only routine data changes, such as updates to `compat_features` arrays or `support` objects.

   The "[major version required][major-version]" and "[minor version required][minor-version]" labels should be used to support this decision.

1. Trigger the [Prepare web-features release workflow](https://github.com/web-platform-dx/web-features/actions/workflows/prepare_release.yml).

   1. Click the **Run workflow** dropdown.
   1. Choose the semver level.
   1. Click the **Run workflow** button.

   When the workflow finishes, your review is requested on a new release pull request.

1. Review the PR.
   
   1. Close and reopen the relase PR, to allow the tests to run.
   1. Review and approve the changes.
   1. When you're ready to complete the remaining steps, merge the PR.

1. Create the GitHub release.

   1. Go to https://github.com/web-platform-dx/web-features/releases/new to start a new draft release.
   1. Fill in the tag name `vX.Y.Z` manually as both the tag and release title.
   1. For minor releases, add a `## What's New` section to the top of the release notes, before all other sections.
   1. For major releases, add a `## Breaking Changes` section to the top of the release notes, before all other sections.
   1. Click **Generate release notes**.
   1. In the release description, find unescaped `<` characters and make sure that HTML elements are enclosed with backticks.

      This regular expression can help:

      ```regex
      / (?<!`)<(.*?)> /
      ```

   1. Remove all lines from Dependabot.
   1. Click **Publish release**.

   Publishing the GitHub release creates the tag. This triggers the [Publish web-features GitHub Actions workflows](https://github.com/web-platform-dx/web-features/blob/main/.github/workflows/publish_web-features.yml), uploads GitHub release artifacts and publishes the package to npm.

1. Remove the "[major version required][major-version]" and "[minor version required][minor-version]" labels from any pull requests that were included in the release.

1. (*Optional*) If this release contained schema changes, notify highly-visible donwstream consumers, such as Can I Use (@Fryd), MDN (@LeoMcA), or webstatus.dev (@jcscottiii).

[major-version]: https://github.com/web-platform-dx/web-features/pulls?q=is%3Apr+is%3Amerged+label%3A%22major+version+required%22+sort%3Aupdated-desc
[minor-version]: https://github.com/web-platform-dx/web-features/pulls?q=is%3Apr+is%3Amerged+label%3A%22minor+version+required%22+sort%3Aupdated-desc

## `@next` releases

The [Publish web-features@next GitHub Actions workflows](https://github.com/web-platform-dx/web-features/blob/main/.github/workflows/publish_next_web-features.yml) automatically publish pre-releases on push to the main branch using the `next` [npm dist tag](https://docs.npmjs.com/adding-dist-tags-to-packages).
You can install these prereleases using a command such as `npm install web-features@next`.

## Secrets

> [!NOTE]
> This information is for [project owners](../GOVERNANCE.md#roles-and-responsibilities).

Publishing requires the `NPM_TOKEN` repository secret (set via _Settings_ → _Secrets and variables_ → _Actions_).
If you're replacing this token, then use the following settings:

| Setting                          | Value                                                                                                      |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| Token type                       | _Granular Access Token_                                                                                    |
| Expiration                       | The first day of the next quarter (1 January, 1 April, 1 July, or 1 October) or the first weekday after it |
| Packages and scopes permsissions | _Read and write_                                                                                           |
| Select packages                  | _Only select packages and scopes_                                                                          |
| Select packages and scopes       | `compute-baseline` and `web-features`                                                                      |
| Organizations                    | _No access_                                                                                                |
