---
title: 系统签名app运行webview闪退问题
date: 2021-09-08 10:40:14
tags:
  - Android
categories: 技术
thumbnail: https://cdn.jsdelivr.net/gh/LiuIos/picBed/20210908104159.png
toc: false
---



> 开发定制Android系统应用时，系统签名后app闪退
>
> ````java.lang.UnsupportedOperationException: For security reasons, WebView is not allowed in privileged processes````

```java
public static void hookWebView() {
        int sdkInt = Build.VERSION.SDK_INT;
        try {
            Class<?> factoryClass = Class.forName("android.webkit.WebViewFactory");
            Field field = factoryClass.getDeclaredField("sProviderInstance");
            field.setAccessible(true);
            Object sProviderInstance = field.get(null);
            if (sProviderInstance != null) {

                Log.d("hookWebView==","sProviderInstance isn't null");
                return;
            }
            Method getProviderClassMethod;
            if (sdkInt > 22) { // above 22
                getProviderClassMethod = factoryClass.getDeclaredMethod("getProviderClass");
            } else if (sdkInt == 22) { // method name is a little different
                getProviderClassMethod = factoryClass.getDeclaredMethod("getFactoryClass");
            } else { // no security check below 22

                Log.d("hookWebView==","Don't need to Hook WebView");
                return;
            }
            getProviderClassMethod.setAccessible(true);
            Class<?> providerClass = (Class<?>) getProviderClassMethod.invoke(factoryClass);
            Class<?> delegateClass = Class.forName("android.webkit.WebViewDelegate");
            Constructor<?> providerConstructor = providerClass.getConstructor(delegateClass);
            if (providerConstructor != null) {
                providerConstructor.setAccessible(true);
                Constructor<?> declaredConstructor = delegateClass.getDeclaredConstructor();
                declaredConstructor.setAccessible(true);
                sProviderInstance = providerConstructor.newInstance(declaredConstructor.newInstance());

                Log.d("hookWebView==","sProviderInstance:{}");
                field.set("sProviderInstance", sProviderInstance);
            }

            Log.d("hookWebView==","Hook done!");
        } catch (Throwable e) {

            Log.d("hookWebView==",e.toString());
        }
    }
```

