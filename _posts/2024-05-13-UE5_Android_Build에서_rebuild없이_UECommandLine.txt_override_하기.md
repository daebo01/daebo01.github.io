---
title: UE5 Android Build에서 rebuild없이 UECommandLine.txt override 하기
date: 2024-05-13 11:39:00 +0900
categories:
- Unreal Engine 5
tags:
- UE5
---

Android Build를 뽑았을 때 UECommandLine.txt는 apk안 assets 폴더 안에 있다.

![apk](/assets/attachments/2024-05-13-UE5_Android_Build에서_rebuild없이_UECommandLine.txt_override_하기/img1.png)

만일 apk안에서 UECommandLine.txt을 수정한다면 apk에 signing을 다시 해야할 것이고, 에디터에서 수정한다면 다시 빌드 및 설치해야하므로 번거롭다.

에픽에서는 안드로이드 외부 저장소의 개별 앱 공간 특정 경로안에서 UECommandLine.txt를 먼저 찾고, 없으면 apk안 UECommandLine.txt를 찾도록 만들어두었다.

안드로이드 저장소에 관해서는 [링크](https://velog.io/@eia51/Android-%EC%A0%80%EC%9E%A5%EC%86%8C-%EA%B4%80%EB%A6%AC) 를 참조하면 좋다.

```java
// /Engine/Build/Android/Java/src/com/epicgames/unreal/GameActivity.java.template 
// 에서 UECommandLine.txt 를 찾아서 읽는 부분

// ...

	public void ParseCommandline(String ProjectName, boolean bUseExternalFilesDir)
	{
		// ...

		// bUseExternalFilesDir는 AndroidManifest.xml에 meta-data tag중 com.epicgames.unreal.GameActivity.bUseExternalFilesDir의 값으로 세팅된다. 기본값은 true인듯? 

		BufferedReader reader = null;

		// determine the proper place to look for UECommandLine.txt as override outside APK
		// sdcard 경로 다만 android 10부터 이용할 수 없다.
		String BaseDirectory = android.os.Environment.getExternalStorageDirectory().getAbsolutePath();
		if (bUseExternalFilesDir)
		{
			// 개별 앱 공간 {sdcardpath}/Android/data/{app package name}
			BaseDirectory = getExternalFilesDir(null).getAbsolutePath();
			if (nativeIsShippingBuild())
			{
				BaseDirectory = getFilesDir().getAbsolutePath();
			}
		}

		// first look for an override in the project directory
		String filename = BaseDirectory + "/UnrealGame/" + ProjectName + "/UECommandLine.txt";
		reader = TryOpenFileReader(filename);
		if (reader == null)
		{
			filename = BaseDirectory + "/UnrealGame/" + ProjectName + "/uecommandline.txt";
			reader = TryOpenFileReader(filename);
		}

		// then look in assets in APK
		if (reader == null)
		{
			try
			{
				InputStream stream = AssetManagerReference.open("UECommandLine.txt");
				reader = new BufferedReader(new InputStreamReader(stream));
				Log.debug("Using APK commandline");
			}
			catch (FileNotFoundException fe)
			{
			}
			catch (IOException ie)
			{
			}
		} else {
			Log.debug("Using commandline from: " + filename);
		}

		// ...
	}

// ...
```

코드를 보면 {BaseDirectory}/UnrealGame/{ProjectName}/UECommandLine.txt를 먼저 찾는 것을 알 수 있다. 

BaseDirectory는 안드로이드 버전이나 기기에 따라 다를 수 있지만 대체로 /storage/emulated/0/Android/data/{apk package name}/files 이 될 것 이다.


![storage](/assets/attachments/2024-05-13-UE5_Android_Build에서_rebuild없이_UECommandLine.txt_override_하기/img2.png)

해당 경로에 UECommandLine.txt 파일을 복사해준다.

![log](/assets/attachments/2024-05-13-UE5_Android_Build에서_rebuild없이_UECommandLine.txt_override_하기/img3.png)

로그에서 commandline이 변경된 걸 확인할 수 있다.
