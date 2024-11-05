<div align="center">

[![logo](logo-b.png)](https://github.com/ipitio/backage)

# [backage](https://github.com/ipitio/backage)

**It's all part and parcel**

---

[![build](https://github.com/ipitio/backage/actions/workflows/publish.yml/badge.svg)](https://github.com/ipitio/backage/pkgs/container/backage) [![packages](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fgithub.com%2Fipitio%2Fbackage%2Fraw%2Findex%2F.json&query=%24.packages&logo=github&logoColor=959da5&label=packages&labelColor=333a41&color=2ebc4f)](https://github.com/ipitio/backage/tree/index) [![date](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fgithub.com%2Fipitio%2Fbackage%2Fraw%2Findex%2F.json&query=%24.date&logo=github&logoColor=959da5&label=refreshed&labelColor=333a41&color=2ebc4f)](https://github.com/ipitio/backage/releases/latest)

</div>

The GitHub Packages API doesn't expose much of the publicly-available metadata that other registries provide. This completely automated closed-loop system [is the solution](https://github.com/badges/shields/issues/5594#issuecomment-2157626147). **Just star this project to have GitHub supplement its API with an additional endpoint for your public packages!**

A service ran by GitHub will add them to its circular priority queue within the next few hours and update the [dataset](https://github.com/ipitio/backage/releases/latest). If you'd then like the service to forget and ignore some or all of your packages, add `owner[/repo[/package]]` to `optout.txt` [here](https://github.com/ipitio/backage/edit/master/optout.txt) and make a pull request.

To add any other users or organizations not yet [in the index](https://github.com/ipitio/backage/tree/index), add the case-sensitive name of each one on a new line in `owners.txt` on your own fork [here](https://github.com/ipitio/backage/edit/master/owners.txt) and make a pull request. Please add just the name(s) -- ids, repos, and packages will be found automatically!

<div align="center">

</div>

## Metadata Endpoint

```prolog
https://ipitio.github.io/backage/OWNER/REPO/PACKAGE.FORMAT
```

Use something like [shields.io/json](https://shields.io/badges/dynamic-json-badge) or [shields.io/xml](https://shields.io/badges/dynamic-xml-badge) with this endpoint to access the latest data and make badges like the ones above. Replace `OWNER/REPO/PACKAGE.FORMAT` with their respective values.

> [!NOTE]
> The format can be either `json` or `xml`. You'll need the XML endpoint to evaluate expressions, like filters, with Shields -- see [this issue](https://github.com/ipitio/backage/issues/23).

> [!TIP]
> Use the proxy to convert external JSON to XML! This doesn't currently work with Shields, though.

You'll find these properties for the package and its versions:

<details>

<summary>Package</summary>

|       Property        |     Type     | Description                                         |
| :-------------------: | :----------: | --------------------------------------------------- |
|      `owner_id`       |    number    | The ID of the owner                                 |
|     `owner_type`      |    string    | The type of owner (e.g. `users`)                    |
|    `package_type`     |    string    | The type of package (e.g. `container`)              |
|        `owner`        |    string    | The owner of the package                            |
|        `repo`         |    string    | The repository of the package                       |
|       `package`       |    string    | The package name                                    |
|        `date`         |    string    | The most recent date the package was refreshed      |
|        `size`         |    string    | Formatted size of the latest version                |
|      `versions`       |    string    | Formatted count of all versions ever tracked        |
|       `tagged`        |    string    | Formatted count of all tagged versions ever tracked |
|      `downloads`      |    string    | Formatted count of all downloads                    |
|   `downloads_month`   |    string    | Formatted count of all downloads in the last month  |
|   `downloads_week`    |    string    | Formatted count of all downloads in the last week   |
|    `downloads_day`    |    string    | Formatted count of all downloads in the last day    |
|      `raw_size`       |    number    | Size of the latest version, in bytes                |
|    `raw_versions`     |    number    | Count of versions tracked                           |
|     `raw_tagged`      |    number    | Count of tagged versions tracked                    |
|    `raw_downloads`    |    number    | Count of all downloads                              |
| `raw_downloads_month` |    number    | Count of all downloads in the last month            |
| `raw_downloads_week`  |    number    | Count of all downloads in the last week             |
|  `raw_downloads_day`  |    number    | Count of all downloads in the last day              |
|       `version`       | object array | The versions of the package (see below)             |

</details>

<details>

<summary>Version</summary>

|       Property        |     Type     | Description                                    |
| :-------------------: | :----------: | ---------------------------------------------- |
|         `id`          |    number    | The ID of the version                          |
|        `name`         |    string    | The version name                               |
|        `date`         |    string    | The most recent date the version was refreshed |
|       `newest`        |   boolean    | Whether the version is the newest              |
|       `latest`        |   boolean    | Whether the version is the newest tagged       |
|        `size`         |    string    | Formatted size of the version                  |
|      `downloads`      |    string    | Formatted count of downloads                   |
|   `downloads_month`   |    string    | Formatted count of downloads in the last month |
|   `downloads_week`    |    string    | Formatted count of downloads in the last week  |
|    `downloads_day`    |    string    | Formatted number of downloads in the last day  |
|      `raw_size`       |    number    | Size of the version, in bytes                  |
|    `raw_downloads`    |    number    | Count of downloads                             |
| `raw_downloads_month` |    number    | Count of downloads in the last month           |
| `raw_downloads_week`  |    number    | Count of downloads in the last week            |
|  `raw_downloads_day`  |    number    | Count of downloads in the last day             |
|        `tags`         | string array | The tags of the version                        |

</details>

They can be queried with the following paths:

<details>

<summary>JSON</summary>

You can query a package for its properties, like size or version:

```jboss-cli
$.PROPERTY
```

```jboss-cli
$.size
```

Versions may be filtered in and tags out:

```jboss-cli
$.version[FILTER].PROPERTY
```

```jboss-cli
$.version[?(@.latest)].tags[?(@!="latest")]
```

</details>

<details>

<summary>XML</summary>

You can query a package for its properties, like size or version:

```prolog
/bkg/PROPERTY
```

```prolog
/bkg/size
```

Versions can be filtered in and tags out:

```prolog
/bkg/version[FILTER]/PROPERTY
```

```prolog
/bkg/version[./latest[.="true"]]/tags[.!="latest"]
```

</details>

## JSON2XML Proxy

```prolog
https://ipitio.github.io/backage?json=https://URLENCODED/JSON/PATH
```

Use your own JSON endpoint with this proxy to convert it into XML. Try it out in your browser:

##### [https://ipitio.github.io/backage?json=https://ipitio.github.io/backage/ipitio/backage/backage.json](https://ipitio.github.io/backage?json=https://ipitio.github.io/backage/ipitio/backage/backage.json)
