---
id: agora-sdk-plugin
title: Symbl-Agora Marketplace Extension
slug: /integrations/agora-sdk-plugin
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

---

This document provides information about how to build and use the Symbl Conversation Intelligence extension with the Agora SDK for Android applications. 

## Introduction
[Symbl](https://symbl.ai/) is an AI-powered Conversation Intelligence (CI) platform for developers to build and extend applications capable of understanding natural human conversations at scale. 

Our comprehensive suite of products and APIs enable developers to easily build and deploy intelligent speech-to-text functionality, extract contextual insights, generate domain-specific insights and intelligence, and access advanced conversation analytics.

With Symbl, you can analyze every call & interaction in real time as well as asynchronously to automatically surface the most important interactions that will help you identify compliance gaps, coaching opportunities, and missed opportunities to drive growth and revenue.  
 
- **Make your events accessible and inclusive**: Enable live captioning in all your events along with facilitating translation and localization in different languages based on your geographical reach. Show the current topic of discussion and important questions asked for the audience to keep face with how the conversation flows.

- **Real-time coaching**: You can take your existing contact centre experience to the next level by automatically providing real-time suggestions that a customer care agent can use during a call, enabling the agent to focus on the immediate engagement with the customer and resolve customer needs quickly thanks to task automation. This results in the ability to serve more customers with higher satisfaction during a shift.
 
- **Automated Call Summarization**: After a call, Symbl automates call disposition along with key follow-ups and questions, that shaves off a few seconds from the average call handling time and gives your agent time to focus on customer experience. 

- **Enable effective search, catch-up and further content recommendations**: Automatically index the event enabling the  search by keywords, phrases, enable late attendees to catch up to the event by showing a historic transcript with topics and last questions asked. Based on the engagement of watched events, enable future content recommendations.
 
- **Real time compliance and automation**: Easily monitor and score interactions by adding agent and customer specific analytics to your existing dashboards. Dive into deeper analysis and metadata on call statistics, agent productivity, and call quality, all of which lead to a better understanding of the priorities for the business. Also take actions based on intents, agent characteristics, answering machine detection and custom rules.

:::note Symbl analyzes the conversation data and lets you:
- **Generate Conversation Intelligence** like Sentiment Analysis, Action Items, Topics, Trackers, Summary, and much more in your applications.
- **Generate Speech-to-text capabilities** like Transcriptions, Speaker Separation, and Speaker Diarization.
- **Leverage multiple channels** such as Video, Audio, Text, and Streaming.
:::

To learn more about what is Symbl and how it works, go to our [Introduction](/docs/what-is-symbl/) page.

## Product Overview
---

This extension allows you to embed real-time conversation intelligence into your mobile application providing contextual analysis of conversation data along with speech recognition without any upfront data training requirements.

## Extension Workflow
---
Before you use this extension, make sure you activate it in the [Agora Marketplace](https://console.agora.io/). After successfully activating this extension, you can start sending audio data through your mobile application to Symbl. As a result of your application configuration and the audio data provided, you will receive the analyzed results.

## Features
---
This extension provides the following out-of-the-box conversational intelligence features:
 
- [**Speech-to-Text**](/docs/concepts/speech-to-text) capabilities converting speech from a live audio stream into text for each speaker in the meeting. This feature is also known as transcription.
- **Conversation Insights**:
  - [**Action Item**](/docs/concepts/action-items): An action item is a specific outcome recognized in the conversation that requires one or more people in the conversation to act on it in the future.
  - [**Follow-Ups**](/docs/concepts/follow-ups): This is a category of Action Items with a connotation to follow up a request or a task (e.g. sending an email or making a phone call or booking an appointment or setting up a meeting).
  - [**Questions**](/docs/concepts/questions): Explicit question or request for information that comes up during the conversation, whether answered or not, is recognized as a question.
- [**Topics**](/docs/concepts/topics): The most relevant topics of the discussion from the conversation that is generated based on the combination of the overall scope of the discussion.
- [**Sentiment Analysis**](/docs/concepts/sentiment-analysis): Sentiment analysis over each Topic that is being discussed in the conversation.
- [**Trackers**](/docs/concepts/trackers): Trackers (custom business intents) allow you to track the occurrence of certain keywords or phrases in a conversation so you can identify emerging trends and gauge the nature of interactions. You can define keywords or phrases in a Tracker and Symbl will return messages that contain the same or contextually relevant phrases.
- [**Speaker Analytics**](/docs/concepts/conversational-analytics): Provides you with functionality such as finding the speaker ratio, talk time, silence, pace, and overlap in a conversation.

## Supported Platforms
---
**Android API Level 29** or higher.

## Integration 
---
### Overview
This section describes how to integrate the Symbl Conversation Intelligence extension into your Agora based mobile application. The code samples are written in Java.

### Prerequisites
Following are some of the prerequisites of integrating Symbl-Agora extension: 

- **Agora Credentials**: Make sure you have the Agora credentials and required information (App ID, Channel Name, Temp Token and Meeting ID). This information can be retrieved from the [Agora Console](https://console.agora.io).

![agora-creds](/img/agora-console.png)

Learn more about the Agora required credentials in the [Setup Authentication](https://docs.agora.io/en/Agora%20Platform/token) page.


- **Symbl API Credentials**: You will also need the Symbl API credentials which are available on the Symbl Conversation Intelligence Extensions Marketplace product details page. 

Navigate to the [Agora Extensions Marketplace](https://console.agora.io/marketplace) and click on the Symbl [card](https://console.agora.io/marketplace/extension/introduce?serviceName=symbl).

Activate the Symbl Conversation Intelligence Extension.

![symbl-extension-activation](/img/symbl-activation-extension.png)

After activating the Symbl Conversation Intelligence Extension, click the **View** button under the **Credentials** column to retrieve the required Symbl credentials (App ID and App Secret).

![symbl-creds-agora](/img/access_symbl_creds_on_agora_dashoboard.png)

- **Android Mobile Application**: This guide assumes you have an Android mobile application with the Agora Video SDK enabled. Follow the steps [here](https://docs.agora.io/en/Video/start_call_android?platform=Android) to set it up.

### Integration Steps
---

This section walks you through the steps necessary to set up the Symbl Conversation Intelligence extension in your mobile application.

1. Download the [Symbl Extension](https://cdn-agora.symbl.ai/agora-symblai-filter-debug.aar) (if you haven't already).
 
2. Add the `.aar` file as a dependency to your application. 
![agora-creds](/img/agora-arr-files.png)

3. Add the following information into your `build.gradle` module file:

```js
implementation fileTree(include: ['*.jar'], dir: 'libs')
implementation 'com.squareup.okhttp3:okhttp:3.10.0'
implementation 'org.java-websocket:Java-WebSocket:1.5.1'
```
4. Implement the interface io.agora.rtc2.IMediaExtensionObserver
 
```js
public class MainActivity extends AppCompatActivity implements io.agora.rtc2.IMediaExtensionObserver {
```

5. Add the following method to set all the necessary information to initialize the Symbl configuration. You can find description for the parameters used in the table below:

```js
private void setSymblPluginConfigs(JSONObject pluginParams) throws JSONException {
   try {
       // Symbl main extension config objects
       SymblPluginConfig symblParams = new SymblPluginConfig();

       // Set the Symbl API Configuration
       ApiConfig apiConfig = new ApiConfig();
       apiConfig.setAppId("<symbl_app_id>");
       apiConfig.setAppSecret("<symbl_app_secret>");
       apiConfig.setTokenApi("https://api.symbl.ai/oauth2/token:generate");
       apiConfig.setSymblPlatformUrl("api-agora-1.symbl.ai");
       symblParams.setApiConfig(apiConfig);

       // Set the Symbl Confidence Level and Language Code
       RealtimeStartRequest realtimeFlowInitRequest = new RealtimeStartRequest();
       RealtimeAPIConfig realtimeAPIConfig = new RealtimeAPIConfig();
realtimeAPIConfig.setConfidenceThreshold(Double.parseDouble("<symbl_confidence_threshold>"));
realtimeAPIConfig.setLanguageCode("en-US");

       // Set the Speaker information
       Speaker speaker=new Speaker();
       speaker.setName("<symbl_meeting_user_Name>");
       speaker.setUserId("<symbl_meeting_UserId>");
       realtimeFlowInitRequest.setSpeaker(speaker);

       // Set the meeting encoding and speaker sample rate hertz
       SpeechRecognition speechRecognition = new SpeechRecognition();
       speechRecognition.setEncoding("<symbl_meeting_encoding>"));
speechRecognition.setSampleRateHertz(Double.parseDouble("<symbl_meeting_sampleRateHertz>"));
       realtimeAPIConfig.setSpeechRecognition(speechRecognition);

       // Set the redaction content values
       Redaction redaction = new Redaction();
       redaction.setIdentifyContent(true);
       redaction.setRedactContent(true);
       redaction.setRedactionString("*****");
       realtimeAPIConfig.setRedaction(redaction);

       // Set the Tracker (custom business intent) information
       realtimeFlowInitRequest.setConfig(realtimeAPIConfig);
       Tracker tracker1 = new Tracker();
       tracker1.setName("Budget");
       List<String> vocabulary = new ArrayList<>();
       vocabulary.add("budget");
       vocabulary.add("budget decision");
       tracker1.setVocabulary(vocabulary);
       List<Tracker> trackerList = new ArrayList<>();
       trackerList.add(tracker1);

       // Set the Symbl conversation parameters
       realtimeFlowInitRequest.setTrackers(trackerList);
       realtimeFlowInitRequest.setType("start_request");
       realtimeFlowInitRequest.setId("<symbl_unique_meetingId>");
       realtimeFlowInitRequest.setSentiments(true);
       realtimeFlowInitRequest.setInsightTypes(Arrays.asList("action_item", "question", "follow_up"));
       symblParams.setRealtimeStartRequest(realtimeFlowInitRequest);
       Gson gson = new Gson();
       pluginParams.put("inputRequest", gson.toJson(symblParams));
   } catch (Exception ex) {
       Log.e(TAG, "ERROR while setting Symbl extension configuration");
   }
}
``` 
 
The following table lists the parameters and their descriptions used in the sample code above:

| Parameter | Description |
| ---- | ------ |
| `symbl_meeting_UserId` | Used to identify the user in the real-time meeting.
| `symbl_meeting_user_Name` | The name of the user attending the real-time meeting.
| `symbl_unique_meetingId` | Unique identifier for the meeting.
| `symbl_platform_url` | The dedicated URL for the Symbl platform. Use `api-agora-1.symbl.ai`.
| `symbl_app_id` | The Symbl App ID.
| `symbl_app_secret` | The Symbl App Secret.
| `symbl_meeting_language_code` | The language code. Currently, en-US (English US) is the only language supported.
| `symbl_token_api` | The URL for secure token generation.
| `symbl_meeting_encoding` | The audio encoding in which the audio will be sent over the WebSocket connection.
| `symbl_meeting_sampleRateHertz` | The rate of the incoming audio stream.
| `symbl_confidence_threshold` | The minimum confidence score that you can set for an API to consider it as valid insight. The minimum confidence score should be in the range <=0.5 to <=1.0 (greater than or equal to 0.5 and less than or equal to 1.0.). Default value is 0.5.

You would have to call the method above during the initialization of the Agora Engine in your application.
 
On your **onEvent()** method, you should receive all the transcription information and the analyzed conversation events where you can parse the JSON responses.
 
For a basic Speech-to-Text (recognition result) type of response, you should expect the following payload:
```js
{
 "type": "recognition_result",
 "isFinal": true,
 "payload": {
   "raw": {
     "alternatives": [{
       "words": [{
         "word": "Hello",
         "startTime": {
           "seconds": "3",
           "nanos": "800000000"
         },
         "endTime": {
           "seconds": "4",
           "nanos": "200000000"
         }
       }, {
         "word": "world.",
         "startTime": {
           "seconds": "4",
           "nanos": "200000000"
         },
         "endTime": {
           "seconds": "4",
           "nanos": "800000000"
         }
       }],
       "transcript": "Hello world.",
       "confidence": 0.9128385782241821
     }]
   }
 },
 "punctuated": {
   "transcript": "Hello world"
 },
 "user": {
   "userId": "emailAddress",
   "name": "John Doe",
   "id": "23681108-355b-4fc3-9d94-ed47dd39fa56"
 }
}
```
 
For a Topic type of response, you should expect the following payload:

```js
[{
 "id": "e69a5556-6729-XXXX-XXXX-2aee2deabb1b",
 "messageReferences": [{
   "id": "0df44422-0248-47e9-8814-e87f63404f2c",
   "relation": "text instance"
 }],
 "phrases": "auto insurance",
 "rootWords": [{
   	"text": "auto"
 }],
 "score": 0.9,
 "type": "topic"
}]
```

For a Tracker type of response, you should expect the following payload:

```js
{
 "type": "tracker_response",
 "trackers": [{
   "name": "budget",
   "matches": [{
     "type": "vocabulary",
     "value": "budget",
     "messageRefs": [{
       "id": "0f3275d7-d534-48e2-aee2-685bd3167f4b",
       "text": "I would like to set a budget for this month.",
       "offset": 22
     }],
     "insightRefs": []
    }]
 }],
 "sequenceNumber": 0
}
```

Given below is a complete sample of `MainActivity.java` class which includes the Agora RTC and Symbl settings.

```js
package agoramarketplace.symblai.cai;
import androidx.annotation.NonNull;
import androidx.annotation.RequiresApi;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;
import android.Manifest;
import android.content.Context;
import android.content.pm.PackageManager;
import android.os.Build;
import android.os.Bundle;
import android.util.Log;
import android.view.SurfaceView;
import android.view.View;
import android.view.inputmethod.InputMethodManager;
import android.widget.Button;
import android.widget.EditText;
import android.widget.FrameLayout;
import android.widget.TextView;

import com.google.gson.Gson;
import io.agora.extension.symblai.model.request.ApiConfig;
import io.agora.extension.symblai.model.request.RealtimeAPIConfig;
import io.agora.extension.symblai.model.request.RealtimeStartRequest;
import io.agora.extension.symblai.model.request.Redaction;
import io.agora.extension.symblai.model.request.Speaker;
import io.agora.extension.symblai.model.request.SpeechRecognition;
import io.agora.extension.symblai.model.request.SymblPluginConfig;
import io.agora.extension.symblai.model.request.Tracker;
import io.agora.extension.symblai.model.response.SymblResponse;
import org.json.JSONException;
import org.json.JSONObject;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.UUID;
import io.agora.extension.symblai.ExtensionManager;
import io.agora.extension.symblai.SymblAIFilterManager;
import io.agora.rtc2.Constants;
import io.agora.rtc2.IRtcEngineEventHandler;
import io.agora.rtc2.RtcEngine;
import io.agora.rtc2.RtcEngineConfig;
import io.agora.rtc2.UserInfo;
import io.agora.rtc2.video.VideoCanvas;
import io.agora.rtc2.video.VideoEncoderConfiguration;

public class MainActivity extends AppCompatActivity implements io.agora.rtc2.IMediaExtensionObserver {

   private final static String TAG = "Agora_SymblTag java :";
   private static final int PERMISSION_REQ_ID = 22;
   // Set Agora platform configuration
   private final static String AGORA_CUSTOMER_CHANNEL_NAME = "Demo";
   private static final String AGORA_CUSTOMER_APP_ID = "XXXXXXXXXX";
   public static final String TOKEN_VALUE = "XXXXXXXXXX";
   public static final String AGORA_MEETING_ID = "Sample Meeting";
   // Set Symbl configuration in the code or optionally in "strings.xml"
   public static final String symbl_meeting_UserId = "user@mail.com";
   public static final String symbl_meeting_user_Name = "userName";
   private FrameLayout localVideoContainer;
   private FrameLayout remoteVideoContainer;
   private RtcEngine mRtcEngine;
   private SurfaceView mRemoteView;
   private TextView infoTextView;
   private Button wmButton;
   private EditText et_watermark;
   private static final String[] REQUESTED_PERMISSIONS = {
           Manifest.permission.RECORD_AUDIO,
           Manifest.permission.CAMERA
   };

   @RequiresApi(api = Build.VERSION_CODES.M)
   @Override
   protected void onCreate(Bundle savedInstanceState) {
       Log.d(TAG, "onCreate");
       super.onCreate(savedInstanceState);
       setContentView(R.layout.activity_main);
       initUI();
       checkPermission();
       infoTextView.setText("Symbl Sample Conversation Application");
       wmButton.setEnabled(true);
   }

   @Override
   protected void onDestroy() {
       Log.d(TAG, "onDestroy");
       super.onDestroy();
       mRtcEngine.leaveChannel();
       mRtcEngine.destroy();
   }

   @RequiresApi(api = Build.VERSION_CODES.M)
   private void checkPermission() {
       Log.d(TAG, "checkPermission");
       if (checkSelfPermission(REQUESTED_PERMISSIONS[0], PERMISSION_REQ_ID) &&
               checkSelfPermission(REQUESTED_PERMISSIONS[1], PERMISSION_REQ_ID)) {
           initAgoraEngine();
       }
   }

   @Override
   public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
       super.onRequestPermissionsResult(requestCode, permissions, grantResults);
       if (requestCode == PERMISSION_REQ_ID && grantResults.length > 0 &&
               grantResults[0] == PackageManager.PERMISSION_GRANTED) {
           initAgoraEngine();
       }
   }

   private void initUI() {
       localVideoContainer = findViewById(R.id.view_container);
       remoteVideoContainer = findViewById(R.id.remote_video_view_container);
       infoTextView = findViewById(R.id.infoTextView);
       et_watermark = findViewById(R.id.et_watermark);
       wmButton = findViewById(R.id.enable_button);
       wmButton.setOnClickListener(new View.OnClickListener() {
           @Override
           public void onClick(View v) {
               InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
               imm.hideSoftInputFromWindow(et_watermark.getWindowToken(), 0);
               if (wmButton.isSelected()) {
                   wmButton.setSelected(false);
                   wmButton.setText(R.string.string_enable);
                   disableEffect();
               } else {
                   wmButton.setSelected(true);
                   wmButton.setText(R.string.string_disable);
                   enableEffect();
               }
           }
       });
   }

   private boolean checkSelfPermission(String permission, int requestCode) {
       if (ContextCompat.checkSelfPermission(this, permission) !=
               PackageManager.PERMISSION_GRANTED) {
           ActivityCompat.requestPermissions(this, REQUESTED_PERMISSIONS, requestCode);
           return false;
       }
       return true;
   }

   private void initAgoraEngine() {
       try {
           RtcEngineConfig config = new RtcEngineConfig();
           config.mContext = this;
           config.mAppId = AGORA_CUSTOMER_APP_ID;
           config.addExtension(ExtensionManager.EXTENSION_NAME);
           config.mExtensionObserver = this;
           config.mEventHandler = new IRtcEngineEventHandler() {
               @Override
               public void onJoinChannelSuccess(String s, int i, int i1) {
                   Log.d(TAG, "on Join Channel Success");
                   mRtcEngine.startPreview();
                   JSONObject pluginParams = new JSONObject();
                   try {
                       setSymblPluginConfigs(pluginParams, AGORA_CUSTOMER_CHANNEL_NAME);
       mRtcEngine.setExtensionProperty(ExtensionManager.EXTENSION_VENDOR_NAME, ExtensionManager.EXTENSION_FILTER_NAME, "init", pluginParams.toString());
                   } catch (JSONException e) {
                       Log.d(TAG, "ERROR while joining channel " + e.getMessage());
                   }
               }

               @Override
               public void onFirstRemoteVideoDecoded(final int i, int i1, int i2, int i3) {
                   super.onFirstRemoteVideoDecoded(i, i1, i2, i3);
                   runOnUiThread(new Runnable() {
                       @Override
                       public void run() {
                           setupRemoteVideo(i);
                       }
                   });
               }

               @Override
               public void onUserJoined(int i, int i1) {
                   super.onUserJoined(i, i1);
               }

               @Override
               public void onUserOffline(final int i, int i1) {
                   super.onUserOffline(i, i1);
                   runOnUiThread(new Runnable() {
                       @Override
                       public void run() {
                           onRemoteUserLeft();
                       }
                   });
               }
           };
           mRtcEngine = RtcEngine.create(config);
           mRtcEngine.enableExtension(ExtensionManager.EXTENSION_VENDOR_NAME, ExtensionManager.EXTENSION_FILTER_NAME, true);
           setupLocalVideo();
           VideoEncoderConfiguration configuration = new VideoEncoderConfiguration(640, 360,
                   VideoEncoderConfiguration.FRAME_RATE.FRAME_RATE_FPS_24,
                   VideoEncoderConfiguration.STANDARD_BITRATE,
  VideoEncoderConfiguration.ORIENTATION_MODE.ORIENTATION_MODE_FIXED_PORTRAIT);
           mRtcEngine.setVideoEncoderConfiguration(configuration);
           mRtcEngine.setClientRole(Constants.AUDIO_ENCODED_FRAME_OBSERVER_POSITION_MIC);
           mRtcEngine.enableLocalAudio(true);
           mRtcEngine.setEnableSpeakerphone(true);
           mRtcEngine.setAudioProfile(1);
           mRtcEngine.enableAudio();
           mRtcEngine.addHandler(new IRtcEngineEventHandler() {
               @Override
               public void onUserInfoUpdated(int uid, UserInfo userInfo) {
                   super.onUserInfoUpdated(uid, userInfo);
               }
               @Override
               public void onUserJoined(int uid, int elapsed) {
                   super.onUserJoined(uid, elapsed);
               }
               @Override
               public void onUserOffline(int uid, int reason) {
                   super.onUserOffline(uid, reason);
               }
               @Override
               public void onConnectionStateChanged(int state, int reason) {
                   super.onConnectionStateChanged(state, reason);
               }
               @Override
               public void onConnectionInterrupted() {
                   super.onConnectionInterrupted();
               }
               @Override
               public void onConnectionLost() {
                   super.onConnectionLost();
               }
               @Override
               public void onApiCallExecuted(int error, String api, String result) {
                   super.onApiCallExecuted(error, api, result);
               }
               @Override
               public void onTokenPrivilegeWillExpire(String token) {
                   super.onTokenPrivilegeWillExpire(token);
               }
               @Override
               public void onRequestToken() {
                   super.onRequestToken();
               }
               @Override
               public void onActiveSpeaker(int uid) {
                   super.onActiveSpeaker(uid);
               }
               @Override
               public void onFirstLocalAudioFramePublished(int elapsed) {
                   super.onFirstLocalAudioFramePublished(elapsed);
               }
               @Override
               public void onAudioPublishStateChanged(String channel, STREAM_PUBLISH_STATE oldState, STREAM_PUBLISH_STATE newState, int elapseSinceLastState) {
                   super.onAudioPublishStateChanged(channel, oldState, newState, elapseSinceLastState);
               }
               @Override
               public void onAudioSubscribeStateChanged(String channel, int uid, STREAM_SUBSCRIBE_STATE oldState, STREAM_SUBSCRIBE_STATE newState, int elapseSinceLastState) {
                   super.onAudioSubscribeStateChanged(channel, uid, oldState, newState, elapseSinceLastState);
               }
               @Override
               public void onAudioRouteChanged(int routing) {
                   super.onAudioRouteChanged(routing);
               }
               @Override
               public void onAudioQuality(int uid, int quality, short delay, short lost) {
                   super.onAudioQuality(uid, quality, delay, lost);
               }
               @Override
               public void onRtcStats(RtcStats stats) {
                   super.onRtcStats(stats);
               }
               @Override
               public void onLocalAudioStats(LocalAudioStats stats) {
                   super.onLocalAudioStats(stats);
               }
               @Override
               public void onAudioMixingFinished() {
                   super.onAudioMixingFinished();
               }

               @Override
               public void onLocalAudioStateChanged(LOCAL_AUDIO_STREAM_STATE state, LOCAL_AUDIO_STREAM_ERROR error) {
                   super.onLocalAudioStateChanged(state, error);
               }
               @Override
               public void onEncryptionError(ENCRYPTION_ERROR_TYPE errorType) {
                   super.onEncryptionError(errorType);
               }
               @Override
               public void onPermissionError(PERMISSION permission) {
                   super.onPermissionError(permission);
               }
           });
           mRtcEngine.joinChannel(TOKEN_VALUE, AGORA_CUSTOMER_CHANNEL_NAME, AGORA_MEETING_ID, 0);
           mRtcEngine.startPreview();
       } catch (Exception e) {
           e.printStackTrace();
           Log.e(TAG, " ERROR:: RTC engine startup error " + e.getMessage());
       }
   }

   private void setupLocalVideo() {
       SurfaceView view = RtcEngine.CreateRendererView(this);
       view.setZOrderMediaOverlay(true);
       localVideoContainer.addView(view);
       mRtcEngine.setupLocalVideo(new VideoCanvas(view, VideoCanvas.RENDER_MODE_HIDDEN, 0));
       mRtcEngine.setLocalRenderMode(Constants.RENDER_MODE_HIDDEN);
   }

   private void setupRemoteVideo(int uid) {
       // Only one remote video view is available for this sample application.
       int count = remoteVideoContainer.getChildCount();
       View view = null;
       for (int i = 0; i < count; i++) {
           View v = remoteVideoContainer.getChildAt(i);
           if (v.getTag() instanceof Integer && ((int) v.getTag()) == uid) {
               view = v;
           }
       }

       if (view != null) {
           return;
       }

       Log.d(TAG, " setupRemoteVideo uid = " + uid);
       mRemoteView = RtcEngine.CreateRendererView(getBaseContext());
       remoteVideoContainer.addView(mRemoteView);
       mRtcEngine.setupRemoteVideo(new VideoCanvas(mRemoteView, VideoCanvas.RENDER_MODE_HIDDEN, uid));
       mRtcEngine.setRemoteRenderMode(uid, Constants.RENDER_MODE_HIDDEN);
       mRemoteView.setTag(uid);
   }

   private void onRemoteUserLeft() {
       removeRemoteVideo();
   }

   private void removeRemoteVideo() {
       if (mRemoteView != null) {
           remoteVideoContainer.removeView(mRemoteView);
       }
       mRemoteView = null;
   }

   private void enableEffect() {
       JSONObject pluginParams = new JSONObject();
       try {
           setSymblPluginConfigs(pluginParams, AGORA_CUSTOMER_CHANNEL_NAME);
           mRtcEngine.setExtensionProperty(ExtensionManager.EXTENSION_VENDOR_NAME, ExtensionManager.EXTENSION_FILTER_NAME, "start", pluginParams.toString());
       } catch (JSONException e) {
           e.printStackTrace();
       }
   }

   /**
    * This method is used to set the Symbl configuration
    *
    * @param pluginParams
    * @param channelName
    * @throws JSONException
    */
   private void setSymblPluginConfigs(JSONObject pluginParams, String channelName) throws JSONException {
       try {
           String symbl_unique_meetingId = UUID.randomUUID().toString();
           pluginParams.put("secret", getString(R.string.symbl_app_secret));
           pluginParams.put("appKey", getString(R.string.symbl_app_id));

           pluginParams.put("meetingId", symbl_unique_meetingId);

           pluginParams.put("userId", symbl_meeting_UserId);
           pluginParams.put("name", symbl_meeting_user_Name);
           pluginParams.put("languageCode", "en-US");

           // Symbl main extension config objects
           SymblPluginConfig symplParams = new SymblPluginConfig();

           // Set the Symbl API Configuration
           ApiConfig apiConfig = new ApiConfig();
           apiConfig.setAppId(getString(R.string.symbl_app_id));
           apiConfig.setAppSecret(getString(R.string.symbl_app_secret));
           apiConfig.setTokenApi(getString(R.string.symbl_token_api));
           apiConfig.setSymblPlatformUrl(getString(R.string.symbl_platform_url));
           symplParams.setApiConfig(apiConfig);
          
          // Set the Symbl Confidence Level and Language Code
           RealtimeStartRequest realtimeFlowInitRequest = new RealtimeStartRequest();
           RealtimeAPIConfig realtimeAPIConfig = new RealtimeAPIConfig();
           realtimeAPIConfig.setConfidenceThreshold(Double.parseDouble(String.valueOf(R.string.symbl_confidence_threshold)));
           realtimeAPIConfig.setLanguageCode(getString(R.string.symbl_meeting_language_code));
         
		   // Set the Speaker information
           Speaker speaker=new Speaker();
           speaker.setName(symbl_meeting_user_Name);
           speaker.setUserId(symbl_meeting_UserId);
           realtimeFlowInitRequest.setSpeaker(speaker);
        
		  // Set the meeting encoding and speaker sample rate hertz
           SpeechRecognition speechRecognition = new SpeechRecognition();
           speechRecognition.setEncoding(getString(R.string.symbl_meeting_encoding));
           speechRecognition.setSampleRateHertz(Double.parseDouble(getString(R.string.symbl_meeting_sampleRateHertz)));
           realtimeAPIConfig.setSpeechRecognition(speechRecognition);
           
		   // Set the redaction content values
           Redaction redaction = new Redaction();
           redaction.setIdentifyContent(true);
           redaction.setRedactContent(true);
           redaction.setRedactionString("*****");
           realtimeAPIConfig.setRedaction(redaction);
          
		  // Set the Tracker (custom business intent) information
           realtimeFlowInitRequest.setConfig(realtimeAPIConfig);
           Tracker tracker1 = new Tracker();
           tracker1.setName("Budget");
           List<String> vocabulary = new ArrayList<>();
           vocabulary.add("budgeted");
           vocabulary.add("budgeted decision");
           tracker1.setVocabulary(vocabulary);
           List<Tracker> trackerList = new ArrayList<>();
           trackerList.add(tracker1);
           
		   // Set the Symbl conversation parameters
           realtimeFlowInitRequest.setTrackers(trackerList);
           realtimeFlowInitRequest.setType("start_request");
           realtimeFlowInitRequest.setId(symbl_unique_meetingId);
           realtimeFlowInitRequest.setSentiments(true);
           realtimeFlowInitRequest.setInsightTypes(Arrays.asList("action_item", "question", "follow_up"));
           symplParams.setRealtimeStartRequest(realtimeFlowInitRequest);
           Gson gson = new Gson();
           pluginParams.put("inputRequest", gson.toJson(symplParams));
       } catch (Exception ex) {
           Log.e(TAG, "ERROR while setting Symbl extension configuration");
       }
   }

   private void disableEffect() {
       JSONObject o = new JSONObject();
       mRtcEngine.setExtensionProperty(ExtensionManager.EXTENSION_VENDOR_NAME, ExtensionManager.EXTENSION_FILTER_NAME, "stop", o.toString());
   }

   @Override
   public void onEvent(String vendor, String extension, String key, String value) {
       Log.i(TAG, "Symbl conversation Event \n \n  " + vendor + "  extension: " + extension + "  key: " + key + "  value: " + value);
       final StringBuilder sb = new StringBuilder();
       sb.append(value);
       if ("result".equals(key)) {
           try {
               Gson json = new Gson();
               SymblResponse symblResponse = json.fromJson(value, SymblResponse.class);
               if(symblResponse.getEvent()!=null && symblResponse.getEvent().length()>0) {
                   switch (symblResponse.getEvent()) {
                       //this is the conversation response from Symbl platform
                       case SymblAIFilterManager.SYMBL_START_PLUGIN_REQUEST:
                           break;
                       case SymblAIFilterManager.SYMBL_ON_MESSAGE:
                           try {
                               if (symblResponse.getErrorMessage() != null && symblResponse.getErrorMessage().length() > 0) {
                               }else {
                               }
                           }catch (Exception ex) {
                               Log.e(TAG, "ERROR on Symbl message on message transform error "+ex.getMessage());
                           }
                           break;
                       case SymblAIFilterManager.SYMBL_CONNECTION_ERROR:
                           if(symblResponse!=null)
                               Log.i(TAG, "SYMBL_CONNECTION_ERROR error code %s , error message "+symblResponse.getErrorCode());
                           break;
                       case SymblAIFilterManager.SYMBL_WEBSOCKETS_CLOSED:
                           if(symblResponse!=null)
                           Log.i(TAG, "SYMBL_CONNECTION_ERROR "+symblResponse.getErrorCode());
                           break;
                       case SymblAIFilterManager.SYMBL_TOKEN_EXPIRED:
                           break;
                       case SymblAIFilterManager.SYMBL_STOP_REQUEST:
                           break;
                       case SymblAIFilterManager.SYMBL_ON_CLOSE:
                           break;
                       case SymblAIFilterManager.SYMBL_SEND_REQUEST:
                           break;
                       case SymblAIFilterManager.SYMBL_ON_ERROR:
                           break;
                   }
               }else{// all error cases handle here
                   if(symblResponse!=null) {
                       sb.append("\n Symbl event :" + symblResponse.getEvent());
                       sb.append("\n Error Message :" + symblResponse.getErrorMessage());
                       sb.append("\n Error code :" + symblResponse.getErrorCode());
                   }
               }

           } catch (Exception exception) {
               System.out.println("result parse error ");
           }
       }
       this.runOnUiThread(new Runnable() {
           @Override
           public void run() {
               infoTextView.setText(sb.toString());
           }
       });
   }
}
```
### API Reference
---

Find comprehensive information about our REST APIs in the [API Reference](https://docs.symbl.ai/docs/api-reference/getting-started) section.
