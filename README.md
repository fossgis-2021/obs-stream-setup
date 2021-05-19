# OBS Streaming Setup for FOSSGIS 2021 Rapperswil
- [OBS Streaming Setup for FOSSGIS 2021 Rapperswil](#obs-streaming-setup-for-fossgis-2021-rapperswil)
- [Requirements](#requirements)
- [Setup](#setup)
  - [Import OBS Profile and Scenes](#import-obs-profile-and-scenes)
    - [Profile & Scene-Collection](#profile--scene-collection)
    - [Resources](#resources)
    - [Link to Logo and Background](#link-to-logo-and-background)
  - [Browser Source Setup](#browser-source-setup)
    - [Speaker/Moderator](#speakermoderator)
      - [Brower Source Settings](#brower-source-settings)
      - [Custom CSS](#custom-css)
  - [Audio Setup](#audio-setup)
    - [Meeting Audio Source](#meeting-audio-source)
    - [Audio Mixer](#audio-mixer)
- [Usage](#usage)
  - [How to interact](#how-to-interact)
    - [Focus individual Speaker/Moderator](#focus-individual-speakermoderator)
    - [Hide sidebar](#hide-sidebar)
    - [Get back to overview](#get-back-to-overview)

# Requirements
- OBS (version 26.1.1)

# Setup
The original idea came from the organizers of FOSSGIS and was improved upon here to allow a more seamless broadcasting experience without having to rely on window captures and computer audio. This version should also improve streaming performance for lower end hardware since there's no overhead from window caputre.

## Import OBS Profile and Scenes
In order to get the profile settings you need to download the contents of this repository either by clicking the green `Code`-Button and then Download ZIP or by using `git clone`.

### Profile & Scene-Collection
Once downloaded and extraced you should have the `obs-studio` folder and a `resource` folder. Copy the contents of the `obs-studio` folder into the obs folder of your current user by opening the Run-Dialogue with Win + R and then pasting 
```
%APPDATA%\obs-studio
```
Click "Ok" and copy the contents of `obs-studio` into the window that just opened.

### Resources
The second folder you need to copy is the `resources` folder. It currently contains the background and logo of FOSSGIS 2021. You can copy the folder to wherever you like as long as you know where you put it.

After that you can relaunch/open OBS and you should see a new profile and scene-collection in the toolbar. Select them (FOSSGIS).
<p float='left' >
    <img src="./screenshots/obs_settings_profiles.png?raw=true" width = "45%" style="margin: 5pt"/>
    <img src="./screenshots/obs_settings_scenes.png?raw=true" width = "45%" style="margin: 5pt"/>
</p>

### Link to Logo and Background
Because OBS can't handle relative paths you need to manually update the `Background` and `Logo` Source.
<img src="./screenshots/obs_sources_logo_background.png?raw=true"/>

Open the properties of the logo and click "Browse". Navigate to the location of the `resource` folder you copied previously. Select `FOSSGIS Konferenz Logo 2021_BGw.png`. Do the same for the background and select `hsr_arial_blurred.jpg`.

## Browser Source Setup
OBS introduced the function of interacting with browser sources a few releases back and this allows to setup web browser based content to be used in OBS which is perfect for our use case since we want to capture a Jitsi-Meeting.

### Speaker/Moderator
This part is a full run down on how to add a Jitsi-Meeting to OBS which isn't necessary if you already imported the `obs-studio` folder with the settings.

- insert the URL of the Jitsi-Meeting you want to capture
- set width to `1920` and height to `1080`
- make sure `Control Audio via OBS` is checked
- insert the custom CSS provided
- make sure both `Shutdown source when not visible` and `Refresh browser when scene becomes active` are off

#### Brower Source Settings
<img src="./screenshots/browser_src_settings.png?raw=true"/>

#### Custom CSS
```css
/* Default Browser Source CSS from OBS */
body {
  background-color: rgba(0, 0, 0, 0);
  margin: 0px auto;
  overflow: hidden;
}

/* Hide controls. The ones that show up on hover */
.subject.visible {
  top: -120px;
}
.new-toolbox.visible .toolbox-background {
  bottom: -160px;
}
.new-toolbox.visible {
  bottom: calc((40px * 2) * -1) !important;
}

/* Hide notifications */
.atlaskit-portal-container {
  display: none !important;
}

/* Hide Video Quality Label */
#videoResolutionLabel {
  display: none !important;
}

/* Hide Expand Arrow in the bottom right */
path[d="M16 10.688l8 8-1.875 1.875L16 14.438l-6.125 6.125L8 18.688z"] {
  display: none !important;
}
```

## Audio Setup
Since every browser source is its own audio source you can mute all individual speakers and moderators and add an additional browser source of the meeting for audio only this way you won't get duplicate audio sources.

### Meeting Audio Source
This browser source doesn't require custom CSS. However the following settings must still be applied:
- insert the URL of the Jitsi-Meeting you want to capture
- make sure `Control Audio via OBS` is checked
- make sure both `Shutdown source when not visible` and `Refresh browser when scene becomes active` are off

<img src="./screenshots/browser_src_audio.png?raw=true"/>

### Audio Mixer
In order to only have one one active audio source we have to mute the Speaker Browser Sources and instead use the Meeting Audio Source we created in the previous step.
<img src="./screenshots/browser_src_audio_mixer.png?raw=true"/>

You have to put the "Meeting Audio" Source to the bottom so you won't see it.
<p float='left' >
    <img src="./screenshots/obs_audio_setup_1.png?raw=true" width = "45%" style="margin: 5pt"/>
    <img src="./screenshots/obs_audio_setup_2.png?raw=true" width = "45%" style="margin: 5pt"/>
</p>

In order to only see whats relevant, you can hide the muted audio sources you don't use ("Speaker 1, Speaker 2, etc.).
<p float='left'>
    <img src="./screenshots/browser_src_audio_hide.png?raw=true" width = "45%" style="margin: 5pt"/>
    <img src="./screenshots/browser_src_audio_hidden.png?raw=true" width = "45%" style="margin: 5pt"/>
</p>

# Usage
In order to setup the individual Speakers/Moderators you need to interact with the browser sources. You'll need to repeat the following steps for all speakers and moderators.

## How to interact
You can interact with a browser source by right clicking it and selecting "interact" from the dropdown menu. After that you will be presented with the interact screen that allows you control the Jitsi-Meeting.
<img src="./screenshots/browser_src_interact_overview.png?raw=true"/>

### Focus individual Speaker/Moderator
By clicking the video feed of the speaker, you can highlight them.
<img src="./screenshots/browser_src_interact_focused.png?raw=true"/>

### Hide sidebar
By clicking the little arrow in the bottom right corner you can hide the side bar which contains the other participants video feeds. At this point you have a clean 1080p stream of an individual speaker.
<img src="./screenshots/browser_src_interact_focused_no_sidebar.png?raw=true"/>

### Get back to overview
When interacting with the browser source you can press "W" to get back into the overview. This way you can focus a different speaker or re-focus a speaker that dropped out of a call and joined back in.