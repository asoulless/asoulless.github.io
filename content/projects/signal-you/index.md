+++
title = "Signal-You"
description = "A Material You fork of a fork of Signal"

weight = 0

[extra]
accent_color = "#555a92"
accent_color_dark = "#bec2ff"

banner = "banner.png"

archive = "This Signal fork has been archived, and I am no longer maintaining it."
+++
<div class="buttons">
  <a class="colored external" href="https://github.com/asoulless/signal-you">See the code</a>
</div>

*Signal-You* was a fork of <a class="external" href="https://github.com/johanw666/Signal-Android">johanw666's fork</a> of the Android Signal app that replaced the colors used with the *Material You* ones introduced in Android 12. The idea originally came from <a class="external" href="https://github.com/johanw666/Signal-Android/issues/67">an issue on the upstream fork asking for a black theme for the app</a>, despite the maintainer saying that they weren't going to add it back due to the code for the UI being changed too frequently.

I originally wanted to try it out because the person who made the issue pointed out that it was as simple as changing some XML values, so the bar for actually trying that out became much lower for me. One thing led to another, and a version of the app that fulfilled the request of the user turned into a fork with fully working Material You!

However, as time went on, johanw666's complaints about the code proved to be correct, and the team eventually started moving to use Kotlin and Jetpack Compose for their UI (at least, that's what I think they're using now), and one issue after another came in about the inconsistent theme. Eventually, I decided to just retire the project, since I didn't have a desire to try and learn the two to try and fix the issue.

Thankfully, Signal-You lives on in spirit thanks to <a class="external" href="https://molly.im">Molly</a>, a hardened fork of the Android app. They were actually able to theme more parts of the app than I was able to, and they even asked me for feedback on it! If you still want a Material You version of Signal, check out the app if you're on Android.