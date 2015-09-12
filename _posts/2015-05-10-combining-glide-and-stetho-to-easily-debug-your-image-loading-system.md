---
layout: post
title: Combining Glide and Stetho to easily debug your image loading system
categories: [Android]
tags: [androiddev, stetho, glide, debugging]
fullview: false
comments: true
---

Downloading, managing and displaying efficiently images on Android can be a difficult task. Luckily, libraries like [Glide](https://github.com/bumptech/glide)  or [Picasso](http://square.github.io/picasso/) made it really handy for any developer. While they both are really easy to implement and integrate perfectly within the `Activity` and `Fragment` lifecyle; Glide proposes also a killing feature: letting the developer choose his network stack.

On its side, [Stetho](http://facebook.github.io/stetho/) is an Android debug bridge tool which bring View hierarchy, Network & DB inspection and more into your Chrome Developer Tools. It is really handy and comes almost for free in terms of implementation. ([Here's a good tutorial](http://littlerobots.nl/blog/stetho-for-android-debug-builds-only/) to only use it with your `Debug` builds)

So we have 2 good libraries, and both of them support [OkHttp](http://square.github.io/okhttp/) as a networking client.

Now let's see how can we combine them to see every HTTP requests from Glide into your Developer Tools.

### 1. Setting up the Network inspection via Stetho

First, in your gradle file you should have:

    debugCompile 'com.facebook.stetho:stetho:1.1.0'
    debugCompile 'com.facebook.stetho:stetho-okhttp:1.1.0'

Then in your Application class you enable the webkit inspector:

Stetho.initialize(Stetho.newInitializerBuilder(context)
                        .enableWebKitInspector(Stetho.defaultInspectorModulesProvider(context))
                        .build());
You finally just need to add a `StethoInterceptor` to your `OkHttpClient` and ta-daaa!

OkHttpClient okClient = new OkHttpClient();
            okClient.networkInterceptors().add(new StethoInterceptor());


### 2. Configuring Glide to use OkHttp and Stetho

Glide provides also a basic `GlideModule` to directly use OkHttp. I recommend you to read how to integrate it directly [in their page](https://github.com/bumptech/glide/wiki/Integration-Libraries#okhttp).

But this is not enough to make it work. The `OkHttpGlideModule` does not allow you to configure the `OkHttpClient`. You do need to compile the `okhttp-integration` library but you will have to create your own `GlideModule`.

    public class StethoOkHttpGlideModule  implements GlideModule {
        @Override
        public void applyOptions(Context context, GlideBuilder builder) { }

        @Override
        public void registerComponents(Context context, Glide glide) {
            OkHttpClient client = new OkHttpClient();
            client.networkInterceptors().add(new StethoInterceptor());
            glide.register(GlideUrl.class, InputStream.class, new OkHttpUrlLoader.Factory(client));
        }
    }


And do not forget to reference you module in your `AndroidManifest.xml` file:

     <meta-data android:name="utils.glide.StethoOkHttpGlideModule"
                android:value="GlideModule" />


### 3. Try it!

 ![And enjoy!]({{ site.url }}/assets/media/glide-stetho.png)
