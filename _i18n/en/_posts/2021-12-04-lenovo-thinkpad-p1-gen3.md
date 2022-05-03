---
layout: post
title: "Lenovo ThinkPad P1 Gen3 Review (with Linux)"
date: 2021-12-04
tags: wfh workplace remote-work
comments: true
featured_image: /images/2021/12/thinkpad-p1-gen3-3.jpg
twitter: narmontase
---

I’ve used Linux for many years on servers, but still kept macOS as my main working environment for almost ten years now. A few months ago, I gave up on macOS and started using Fedora as my main operating system on the ThinkPad P1 Gen3 15".

![ThinkPad P1 Gen3](/images/2021/12/thinkpad-p1-gen3-9.jpg)

I’ve [posted this image](https://www.reddit.com/r/linuxmasterrace/comments/r2jxrj/finally_ive_made_the_switch_from_macos_to_fedora/) to Reddit *r/linuxmasterrace* and redditors had a lot of questions about the experience with this device, so I decided to share it here.

### Specs
Display: 15.6” FHD (1920 x 1080) IPS Dolby Vision HDR  
GPU: Integrated Intel UHD Graphics  
CPU: Intel Core i9-10885H (2.40 GHz, 8 Cores, 16 Threads, 16 MB Cache)  
Memory: 64 GB DDR4 2933MHz (2 x 32 GB)  
Storage: Toshiba XG6 1 TB PCIe SSD  
Operating System: Fedora Workstation 35

### My Key Takeaways
Color meanings: green - good, yellow - mixed feelings, red - bad.

🟡 Design. The ThinkPad P1 is a really beautiful looking device with a high build quality. It’s functional and minimal looking at the same time.

Chassis is made from carbon fiber and magnesium, which makes it really light and comfortable to work with.

However, it’s a fingerprint magnet. Every single touch on the laptop will leave a visible fingerprint.

![ThinkPad P1 Gen3](/images/2021/12/thinkpad-p1-gen3-2.jpg)
![ThinkPad P1 Gen3](/images/2021/12/thinkpad-p1-gen3-3.jpg)
![ThinkPad P1 Gen3](/images/2021/12/thinkpad-p1-gen3-4.jpg)
![ThinkPad P1 Gen3](/images/2021/12/thinkpad-p1-gen3-8.jpg)

🟢 Keyboard is back-lit, responsive and pleasant to type with. It's a really well designed keyboard, which gives an amazing typing experience with enough travel and a firm feel.

I don't like that it doesn't have the media control buttons, but I've mapped the F12 button as my play/pause button. Also, the Fn key is located in the very left corner, but the functionality can be swapped in the Lenovo firmware settings so it becomes the Ctrl key.

Other than that, I don’t have any issues with the keyboard.
Lenovo is well known for their world-class keyboards and this one is not an exception.

![ThinkPad P1 Gen3](/images/2021/12/thinkpad-p1-gen3-6.jpg)

🔴 TrackPad is terrible. I don't know how Lenovo never managed to actually innovate and create a decent touch-pad. For me, the experience is just awful: gestures are not working, dragging stuff is almost impossible, accuracy is very low. I am not overstating it. ThinkPad users who switched from MacBooks can also [confirm that](https://www.reddit.com/r/thinkpad/comments/snvby7/after_5_years_with_a_macbook_switched_to_a/). I only got used to the TrackPad after 3 months of usage.

Lenovo is famous for their TrackPoint (that little red thing on the keyboard), but I am not sure if I'll take the time to learn using it. Some say it's far more precise and comfortable as you don't have to lift your fingers from the keyboard. I might give it a shot sometime.

![ThinkPad P1 Gen3](/images/2021/12/thinkpad-p1-gen3-7.jpg)

🟢 Fingerprint reader. I didn’t expect it to work, but it did! It’s not as accurate as the one on the MacBook, but it’s pretty much close. It was harder to register my finger, but when I did, the experience was flawless.

I use it to unlock the laptop, confirm elevated permission prompts, unlock third party apps like 1Password and even authenticate `sudo` commands in the Terminal.

🟡 Webcam is not great, not terrible. The 720p camera produces a good enough image during my meetings.

I really like that the camera features a built-in ThinkShutter webcam privacy shutter. No more stickers, that can actually [break your screen](https://www.businessinsider.com/apple-macbook-pro-screen-break-closed-with-laptop-camera-cover-2020-7?op=1).

🟡 Speakers. A custom designed Dolby Atmos speaker system sounds good, but it’s still not as good as I would expect it to be. It’s good for work, but not for listening to music or gaming.

🟢 80 Wh battery is rated at 15 hours, but it lasts a bit less during normal usage. Anyway, the battery life is still great for such a powerful workstation:
- 7-9 hours of typing, browsing and some programming.
- 3-4 hours of video/audio conferencing or other CPU intensive tasks.

🟢 Performance is exceptional. With specs like that, I could not expect anything else.

During a really heavy load, thermal throttling limits performance, but nothing substantial. I am not really doing a lot of multi-threaded workloads, so I rarely even hear a loud fan noise during normal day-to-day usage. I don't remember ever experiencing any lag with this machine.

I think this is the strongest part of this laptop.

🟢 GPU. ThinkPad P1 supports Nvidia Quadro T1000 and T2000 4GB Max-Q cards, as well as integrated Intel UHD Graphics.

I opted for the integrated Intel UHD Graphics due to [well known issues with Nvidia](https://itsfoss.com/fix-ubuntu-freezing), which gives me a simple device without the need to deal with video drivers. On top of that, I am not planning to run anything more GPU intensive than Factorio.

Overall, I like how the Intel UHD Graphics performs. I didn't feel any lag with an external monitor connected via the USB-C port.

![ThinkPad P1 Gen3](/images/2021/10/home-office-6.jpg)

🟢 Display is configurable from the base version 15.6" FHD (1920 x 1080) IPS up to 15.6" 4K (3840 x 2160) OLED with Dolby Vision HDR.

I had to choose the 15.6" FHD (1920x1080) Dolby Vision HDR screen, because 4K options are not supported on the integrated GPU. It's a HDR400 WVA IPS display panel with 16:9 aspect ratio, 800:1 contrast ratio and 72% NTSC color gamut.

I guess it's a decent display that does the job for me, as someone who works with an external display most of the time.

![ThinkPad P1 Gen3](/images/2021/10/home-office-5.jpg)

🟢 Connectivity options are very extensive:
- 2 x USB A 3.2 Gen 1
- 2 x USB Type-C Thunderbolt 3
- 1 x HDMI 2.0
- 1 x mic / headphone combo jack
- SD card reader.

What else would you need?

![ThinkPad P1 Gen3](/images/2021/12/thinkpad-p1-gen3-5.jpg)

🟢 Expandable. If you still need better specs, no problem. Open the laptop and install new parts. No soldered memory or storage devices. [Not like the competition](/function-over-form-mounting-external-ssd-on-macbook).

![ThinkPad P1 Gen3](/images/2021/12/thinkpad-p1-gen3-1.jpg)

🟢 Linux support. Good support for Linux out of the box. I only had to install some video/audio codecs so HTML5 video would play on Firefox.

### Conclusion
Lenovo ThinkPad P1 Gen3 is a well built mobile workstation, which runs absolutely great with Linux. It’s a perfect choice for engineers who need enough computing power and a comfortable typing experience. The only major downside for me is the touch-pad.
