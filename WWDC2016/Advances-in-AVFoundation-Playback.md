# Advances in AVFoundation Playback

> 📅 2020.6.4 (THU)
>
>🗂 WWDC2016 | Session : 503 | Category : AVKit
>
> 🔗 [https://developer.apple.com/videos/play/wwdc2016/503/](https://developer.apple.com/videos/play/wwdc2016/503/)

🔖 AVFoundation is a powerful framework for media operations, providing capture, editing, playback, and export. Learn about new APIs and methods for media playback. Create seamless loops, simplify your playback logic with "autowait", and see how to deliver an even faster playback startup experience.


### AVFoundation

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled.png)

### Kinds of Playback

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%201.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%201.png)

# Buffering

### Media Playback Over the Internet

**Playback is at the mercy of the network!**

- Start too soon → playback may stall
- Start too late → user unhappy
- Start when likely to keep up → just right

### AVPlayItem Buffering State Properties

> Existing

`**playbackLikelyToKeepUp**` : true  if you were to stop playing now, you could keep on playing without stalling until you got to the end.

`**playbackBufferFull**` : true if the buffer does as much as it's going to

`**playbackBufferEmpty**` : you are stalling or you're about to stall 

For progressive-download playback, in iOS  9

- Wait until `playbackLikelyToKeepUp` or `playbackBufferFull` before setting `AVPlayer.rate`

For HLS, rules are simpler

- Set `AVPlayer.rate` and it will automatically wait for buffering before play begins

> 🆕 AVPlayer in iOS 10, macOS Sierra, tvOS 10

Same rules for progressive and HLS

- Set `AVPlayer.rate` when user clicks play
- Automatically waits to buffer to avoid stalling

If network drops and playback stalls, playback will automatically resume when buffered

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%202.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%202.png)

**AVKit or MediaPlayer framework** → It already supports automatic waiting for buffering, and it will continue to

**AVFoundation** → You may need to make some adjustments

### → `automaticallyWaitsToMinimizeStalling`  (autoplay)

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%203.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%203.png)

### AVPlayer.rate

> Might not mean what you thought it meant

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%204.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%204.png)

### Demo

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%205.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%205.png)

When he hit play, it went into the `Waiting` state. Because playback was not yet likely to keep up.

After a few seconds, AVFoundation determined that playback was likely to keep up and so it set the state into `Playing` , and you see that the player rate and the timebase rate are both 1.

Remember the player's rate property tells you the app's desired playback rate.

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%206.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%206.png)

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%207.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%207.png)

### Cautions

- Enabled automatically if app linked on or after iOS 10, OSX 10.12, tvOS 10
    - `AVPlayer.automaticallyWaitsToMinizeStalling = true`
- Opt out if using `setRate(..., time:..., atHostTime:...)` to synchronize playback with external timeline
    - `AVPlayer.automaticallyWaitsToMinimizeStalling = false`
    - Otherwise, NSException
- Never use the player rate to project currentTime into the future
    - Use currentItem's timebase rate for that instead

---

# Looping

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%208.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%208.png)

→ Set up a listener for the notification that fires when playback has reached end. And when you get called, seek back to the beginning and start again.

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%209.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%209.png)

But unfortunately, it will lead to a gap between the playbacks.

1. There will be latency due to the time it takes for the notification to reach your program and for your second player requests to get back to the playback system
2. Time needed for prerolling . It's not actually possible to start media playback instantaneously without some preparation. This process of filling up the playback pipelines before playback starts is called preroll.

 

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2010.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2010.png)

If AVFoundation knows about playback item B but early enough, then it can begin prerolling and decoding before item A has finished playing out. → `AVQueuePlayer`

### AVQueuePlayer

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2011.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2011.png)

The current item is the one in the first position of the array. You can create multiple AVPlayer items from the same AVAsset. This is another optimization, since AVFoundation does not have to load and pause the media file multiple times.

The purpose of the play queue is to provide information about item to be played in the near future so that AVFoundation can optimize transitions. ( do not load the next 10,000 items, the play queue is not a playlist)

### The "Treadmill"

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2012.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2012.png)

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2013.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2013.png)

When you want to loop a single media file indefinitely is to make a small number of AVPlayer items and put them in the AVQueuePlayer's queue with the action item end property set to advance. 

When playback reaches the end of one item, it will be removed from  the play queue as playback advances to the next one. And when you get the notification that has happened, you can take that finished item, set its current time back to the start, and put it on the end of the play queue to **reuse** it. ⇒ Treadmill

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2014.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2014.png)

 

### 🆕 AVPlayerLooper

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2015.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2015.png)

`AVPlayerLooper` , which implements the treadmill pattern.

You give it and AVQueuePlayer and a template AVPlayerItem, and it constructs a small number of copies of that AVPlayerItem, which it then cycles through the play queue until you tell it to stop.

```swift
player = AVQueuePlayer()
playerLayer = AVPlayerLayer(player: player)
playerItem = AVPlayerItem(url: videoURL)
playerLooper = AVPlayerLooper(player: player, templateItem: playerItem)
player.play()
```

### Optimizing Movies for Looping

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2016.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2016.png)

Why? if the audio track is longer, then that means near  the end there's period of time when audio should be playing.

→ So when you build media assets for looping, take the time to make sure that the track durations match up.

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2017.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2017.png)

You could set the AVPlayerItems forward playback end time to the length of the shortest track. This will effect of trimming back the other tracks to match.

---

# Playback refinements

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2018.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2018.png)

### 🆕 Some More Smoothness

> Where once there were glitches

- Adding / Removing the only AVPlayerLayer on playing AVPlayer
- Changing subtitle language on playing AVPlayer
- Changing audio language on playing AVPlayer
- Manually disabling / enabling tracks on playing AVPlayer

→ These changes will no longer cause playback to pause.

---

# Preparing for Wide Color Video

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2019.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2019.png)

We generally represent these choices though the use of enumerated strings, since they're easier to print and display debug.

But in media files, these are represented by numbers. And these standard tag numbers are defined in an MPEG specification called coding independent code points.

🆕 Detecting Wide Color Tags

🆕 Specifying Working Color Space

🆕 Preserving Wide Color Space

🆕 Video Composition

🆕 Custom Video Compositor

---

# Best Practices for Playback

> How can I  make my videos start as fast as possible?

### Speeding Up **Local** File Playback

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2020.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2020.png)

But it still ideal to start with the information that AVFoundation needs in order to get things right first time.

### Order matters

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2021.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2021.png)

Playback pipelines don't get built until the player item becomes the current item. And by that point, the player know what it needs to get things right first time.

### Best Practice

- Configure `AVPlayer` and `AVPlayerItem` first
- Connect `AVPlayerLayer` to `AVPlayer`, or `AVPlayerItemVideoOutput` to `AVPlayerItem`
- `player.play()`
- `player.replaceCurrentItemWithPlayerItem(playItem)`

### Speeding Up HTTP Live Streaming

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2022.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2022.png)

- Retrieve master playlist : the URL you passed to AVURLAsset
- Retrieve contents keys : (If the content is protected with fair play streaming) retrieving the selected variant playlists for the appropriate bitrate and format of video and audio
- Retrieve selected variant playlist : retrieving some media sgements
- Retrieve segments : the highest amount of actual data transfer but with network IO we need to think about round-trip latency

> Preloading the Master Playlist

```swift
var asset = AVURLAsset(url: url)
asset.loadValuesAsynchrenously(forKyes: ["duration"], completionHandler: nil)
```

AVURLAsset is a lazy API. It doesn't begin loading or pausing any data until someone asks it to.

> Compress Playlists

**Compress Master Playlist and Variant Playlist with gzip**

- Your server may be able to do this for you

> Initiate key exchange earlier

```swift
var asset = AVURLAsset(url: url)
asset.resourceLoader.preloadsEligibleContentKeys = true
```

Master playlist must contain SESSION-KEY declarations

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2023.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2023.png)

> Preload segments before playback

🆕 `perferredFowardBufferDuration`

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2024.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2024.png)

### Improving Initial Quality

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2025.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2025.png)

For an iPhone SE, even with a super fast Wi-Fi connection, the 720p variant is the best choice. It's already higher resolution than the iPhone SE screen, so going bigger won't improve any quality.

> AVPlayerLayer

- Size your AVPlayerLayer appropriately and  connect it to AVPlayer early
    - Before bringing in playerItem
- Set `AVPlayerLayer.contentScale` on retina iOS devices

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2026.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2026.png)

AVFoundation's base algorithm is to pick the first applicable variant in the master playlist.

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2027.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2027.png)

There is a tradeoff you have to make between initial quality and startup time.

A higher bitrate first segment takes longer to download and that means it will take longer to start.

One way to make the tradeoff is to figure out a minium acceptable quility level you'd like to see on a particular size of creen and start there. Then AVFoudnation will switch to a higher quality after playback begins as the network allows.

**Use previous playback's statistics**

```swift
if let lastAccessLogEvent = previousPlayerItem.accessLog()?.events.last {
	lastObservedBitrate = lastAccessLogEvent.observedBitrate
}
```

### Controlling Inital Variant Selection

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2028.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2028.png)

![/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2029.png](/WWDC2016/images/Advances-in-AVFoundation-Playback/Untitled%2029.png)

## Profile Your Code Too

- Look for delays in your code, befor AVFoundation is called
- Don't wait for likelyToKeepUp notification before setting rate
(You don't need to now, and in fact, you never have for HLS)

- Make sure you release AVPlayers and AVPlayerItems from old playback sessions
(So that they do not waste bandwidth in the background)
- Use Allocation Instrument to check AVPlayer and AVPlayerItem lifespans

- Suspend other network activity in you app during network playback
(So that the user can take full advantage of available bandwidth for playback)
