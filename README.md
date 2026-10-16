# Uniwind Pro 1.8.1: Android startup crash on React Native 0.88

Minimal Expo SDK 58 app (expo 58.0.12, react-native 0.88.0) with `uniwind-pro` 1.8.1. It needs a Uniwind Pro licence to install.

## Steps

1. `yarn install` (with your Uniwind Pro auth set up)
2. `npx expo prebuild --platform android`
3. `npx expo run:android` (debug build, New Architecture)

## Result

The app shows "There was a problem loading the project" at launch:

```
java.lang.NullPointerException: null cannot be cast to non-null type com.facebook.react.fabric.FabricUIManager
  at com.uniwind.UniwindModule.<init>(UniwindModule.kt)
```

`UniwindModule` casts `UIManagerHelper.getUIManager(reactContext, UIManagerType.FABRIC)` to a non-null `FabricUIManager` in a property initializer, but on RN 0.88 the TurboModule is constructed before the Fabric UIManager exists, so the value is `null`. iOS is not affected.

Details and a working patch (resolve the UIManager lazily): https://github.com/uni-stack/uniwind/issues/735
