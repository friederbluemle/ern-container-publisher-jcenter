# Electrode Native JCenter Container Publisher

[![ci][1]][2]

This publisher can be used to publish Android Electrode Native Containers to a remote [JCenter](https://bintray.com/bintray/jcenter) repository.

You must have a [Bintray](https://bintray.com) account in order to use this publisher.

## Usage

Independent of the method used, the following parameters will be retrieved from your global `~/.gradle/gradle.properties` file. You must have matching variable names defined in this file. They are all required.

- `bintrayUser` : The Bintray account username [*]
- `bintrayApiKey`: The Bintray account API key [*]
- `bintrayRepo` : The Bintray repository [*]
- `bintrayVcsUrl` : The Bintray VCS url [*]

### With `ern publish-container` CLI command

#### Required

- `--containerVersion/-v` : Version of the Container to publish
- `--publisher/-p` : `jcenter`
- `--platform` : `android`
- `--config/-c` : A json string (or path to a json file) containing the following required properties:
  - `artifactId` : The artifact id to be used for the Container
  - `groupId` : The group id to be used for the Container

#### Optional

- `--containerPath` : Path to the Container to publish.\
Defaults to the Electrode Native default Container Generation path (`~/.ern/containergen/out/[platform]` if not changed through config)

- `--containerVersion/-v` : Version of the Container to publish.\
Default to `1.0.0`

The `ern publish-container` CLI command can be used as follow to manually publish a Container using the jcenter publisher :

```sh
ern publish-container --containerPath [pathToContainer] -p jcenter -v [containerVersion] --platform android -e '{"artifactId":"[artifactId]", "groupId":"[groupId]"}'
```

Instead of passing the whole configuration on the command line for `--extra/-e`, it is also possible to use a file path to a json file holding the configuration, or a path to a file stored in the Cauldron. Check out the [ern publish-container](https://native.electrode.io/cli-commands/publish-container) command documentation for more info.

### With Cauldron

#### Required

- `--publisher/-p` : `jcenter`

#### Optional

- `--config/-c` : A json string (or path to a json file) containing the following required properties:
  - `artifactId` : The artifact id to be used for the Container\
Defaults to `[nativeAppName]-ern-container`
  - `groupId` : The group id to be used for the Container\
Defaults to `com.walmartlabs.ern`

To automatically publish Cauldron generated Containers of a target native application and platform, the `ern cauldron add publisher` command can be used as follow :

```sh
ern cauldron add publisher -p jcenter -e '{"artifactId":"[artifactId]", "groupId":"[groupId]"}'
```

Instead of passing the whole configuration on the command line for `--extra/-e`, it is also possible to use a file path to a json file holding the configuration, or a path to a file stored in the Cauldron. Check out the [ern cauldron add-publisher](https://native.electrode.io/cli-commands/cauldron/add-publisher) command documentation for more info.

This will result in the following publisher entry in Cauldron :

```json
{
  "name": "jcenter",
  "extra": {
    "artifactId": "[artifactId]",
    "groupId" : "[groupId]"
  }
}
```

This is only needed once. Once the configuration for the publisher is stored in Cauldron, any new Cauldron generated Container will be publihsed to jcenter.

### Programmatically

```js
import JCenterPublisher from 'ern-container-publisher-jcenter'
const publisher = new JCenterPublisher()
publisher.publish({
    /* Local file system path to the Container */
    containerPath,
    /* Version of the Container. Jenter artifact version */
    containerVersion,
    /* Extra config specific to this publisher */
    extra: {
      /* Artifact id to use for publication */
      artifactId: string
      /* Group id to use for publication */
      groupId: string
    }
  }
})
```

[1]: https://github.com/electrode-io/ern-container-publisher-jcenter/workflows/ci/badge.svg
[2]: https://github.com/electrode-io/ern-container-publisher-jcenter/actions
