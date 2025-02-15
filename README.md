# AppVersioning

# AppVersioning

Sample app that utilizes [changesets](https://changesets-docs.vercel.app/en) for auto-increment version that follows [semver](https://semver.org/).

## Setting up changesets
Changesets is a tool that helps you manage your versioning and changelog. It allows you to create a changeset file that describes the changes you've made to your codebase. This file can then be used to automatically update your version and changelog.

To set up changesets in your project, you need to install the changesets package:

```bash
npx init @changesets/cli
```

This will create a `.changeset` directory in your project, which will contain all of your changeset files.

## Using changesets
In order to use changsets, you need to add a script to your `package.json` file that runs the changeset commands. Here's an example script that runs the changeset commands:
```json
"scripts": {
  "changeset": "changeset",
  "version": "changeset version",
  "update-version": "npm run version && git add . && git commit -m 'Version Packages' && git push"
}
```

This script defines three commands: `changeset`, `version`, and `update-version`. The `changeset` command runs the changeset CLI, the `version` command applies the changesets to your project, and the `update-version` command commits the changes to your repository.

To create a new changeset, you can run the following command:
```bash
npx changeset
```

This will open an interactive prompt that will guide you through creating a new changeset. You can specify the type of change (major, minor, or patch), a summary of the change, and any additional details.

Once you've created a changeset, you can run the following command to apply the changeset to your project:
```bash
npx changeset version
```

This will update your version number and changelog based on the changesets that have been created.


## Running changesets in GitHub Actions

To automatically update your version and changelog using changesets, you can set up a GitHub Actions workflow that runs the changeset commands. Here's an example workflow file that runs the changeset commands on every push to the `main` branch:

```yaml

 - name: Update Version
   run: npm run update-version
   env:
     GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

- name: Sync Changesets Version
  shell: pwsh
  run: |
    $package = Get-Content -Raw ./package.json | ConvertFrom-Json
    $version = $package.version

    $xml = [Xml] (Get-Content ./AppVersioning.csproj)
    $xml.Project.PropertyGroup.ApplicationDisplayVersion = $version
    $xml.Save("./AppVersioning.csproj")
```

This workflow file will run the `npx changeset version` command on every push to the `main` branch, which will apply any changesets that have been created.

## Git Actions Workflow

1. PR 
2. Build
3. Version
4. Deploy