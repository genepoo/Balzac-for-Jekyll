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
  feature: teaplantations.jpg
published: true
---


A long-standing challenge in the dissemination and study of Indian classical music has been accurate and efficient notation. The most commonly used notational system, developed by Vishnu Narayan Bhatkhande(1860-1936) resulted in the preservation of many compositions and songs that may otherwise have been lost. Nonetheless, pieces reproduced automatically from Bhatkhande notation are incomplete in that they lack many of the features that give Indian music it's characteristic sound, a quality that even greats like Leonard Bernstein have inadequately referred to as ["boing"](https://www.youtube.com/watch?v=MB7ZOdp__gQ&feature=youtu.be&t=6m22s)

A few years ago, Wim van der Meer and Suvarnalata Rao published [Music in Motion](https://autrimncpa.wordpress.com/), a database of Hindustani recordings that enable viewers to visualize changes in the performer's fundamental frequency over time. They used [PRAAT](http://www.fon.hum.uva.nl/praat/), a software for phonetic research developed in Amsterdam University for melody capture. The implementation of the melody extraction algorithm in PRAAT works best for monophonic recordings containing only voice. One of the groups that addressed this shortcoming was the Music Technology Group of the [Universitat Pompeu Fabra](http://mtg.upf.edu/) in Barcelona, where Justin Salamon (now in NYU) and Emilia Gomez developed a method, Melodia, that works for polyphonic recordings containing a lead instrument or voice along with accompaniment. Both methods will sometimes give octave errors, and MTG's method sometimes suffers because of interference from accompaniment in similar frequency ranges to the lead. Luckily for the computer literate Hindustani music fan, they made their algorithm easy to use via a [VAMP plugin](http://mtg.upf.edu/technologies/melodia) usable in [Sonic Visualizer](http://www.sonicvisualiser.org/). They also made Melodia available within [Essentia](http://essentia.upf.edu/), which is an open source C++ library with Python bindings for audio information retreival.

I decided to try using Essentia to see if I could automate visualizations in the way Rao et al. did. Installation was [easy](http://essentia.upf.edu/documentation/installing.html). Essentia's extensive documentation and tutorials made implementation a breeze. Below is what I used to get time versus frequency data. 

```python
import essentia 
from essentia.standard import *
loader = essentia.standard.MonoLoader(filename = '/path/to/yourfile.mp3', sampleRate = 44100)
audio=loader()
pExt = PredominantPitchMelodia(frameSize = 2048, hopSize = 128)
pitch, pitchConf = pExt(audio) #this step could take a while
time=numpy.linspace(0.0,len(audio)/44100.0,len(pitch))
output = np.column_stack((time.flatten(),pitch.flatten()))
np.savetxt('path/to/output.txt',output,delimiter='\t')
```

To animate the results I got, I divided the frequency we observed by the tonic of each piece, also detectable using Essentia. This allowed me to view the results in the solfege space familiar to me. I reverted to R's ggplot to first generate individual frames of the time vs frequency videos. Then I used FFmpeg to stitch the images into a video, along with an mp3 of the source track to give me the result. 

```python
ffmpeg -framerate 10 -i location/of/generated/plots/tunePlot%07d.jpg -i sourceaudio.mp3 -c:v libx264 -pix_fmt yuv420p yay-video!.mp4
```