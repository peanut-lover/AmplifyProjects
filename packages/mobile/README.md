# Sample React Native Project

# ๐ง Caution

Mono Repo์ ๋ฆฌ์กํธ ๋ค์ดํฐ๋ธ๊ฐ `no-hoist` ์ต์์์ด ๊ณต์ ๋๋๋ก ํ๋ ์ ๋ต์ ์ ํํ  ๊ฒฝ์ฐ,

React Native ์ค์ ์์ package ๊ฒฝ๋ก๋ ๋ฃจํธ ๊ฒฝ๋ก์ `node_modules` ๋ก ์ง์ ํด์ค์ผํ๋ค.

์ค์  ๋น์์ ์ค์ ํ ๊ฐ๋ค์ ์๋์ ๊ฐ์ต๋๋ค. React Native ๋ฒ์ ผ๋ง๋ค ๋ฌ๋ผ์ง ๊ฐ๋ฅ์ฑ์ด ๋ํํ๋ ํญ์ ์ฃผ์๊น๊ฒ ๊ณ ๋ คํด์ผํ  ๋ถ๋ถ์๋๋ค.

> https://www.callstack.com/blog/setting-up-react-native-monorepo-with-yarn-workspaces

# Update iOS Setting

## Update /ios/Podfile

```ruby
# ๊ฒฝ๋ก๋ฅผ require_relative '../node_modules/react-native/scripts/react_native_pods'
require_relative '../../../node_modules/react-native/scripts/react_native_pods'

# TODO: require_relative '../node_modules/@react-native-community/cli-platform-ios/native_modules'
require_relative '../../../node_modules/@react-native-community/cli-platform-ios/native_modules'

# ...

target 'SampleApp' do
  config = use_native_modules!
    # ...
    post_install do |installer|
        react_native_post_install(
        installer,

        # Update: Add this argument, Default is "../node_modules/react-native"
        "../../../node_modules/react-native",

        # Set `mac_catalyst_enabled` to `true` in order to apply patches
        # necessary for Mac Catalyst builds
        :mac_catalyst_enabled => false
        )
        __apply_Xcode_12_5_M1_post_install_workaround(installer)
    end
    # ...
end
```

## Next open Xcode and inside Project settings > Build Phases open โBundle React Native code and imagesโ

```
- WITH_ENVIRONMENT="../../node_modules/react-native/scripts/xcode/with-environment.sh"
- REACT_NATIVE_XCODE="../../node_modules/react-native/scripts/react-native-xcode.sh"
+ WITH_ENVIRONMENT="../../../node_modules/react-native/scripts/xcode/with-environment.sh"
+ REACT_NATIVE_XCODE="../../../node_modules/react-native/scripts/react-native-xcode.sh"
```

# Update Android Setting

## `android/settings.gradle`

```
rootProject.name = 'SampleApp'
apply from: file("../../../node_modules/@react-native-community/cli-platform-android/native_modules.gradle"); applyNativeModulesSettingsGradle(settings)
include ':app'
includeBuild('../../../node_modules/react-native-gradle-plugin')
```

## `android/app/build.gradle`

```
//   The folder where the react-native NPM package is. Default is ../node_modules/react-native
reactNativeDir = file("../../../../node_modules/react-native")
//   The folder where the react-native Codegen package is. Default is ../node_modules/react-native-codegen
codegenDir = file("../../../../node_modules/react-native-codegen")
//   The cli.js file which is the React Native CLI entrypoint. Default is ../node_modules/react-native/cli.js
cliFile = file("../../../../node_modules/react-native/cli.js")

...

apply from: file("../../../../node_modules/@react-native-community/cli-platform-android/native_modules.gradle"); applyNativeModulesAppBuildGradle(project)
```

## Update `metro.config.js`

```javascript
const path = require('path');

module.exports = {
  watchFolders: [path.resolve(__dirname, '../../node_modules')],
  transformer: {
    getTransformOptions: async () => ({
      transform: {
        experimentalImportSupport: false,
        inlineRequires: true,
      },
    }),
  },
};
```
