# Admob
Set up Admob ad on your android app

[![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=anuraghazra)](https://github.com/anuraghazra/github-readme-stats)


>Step 1. Configure your app
```
buildscript {
    repositories {
        google()
        mavenCentral()
    }
}

allprojects {
    repositories {
        google()
        mavenCentral()
    }
}
```
>Step 2.Add the dependencies for the Google Mobile Ads SDK to your module's app-level Gradle file, normally app/build.gradle:
```
dependencies {
  implementation 'com.google.android.gms:play-services-ads:21.3.0'
}
```

>Step 3.Updating Your AndroidManifest.xml File
```
<uses-permission android:name="android.permission.INTERNET" />  
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />  
```

>Step 4.Add your AdMob app ID (identified in the AdMob UI) to your app's AndroidManifest.xml file.
```
<manifest>
    <application>
        <!-- Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713 -->
        <meta-data
            android:name="com.google.android.gms.ads.APPLICATION_ID"
            android:value="ca-app-pub-xxxxxxxxxxxxxxxx~yyyyyyyyyy"/>
    </application>
</manifest>
```
  
  >Step 5.Initialize the Google Mobile Ads SDK:
```
public class MainActivity extends AppCompatActivity {
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        MobileAds.initialize(this, new OnInitializationCompleteListener() {
            @Override
            public void onInitializationComplete(InitializationStatus initializationStatus) {
            }
        });
    }
}
```
  
  >Step 6.Add AdView to the layout xml file:
```
 <com.google.android.gms.ads.AdView
      xmlns:ads="http://schemas.android.com/apk/res-auto"
      android:id="@+id/adView"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:layout_centerHorizontal="true"
      android:layout_alignParentBottom="true"
      ads:adSize="BANNER"
      ads:adUnitId="ca-app-pub-3940256099942544/6300978111">
  </com.google.android.gms.ads.AdView>
```
  >Step 7.You can alternatively create AdView programmatically:
```
AdView adView = new AdView(this);
adView.setAdSize(AdSize.BANNER);
adView.setAdUnitId("ca-app-pub-3940256099942544/6300978111");
```
  
  >Step 8.Load an Banner Ad:
```
public class MainActivity extends AppCompatActivity {
    private AdView mAdView;

    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        MobileAds.initialize(this, new OnInitializationCompleteListener() {
            @Override
            public void onInitializationComplete(InitializationStatus initializationStatus) {
            }
        });

        mAdView = findViewById(R.id.adView);
        AdRequest adRequest = new AdRequest.Builder().build();
        mAdView.loadAd(adRequest);
    }
}
```
  
  >Step 9.To further customize the behavior of your ad,:
```
mAdView.setAdListener(new AdListener() {
    @Override
    public void onAdClicked() {
      // Code to be executed when the user clicks on an ad.
    }

    @Override
    public void onAdClosed() {
      // Code to be executed when the user is about to return
      // to the app after tapping on an ad.
    }

    @Override
    public void onAdFailedToLoad(LoadAdError adError) {
      // Code to be executed when an ad request fails.
    }

    @Override
    public void onAdImpression() {
      // Code to be executed when an impression is recorded
      // for an ad.
    }

    @Override
    public void onAdLoaded() {
      // Code to be executed when an ad finishes loading.
    }

    @Override
    public void onAdOpened() {
      // Code to be executed when an ad opens an overlay that
      // covers the screen.
    }
});
```
    >Step 10.Load an Interstitial ad:
```
private InterstitialAd mInterstitialAd;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_main);

      MobileAds.initialize(this, new OnInitializationCompleteListener() {
        @Override
        public void onInitializationComplete(InitializationStatus initializationStatus) {}
      });
      AdRequest adRequest = new AdRequest.Builder().build();

      InterstitialAd.load(this,"ca-app-pub-3940256099942544/1033173712", adRequest,
        new InterstitialAdLoadCallback() {
      @Override
      public void onAdLoaded(@NonNull InterstitialAd interstitialAd) {
        // The mInterstitialAd reference will be null until
        // an ad is loaded.
        mInterstitialAd = interstitialAd;
        Log.i(TAG, "onAdLoaded");
      }

      @Override
      public void onAdFailedToLoad(@NonNull LoadAdError loadAdError) {
        // Handle the error
        Log.d(TAG, loadAdError.toString());
        mInterstitialAd = null;
      }
    });
  }
}
```

>Step 11.Set the FullScreenContentCallback
```
mInterstitialAd.setFullScreenContentCallback(new FullScreenContentCallback(){
  @Override
  public void onAdClicked() {
    // Called when a click is recorded for an ad.
    Log.d(TAG, "Ad was clicked.");
  }

  @Override
  public void onAdDismissedFullScreenContent() {
    // Called when ad is dismissed.
    // Set the ad reference to null so you don't show the ad a second time.
    Log.d(TAG, "Ad dismissed fullscreen content.");
    mInterstitialAd = null;
  }

  @Override
  public void onAdFailedToShowFullScreenContent(AdError adError) {
    // Called when ad fails to show.
    Log.e(TAG, "Ad failed to show fullscreen content.");
    mInterstitialAd = null;
  }

  @Override
  public void onAdImpression() {
    // Called when an impression is recorded for an ad.
    Log.d(TAG, "Ad recorded an impression.");
  }

  @Override
  public void onAdShowedFullScreenContent() {
    // Called when ad is shown.
    Log.d(TAG, "Ad showed fullscreen content.");
  }
});
```


>Step 12.Show the Interstitial ad
```
if (mInterstitialAd != null) {
  mInterstitialAd.show(MyActivity.this);
} else {
  Log.d("TAG", "The interstitial ad wasn't ready yet.");
}

```
  
