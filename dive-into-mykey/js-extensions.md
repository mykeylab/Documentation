# JS Extensions

JSBridge is injected javascript code in MYKEY dapp browser enviroment by default, it support Scatter protocol, developers can develop H5 dapp follow [Scatter Document](https://get-scatter.com/docs/api-reference), it will also support web3js protocol when MYKEY support ETH.

Special Notice: [MYKEY account structure](https://github.com/mykeylab/Documentation/blob/master/English/MYKEY%20on%20EOSIO.md#mykey-account-structure) is different with other EOS account, if dapp verify signature in their server side, should use the public key of Reserved, more details see this [Document](mykey-on-eos.md#2-for-dapps-compatible-with-scatter)

For ease of use, it provided a few extra methods, e.g. fullscreen, rotation of the sign box, close window and so on

## Enter fullscreen mode

Enter fullscreen mode, dapp can adjust the rotation of the sign box according the value of isLandscape

```javascript
// isLandscape: Rotation of the sign box , ture as horizontal，false as vertical.
window.MyKey.Browser.openFullScreen(isLandscape)
```

## Exit fullscreen mode

In non-fullscreen mode, the rotation of the sign box always in vertical screen mode.

```javascript
window.MyKey.Browser.closeFullScreen()
```

## Quit

Quit dapp, destory the current window

```javascript
window.MyKey.Browser.closeWindow()
```

## Forbid Physical Back \(Only in Android\)

Fobid native physical back button in Android

```javascript
window.MyKey.Browser.forbidPhysicalBack()
```

## Allow Physical Back \(Only in Android\)

Allow native physical back button in Android

```javascript
window.MyKey.Browser.allowPhysicalBack()
```

