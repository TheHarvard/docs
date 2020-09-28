---
title: Configuring npm for use with GitHub Packages
intro: 'Sie können NPM für die Veröffentlichung von Paketen auf {{ site.data.variables.product.prodname_registry }} und für die Nutzung von auf {{ site.data.variables.product.prodname_registry }} gespeicherten Paketen als Abhängigkeiten in einem NPM-Projekt konfigurieren.'
product: '{{ site.data.reusables.gated-features.packages }}'
redirect_from:
  - /articles/configuring-npm-for-use-with-github-package-registry
  - /github/managing-packages-with-github-package-registry/configuring-npm-for-use-with-github-package-registry
  - /github/managing-packages-with-github-packages/configuring-npm-for-use-with-github-packages
versions:
  free-pro-team: '*'
  enterprise-server: '>=2.22'
---

{{ site.data.reusables.package_registry.packages-ghes-release-stage }}

**Note:** When installing or publishing a docker image, {{ site.data.variables.product.prodname_registry }} does not currently support foreign layers, such as Windows images.

### Bei {{ site.data.variables.product.prodname_registry }} authentifizieren

{{ site.data.reusables.package_registry.authenticate-packages }}

#### Authenticating with a personal access token

{{ site.data.reusables.package_registry.required-scopes }}

You can authenticate to {{ site.data.variables.product.prodname_registry }} with npm by either editing your per-user *~/.npmrc* file to include your personal access token or by logging in to npm on the command line using your username and personal access token.

To authenticate by adding your personal access token to your *~/.npmrc* file, edit the *~/.npmrc* file for your project to include the following line, replacing *TOKEN* with your personal access token.  Create a new *~/.npmrc* file if one doesn't exist.

{% if currentVersion != "free-pro-team@latest" %}
If your instance has subdomain isolation enabled:
{% endif %}

```shell
//npm.pkg.github.com/:_authToken=<em>TOKEN</em>
```

{% if currentVersion != "free-pro-team@latest" %}
If your instance has subdomain isolation disabled:

```shell
$ npm login --registry=https://npm.pkg.github.com
> Username: <em>USERNAME</em>
> Password: <em>TOKEN</em>
> Email: <em>PUBLIC-EMAIL-ADDRESS</em>
```
{% endif %}

To authenticate by logging in to npm, use the `npm login` command, replacing *USERNAME* with your {{ site.data.variables.product.prodname_dotcom }} username, *TOKEN* with your personal access token, and *PUBLIC-EMAIL-ADDRESS* with your email address.

{% if currentVersion != "free-pro-team@latest" %}
If your instance has subdomain isolation enabled:
{% endif %}

```shell
"repository" : {
    "type" : "git",
    "url": "ssh://git@github.com/<em>OWNER</em>/<em>REPOSITORY</em>.git",
    "directory": "packages/name"
  },
```

{% if currentVersion != "free-pro-team@latest" %}
If your instance has subdomain isolation disabled:

```shell
registry=https://npm.pkg.github.com/<em>OWNER</em>
@<em>OWNER</em>:registry=https://npm.pkg.github.com
@<em>OWNER</em>:registry=https://npm.pkg.github.com
```
{% endif %}

#### #### Authenticating with the `GITHUB_TOKEN`

{{ site.data.reusables.package_registry.package-registry-with-github-tokens }}

### Ein Paket veröffentlichen

By default, {{ site.data.variables.product.prodname_registry }} publishes a package in the {{ site.data.variables.product.prodname_dotcom }} repository you specify in the name field of the *package.json* file. Ein Paket namens `@my-org/test` würde beispielsweise im Repository `my-org/test` auf {{ site.data.variables.product.prodname_dotcom }} veröffentlicht. You can add a summary for the package listing page by including a *README.md* file in your package directory. For more information, see "[Working with package.json](https://docs.npmjs.com/getting-started/using-a-package.json)" and "[How to create Node.js Modules](https://docs.npmjs.com/getting-started/creating-node-modules)" in the npm documentation.

You can publish multiple packages to the same {{ site.data.variables.product.prodname_dotcom }} repository by including a `URL` field in the *package.json* file. For more information, see "[Publishing multiple packages to the same repository](#publishing-multiple-packages-to-the-same-repository)."

Die Scope-Zuordnung für Ihr Projekt können Sie entweder über die lokale *.npmrc*-Datei im Projekt oder die Option `publishConfig` in der Datei *package.json* festlegen. {{ site.data.variables.product.prodname_registry }} only supports scoped npm packages. Pakete mit Scopes weisen Namen im Format `@owner/name` auf. Pakete mit Scopes beginnen immer mit dem Symbol `@`. Eventuell müssen Sie den Namen in der Datei *package.json* aktualisieren, um den Namen mit Scope zu verwenden. Beispiel: `"name": "@codertocat/hello-world-npm"`.

{{ site.data.reusables.package_registry.viewing-packages }}

#### Publishing a package using a local *.npmrc* file

You can use an *.npmrc* file to configure the scope mapping for your project. In the *.npmrc* file, use the {{ site.data.variables.product.prodname_registry }} URL and account owner so {{ site.data.variables.product.prodname_registry }} knows where to route package requests. Using an *.npmrc* file prevents other developers from accidentally publishing the package to npmjs.org instead of {{ site.data.variables.product.prodname_registry }}. {{ site.data.reusables.package_registry.lowercase-name-field }}

{{ site.data.reusables.package_registry.authenticate-step}}
{{ site.data.reusables.package_registry.create-npmrc-owner-step}}
{{ site.data.reusables.package_registry.add-npmrc-to-repo-step}}
4. Überprüfen Sie den Namen Ihres Pakets in der Datei *package.json* Ihres Projekts. Das Feld `name` (Name) muss den Scope und den Namen des Pakets enthalten. Wenn z. B. Ihr Paket „test“ heißt und Sie es in der {{ site.data.variables.product.prodname_dotcom }}-Organisation „My-org“ veröffentlichen möchten, muss das Feld `name` (Name) in der Datei *package.json* `@my-org/test` enthalten.
{{ site.data.reusables.package_registry.verify_repository_field }}
{{ site.data.reusables.package_registry.publish_package }}

#### Publishing a package using `publishConfig` in the *package.json* file

You can use `publishConfig` element in the *package.json* file to specify the registry where you want the package published. Weitere Informationen finden Sie unter „[publishConfig](https://docs.npmjs.com/files/package.json#publishconfig)“ in der NPM-Dokumentation.

1. Bearbeiten Sie die Datei *package.json* für Ihr Paket, und fügen Sie den Eintrag `publishConfig` hinzu.
  {% if currentVersion != "free-pro-team@latest" %}
  If your instance has subdomain isolation enabled:
  {% endif %}
  ```shell
    "publishConfig": {
      "registry":"https://npm.pkg.github.com/"
    },
  ```
  {% if currentVersion != "free-pro-team@latest" %}
  If your instance has subdomain isolation disabled:
   ```shell
    "publishConfig": {
      "registry":"https://<em>HOSTNAME</em>/_registry/npm/"
    },
  ```
  {% endif %}
{{ site.data.reusables.package_registry.verify_repository_field }}
{{ site.data.reusables.package_registry.publish_package }}

### Publishing multiple packages to the same repository

To publish multiple packages to the same repository, you can include the URL of the {{ site.data.variables.product.prodname_dotcom }} repository in the `repository` field of the *package.json* file for each package.

To ensure the repository's URL is correct, replace REPOSITORY with the name of the repository containing the package you want to publish, and OWNER with the name of the user or organization account on {{ site.data.variables.product.prodname_dotcom }} that owns the repository.

{{ site.data.variables.product.prodname_registry }} will match the repository based on the URL, instead of based on the package name. If you store the *package.json* file outside the root directory of your repository, you can use the `directory` field to specify the location where {{ site.data.variables.product.prodname_registry }} can find the *package.json* files.

```shell
"repository" : {
    "type" : "git",
    "url": "ssh://git@{% if currentVersion == "free-pro-team@latest" %}github.com{% else %}<em>HOSTNAME</em>{% endif %}/<em>OWNER</em>/<em>REPOSITORY</em>.git",
    "directory": "packages/name"
  },
```

### Ein Paket installieren

You can install packages from {{ site.data.variables.product.prodname_registry }} by adding the packages as dependencies in the *package.json* file for your project. Weitere Informationen zum Verwenden einer *package.json*-Datei in Ihrem Projekt finden Sie unter „[Mit package.json arbeiten](https://docs.npmjs.com/getting-started/using-a-package.json)“ in der npm-Dokumentation.

By default, you can add packages from one organization. For more information, see [Installing packages from other organizations](#installing-packages-from-other-organizations)

You also need to add the *.npmrc* file to your project so all requests to install packages will go through {{ site.data.variables.product.prodname_registry }}. When you route all package requests through {{ site.data.variables.product.prodname_registry }}, you can use both scoped and unscoped packages from *npmjs.com*. Weitere Informationen finden Sie unter „[npm-scope](https://docs.npmjs.com/misc/scope) in der npm-Dokumentation.

{{ site.data.reusables.package_registry.authenticate-step}}
{{ site.data.reusables.package_registry.create-npmrc-owner-step}}
{{ site.data.reusables.package_registry.add-npmrc-to-repo-step}}
4. Configure *package.json* in your project to use the package you are installing. To add your package dependencies to the *package.json* file for {{ site.data.variables.product.prodname_registry }}, specify the full-scoped package name, such as `@my-org/server`. For packages from *npmjs.com*, specify the full name, such as `@babel/core` or `@lodash`. For example, this following *package.json* uses the `@octo-org/octo-app` package as a dependency.

  ```
  {
    "name": "@my-org/server",
    "version": "1.0.0",
    "description": "Server app that uses the @octo-org/octo-app package",
    "main": "index.js",
    "author": "",
    "license": "MIT",
    "dependencies": {
      "@octo-org/octo-app": "1.0.0"
    }
  }
  ```
5. Installieren Sie das Paket.

  ```shell
  $ npm install
  ```

#### Pakete von anderen Organisationen installieren

Standardmäßig können Sie nur {{ site.data.variables.product.prodname_registry }}-Pakete von einer Organisation verwenden. Standardmäßig können Sie nur {{ site.data.variables.product.prodname_registry }}-Pakete von einer Organisation verwenden. {{ site.data.reusables.package_registry.lowercase-name-field }}

{% if currentVersion != "free-pro-team@latest" %}
If your instance has subdomain isolation enabled:
{% endif %}

```shell
registry=https://{% if currentVersion == "free-pro-team@latest" %}npm.pkg.github.com{% else %}npm.<em>HOSTNAME</em>/{% endif %}<em>OWNER</em>
@<em>OWNER</em>:registry={% if currentVersion == "free-pro-team@latest" %}npm.pkg.github.com{% else %}npm.<em>HOSTNAME</em>/{% endif %}
@<em>OWNER</em>:registry={% if currentVersion == "free-pro-team@latest" %}npm.pkg.github.com{% else %}npm.<em>HOSTNAME</em>/{% endif %}
```

{% if currentVersion != "free-pro-team@latest" %}
If your instance has subdomain isolation disabled:

```shell
registry=https://<em>HOSTNAME</em>/_registry/npm/<em>OWNER</em>
@<em>OWNER</em>:registry=<em>HOSTNAME</em>/_registry/npm/
@<em>OWNER</em>:registry=<em>HOSTNAME</em>/_registry/npm/
```
{% endif %}


### Weiterführende Informationen

- "[Deleting a package](/packages/publishing-and-managing-packages/deleting-a-package/)"