---
layout: post
title:  "Tuning VGM files for SN76489"
author: simon
categories: [ VGM ]
tags: [red, yellow]
image: simondotm/images/vgm.jpg
description: "Tuning VGM files for SN76489"
featured: false
hidden: false
---
I’ve been a long time fan of chip music. Back in the day, I used to marvel at the music being created on my old BBC Micro written by programmers and musicians with godlike talents and skill. I particularly remember a disk doing the rounds at my school that had a rendition of Eurythmics “Sweet Dreams” and the Stranglers “Golden Brown”, all delivered in glorious 3 channel square waves, and BASIC programs that contained fancy ENVELOPES and wierd looking DATA strings that were somehow being decoded fashion to create the music. Well, I thought it was brilliant at the time anyway.

In these days of plentiful storage and fast internet connections however, we are somewhat used to music being delivered as purely sampled sound, so it’s nice to see that there’s still a thriving scene for old-skool chip tunes, some of which are ripped from older games on consoles like the Sega Master System, and some that are still being freshly authored for compos and the retro demo-scene using modern chip tune trackers like Deflemask.

So I was recently impressed by a modern BBC Micro demo created by CRTC, that presented some new music, featuring pretty funky chip tunes – with fast arpeggios, smooth portamento effects, and pretty tight drum-like effects. It sounded modern and somehow better than the usual music I’d heard before on this system.

I noticed they were using a file format called VGM, so I had a little dig into this, and was somewhat inspired by what turned out to be a pretty big chip music archive scene (I’m obviously late to the party!).

Sound chips in most computers are driven like any other piece of hardware; by sending data packets that control what values to be written to the chip’s internal registers in order for some audio to be output. VGM is a file format that captures the stream of data commands for a given sound chip, so that it can be reproduced exactly – on both original hardware and emulated hardware, by simply replaying the stream of commands at the correct speed. It also supports a fairly diverse array of different sound chips.

As a format, it seems the primary goal of VGM is to enable archiving of old chip tune music, since it records the command data going to the sound chip at upto 44.1Khz and contains various carefully recorded parameters that enable VGM playback software to faithfully reproduce the music by emulating almost exactly how the original sound chip hardware worked. A number of retro computer emulators enable VGM recording of the data going to the audio chips, so it’s a great way to preserve the artform.

So while the original music data would have been stored in just a few Kilobytes, the equivalent music in VGM form will be much bigger (probably much bigger than available RAM in some of the old systems!). That said, many tunes were played on old hardware at update rates of 50Hz (PAL) or 60Hz (NTSC), so in practice, there isn’t anything like 44,100 data samples per second; it’s still fairly compact as a file format.

So I’m now interested in how to take VGM files and use them as a data format for playback on original or different hardware, simply because there’s so much great music out there that would be awesome to hear on old original or emulated hardware.🙂

# Porting VGM files to 4Mhz SN76489

Since I’m a bit of a BBC Micro geek, I thought it would be fun to have a go at getting some of the awesome music from different systems over to this machine. Well, it turned out to be quite interesting so I thought I’d share some of my adventures.

The Beeb contained a single sound chip – the Texas Instruments SN76489. It’s a primitive beastie – 3 tone channels and 1 noise channel. Square waves only. No effects. No stereo. Nothing fancy. Better than a Spectrum 1-bit beep. Not as good as the C64 SID.

Turns out though that quite a lot of hardware of this era had one of these bad boys in it – Arcade boards like Pac Man, Mr Do, consoles like the Sega Master System, Game Gear, and Neo Geo Pocket (amongst a few others).

So that means in principle, a VGM of music from one of these platforms will work ok on the BBC – same chip, same data commands, all good, right? Except not.

Turns out that the waveform you hear coming out of the SN76489 is directly influenced by a hardware clock signal being fed into it. Most of the systems above clocked their chips at 3.579545Mhz (NTSC), whereas the BBC Micro was clocked at 4Mhz.

A similar problem occurs with the PAL versions of various consoles, where the clock speed is 4.43361875 MHz.

The end result is that  any VGM originally captured on an NTSC system will need to be “re-tuned” to play back accurately on a system with a different clock speed.

For the most part this quite straightforward – it’s a matter of applying a correction to the frequencies we want the SN76489 to generate.