# Cadasta's Documentation System

Cadasta utilizes [GitBook](https://github.com/GitbookIO/gitbook) to generate its online documentation. Note that this is no longer the same as using the [GitBook Version 2 service](https://gitbook.com), which (at time of writing) no longer supports multi-language books or exporting books to PDF.[<sup>[0]</sup>](https://docs.gitbook.com/v2-changes/important-differences)

Information about the inner-workings of GitBook can be found in the [GitBook Toolchain Documentation](https://toolchain.gitbook.com/). Useful sections include:

- [Support for multiple languages](https://toolchain.gitbook.com/languages.html)
- [Generating eBooks and PDFs](https://toolchain.gitbook.com/ebook.html)

## Editing

All documentation content is written in Markdown and can be edited in any standard text editor or the GitBook Editor.

All documentation content is stored in GitHub. Regardless of method, to make changes to Cadasta documentation you must have a GitHub account.

### GitBook Editor

The [GitBook Editor](https://legacy.gitbook.com/editor) simplifies the process of editing content if you are not familiar with Markdown.

Read more about setting up GitBook [here](./gitbook-editor-getting-started.md).

## Deploying Changes

Being that this content is all managed via Git/GitHub, we follow [standard best practices](https://guides.github.com/introduction/flow/) with code management:

1. Changes should be make in a separate branch. GitBook makes it easy to work in a separate branch via its branch controls in the top-left corner.
2. When you feel that your changes are ready to be deployed, make a [Pull Request](https://help.github.com/articles/proposing-changes-to-your-work-with-pull-requests/). If you are unfamiliar with this process, contact a team-member for support.
3. A teammate will code-review your changes for grammar and style and, upon acceptance, merge the changes into the `master` branch.
4. Changes to the `master` branch will trigger an automatic build process, deploying your changes within a few minutes.
