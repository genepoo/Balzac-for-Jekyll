---
layout: post
title: Visualizing passages of music
description: null
category: articles
tags:
  - intro
  - beginner
  - jekyll
  - tutorial
image:
  feature: 'https://farm8.staticflickr.com/7014/6840341477_e49612bc59_o_d.jpg'
published: true
---


A long-standing challenge in the dissemination and study of Indian classical music has been accurate and efficient notation. The most commonly used notational system, developed by Vishnu Narayan Bhatkhande(1860-1936) resulted in the preservation of many compositions and songs that may otherwise have been lost. Nonetheless, pieces reproduced automatically from Bhatkhande notation are incomplete in that they lack many of the features that give Indian music it's characteristic sound, a quality that even greats like Leonard Bernstein have inadequately referred to as ["boing"](https://www.youtube.com/watch?v=MB7ZOdp__gQ&feature=youtu.be&t=6m22s)

A few years ago, Wim van der Meer and Suvarnalata Rao published Music in Motion, their website that documents 

```python
import essentia 
from essentia.standard import *
loader = essentia.standard.MonoLoader(filename = 'LaxmibaiBahar.mp3', sampleRate = 44100)
audio=loader()
```

```python
pExt = PredominantPitchMelodia(frameSize = 2048, hopSize = 128)
pitch, pitchConf = pExt(audio)
```

```python
time=numpy.linspace(0.0,len(audio)/44100.0,len(pitch))
output = np.column_stack((time.flatten(),pitch.flatten()))
np.savetxt('LaxmibaiBahar.txt',output,delimiter='\t')
```


