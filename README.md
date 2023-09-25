# HybridMediaPlayer
Android music player from URL. Uses ExoPlayer and MediaPlayer for lower APIs.

## Installation

To use the library, first include it your project using Gradle.

Add JitPack in your root build.gradle at the end of repositories:
```
    allprojects {
        repositories {
            jcenter()
            maven { url "https://jitpack.io" }
        }
    }
```

If not enabled already, you also need to turn on Java 8 support in all `build.gradle` files depending on ExoPlayer, by adding the following to the `android` section:

```gradle
compileOptions {
    targetCompatibility JavaVersion.VERSION_1_8
}
```

Add it as a dependency in your app's build.gradle file:
```
    dependencies {
        compile 'com.github.mkaflowski:HybridMediaPlayer:${version}' //CHANGE IT TO CURRENT VERSION
    }
```

## How to use

```java
        HybridMediaPlayer mediaPlayer = HybridMediaPlayer.getInstance(context);
        mediaPlayer.setDataSource(url);
	mediaPlayer.prepare();
	
	mediaPlayer.setPlayerView(this, playerView);
	
	//it goes to STATE_IDLE on the end of sources in Chromecast 
        mediaPlayer.setOnCompletionListener(this);
        mediaPlayer.setOnErrorListener(this);
        mediaPlayer.setOnPreparedListener(this);
        
        mediaPlayer.play();
        mediaPlayer.seekTo(1500);
        mediaPlayer.pause();
	
	mediaPlayer.setVolume(0.5f);
        
        mediaPlayer.release();
```

### Methods for ExoPlayer only

```java
        ExoMediaPlayer mediaPlayer = new ExoMediaPlayer(this)
        mediaPlayer.setDataSource(url1, url2, url3, ...);
	mediaPlayer.prepare();
	
	//for proper Chromecast handling
	mediaPlayer.setInitialWindowNum(1); // start track number
	mediaPlayer.setDataSource(mediaSourceInfoList);
	mediaPlayer.setCastPlayer(castContext);
	
	mediaPlayer.isCasting()
	
	//setting video view:
	mediaPlayer.setPlayerView(this, playerView);
	
        mediaPlayer.setOnPositionDiscontinuityListener(this);
	mediaPlayer.setOnTracksChangedListener(this);
        
	mediaPlayer.hasVideo();
        mediaPlayer.seekTo(trackNum,time);
	
	mediaPlayer.setSupportingSystemEqualizer(true);
	//FOR EDITING PLAYLIST
	mediaPlayer.getMediaSource();
	
        mediaPlayer.release();
```
### Creating MediaSourceInfo for proper Chromecast handling example

```java
        MediaSourceInfo source1 = new MediaSourceInfo.Builder().setUrl(url)
                .setTitle("Podcast Stream")
                .setImageUrl("https://cdn.dribbble.com/users/20781/screenshots/573506/podcast_logo.jpg")
                .build();
        MediaSourceInfo source2 = new MediaSourceInfo.Builder().setUrl(url3)
                .setTitle("Movie")
                .setImageUrl("http://www.pvhc.net/img29/amkulkkbogfvmihgspru.png")
                .isVideo(true)
                .build();

        List<MediaSourceInfo> sources = new ArrayList<>();
        sources.add(source1);
        sources.add(source2);

        mediaPlayer.setDataSource(sources);
	mediaPlayer.setCastPlayer(castContext);
```

Register your CastOptionProvider (or default) in manifest file:

```xml
   <meta-data
    android:name="com.google.android.gms.cast.framework.OPTIONS_PROVIDER_CLASS_NAME"
    android:value="com.google.android.exoplayer2.ext.cast.DefaultCastOptionsProvider" />
```
