---
layout: default
title: "eggDJ: A Portable Real-Time Music Augmentation System"
tags: projects
---

# eggDJ: A Portable Real-Time Music Augmentation System

## Introduction

From time to time, I catch myself energetically tapping my fingers to the beat of my music while on the bus and the metro. For a brief moment, I become so captivated by the groove in my headphones that I completely lose awareness of my surroundings. I find myself attempting to match the kick/snare pattern with my index and middle fingers as well as embellish the drums with rhythms of my own. 

This project is an attempt to bring those moments to life. 

## Live Augmentation as Musical Practice

I would like to introduce the notion of live augmentation as a musical practice. Live augmentation, in this context (not to be confused with the music theory term of the same name describing the lengthening of a note), simply refers to adding your own flair to an existing piece of recorded music in real-time. This can be anything from layering vocal samples on top of a lofi SoundCloud house mix to practicing your jazz guitar comping by playing along with an famous jazz recording. The benefits of such practice when compared with the traditional practice of acoustic instruments are threefold: it is often a much more accessible approach to music, it can have interesting pedagogical effects, and it can breathe new life into an existing piece of music, opening up a new realm of creativity. 

Take the example of the guitarist comping along with a Miles Davis record. It is not necessarily an easy task to get, for example, a trumpet player, a bassist, and a drummer in a particular room at a particular time a) that you get along with, b) that are at a similar skill level as you, c) have similar musical goals as you, and d) without being too loud as to disturb others. Not to mention all the equipment hauling and level mixing that needs to be done. By simply playing along with a favorite record, the guitarist can often accomplish many of their musical goals much more easily. 

Additionally, many researchers in the field of Accessible Digital Musical Instruments, or ADMIs, (that seek to build musical control interfaces for persons with physical or mental disabilities) take the "augmentation" approach due to its low barrier to entry and retention of musical nuance [1].

Live augmentation can also be a very pedagogical tool. If we play along with a recording that is deemed to "have more merit" within a particular tradition of music, we can often learn more easily by engaging directly with the music as compared to passive listening. The obvious example is the jazz guitarist emulating the comping technique of say, Freddie Green, while playing along with a Count Basie record, in an attempt to improve their comping skills. By inserting their own musical creativity into the piece, the guitarist is also, by definition, creating a new piece of music that has infinite potential for innovation. 

## Design Goals

For this project, I wanted to implement a real-time music augmentation system that I can use on my way to class. This portability constraint made the project much more interesting than traditional music technology hardware, that often sits in a studio and requires a desktop computer for interaction. 

I had the following goals in mind while I designed eggDJ: 

- it had to be able to play music from any streaming service (e.g. Spotify, SoundCloud, YouTube)
- it had to have the ability to layer any digital sound with a basic sampler mechanism
- the entire system should fit in a jacket pocket wirelessly
- it should have zero visual aspect (only hearing and touch)
- no one should be able to tell what you are doing (don't wanna look weird on the bus)

## Hardware 

An initial reaction to these design goals might be to simply build an Android/iPhone app where one can trigger samples by tapping the screen. While this would be fairly straightforward, it doesn't accomplish all of the goals well because it would be difficult to interact with while not looking at it. 

I decided to use a Raspberry Pi 3 with an [Audio Injector Zero Shield](http://www.audioinjector.net/rpi-zero) to implement the hardware side. It is notoriously difficult to get audio into the Pi (a simple Google search will confirm this, PureData's website has the [best list of working soundcards](https://puredata.info/docs/raspberry-pi) that I was able to find). Even with the Audio Injector Zero, it still took several hours to figure out how to get audio in and out using the sound card. 

In order to trigger the samples, I just decided to use 5 simple pushbuttons, one for each finger. The simple control interface makes the eggDJ very intuitive with a low entry-fee. 

Unfortunately, when I tried to use the Audio Injector Shield (which takes over all 40 of the Pi's GPIO pins) in tandem with buttons attached to the GPIO pins, the Pi crashed. Thus, in order to get the pushbutton data into the Pi, I used a Teensy 3.2 interfaced via a micro-USB cable. The Teensy 3.2 has pullup resistors built-in to all 21 digital I/O pins, making it preferable compared to an Arduino Uno. 

## Software 

On the software side of things, the most straightforward way to trigger samples over an audio input on a Raspberry Pi is using Pure Data (MSP's Vanilla version). My patch implements a simple sample selection mechanism, where the user cycles through an arbitrary number (in this case, ten) of samples on a given channel (finger) by pressing the thumb button. 

My Arduino code simply writes which button was pushed to a serial stream which is read by Pd's comport object. 

## Demo

[https://www.youtube.com/watch?v=idyqRyKqyLU](https://www.youtube.com/watch?v=idyqRyKqyLU)
