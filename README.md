# Cordova crypto file plugin
HTML source file is encrypted at build, and decrypted at run.  
https://www.npmjs.com/package/cordova-plugin-crypto-file

## Add Plugin
`cordova plugin add cordova-plugin-crypto-file`

## Purpose

This plugin was created to solve the issue of using cordova-plugin-ionic-webview with cordova-crypt-file, refer to [#75](https://github.com/tkyaji/cordova-plugin-crypt-file/issues/75) 

## Important

The file `IonicWebViewEngine.java` need to be modified for this plugin to work and for the source code to be encrypted.
### Steps:
1. After adding the cordova-plugin-ionic-webview, navigate to the following location:

**platforms/android/src/com/ionicframework/cordova/webview/IonicWebViewEngine.java**

2. Remove the following code:

```
    @RequiresApi(Build.VERSION_CODES.LOLLIPOP)
    @Override
    public WebResourceResponse shouldInterceptRequest(WebView view, WebResourceRequest request) {
      super.shouldInterceptRequest(view,request);
      return localServer.shouldInterceptRequest(request.getUrl());
    }

    @TargetApi(Build.VERSION_CODES.KITKAT)
    @Override
    public WebResourceResponse shouldInterceptRequest(WebView view, String url) {
      return localServer.shouldInterceptRequest(Uri.parse(url));
    }
```
3. Add this instead:

```
    @Override
    public WebResourceResponse shouldInterceptRequest(WebView view, String url) {
      return super.shouldInterceptRequest(view, url);
    }
 ```
 
 Then the plugin will work.

## Encrypt
`cordova build [ios / android]`

## Decrypt
`cordova emulate [ios / android]`  
or  
`cordova run [ios / android]`  

## Encryption subjects.

### Default

* .html
* .htm
* .js
* .css

### Edit subjects

You can specify the encryption subjects by editing `plugin.xml`.

**plugins/cordova-plugin-crypt-file/plugin.xml**

```
<cryptfiles>
    <include>
        <file regex="\.(htm|html|js|css)$" />
    </include>
    <exclude>
        <file regex="exclude_file\.js$" />
    </exclude>
</cryptfiles>
```

Specify the target file as a regular expression.


## Supported platforms
* iOS
* Android
* CrossWalk


## Based on the original cordova-crypt-file created by tkyaji

https://github.com/tkyaji/cordova-plugin-crypt-file

## License
Apache version 2.0
